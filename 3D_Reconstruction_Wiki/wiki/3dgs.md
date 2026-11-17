# 3D Gaussian Splatting (3DGS) 核心概念與技術原理

本頁面系統性介紹 **3D 高斯潑濺（3D Gaussian Splatting, 3DGS）** 的技術核心、演算法機制、渲染架構與能力邊界。本頁知識完全依據開創性論文 [Kerbl et al. (2023)](../evidence/kerbl2023-3dgs.md) 之事實紀錄撰寫。

---

## 1. 什麼是 3D Gaussian Splatting？

### 1.1 從 NeRF 到 3DGS：顯式與隱式的典範轉移
在多視角 3D 重建與新視角合成（NVS）領域，傳統 [NeRF (Mildenhall et al., 2020)](nerf.md) 使用神經網絡（MLP）隱式表徵連續場景。雖然視覺品質逼真，但由於每次渲染都必須沿著相機視線進行上百次神經網絡前向推論，導致**渲染速度極為緩慢（每幀數秒至數十秒）**。

[Kerbl et al. (2023)](../evidence/kerbl2023-3dgs.md#2-研究目的與核心技術貢獻) 提出了 **3D Gaussian Splatting（3DGS）**，實現了三大範式革新：
1. **顯式表示（Explicit Representation）**：放棄神經網絡權重，改用數十萬至數百萬個具備空間分佈的 **3D 高斯橢球點集** 來直接描述物體與場景外觀。
2. **自適應密度控制（Adaptive Density Control）**：演算法在最佳化過程中動態「長出」或「分裂」新的高斯，並修剪無效的高斯，精確捕捉微小細節。
3. **即時軟體光柵化（Real-time Tile-based Rasterization）**：透過硬體友善的分塊（Tile-based）排序與 $\alpha$-blending 累積，在單張 GPU 上實現了 **100+ FPS** 的即時新視角渲染 [Kerbl et al., 2023, Table 1](../evidence/kerbl2023-3dgs.md#62-table-1-評估數據保持原生解析度ours-30ksec-72-table-1-p-8)。

```
[多視角影像輸入] 
      │ (COLMAP 初始化稀疏點雲)
      ▼
[初始化 3D 高斯集合] 
      │ ◄────────────────────────────────────────┐
      ▼                                          │ (反向傳播)
[視角投影至 2D 影像平面] (EWA Splatting)          │
      │                                          │
      ▼                                          │
[Tile-Based 快速光柵化排序與混合]                │
      │                                          │
      ▼                                          │
[合成 2D 影像與真值計算 Loss] ───────────────────┘
      │
      ▼ (每 100 次迭代)
[自適應密度控制：Clone / Split / Pruning]
```

---

## 2. 核心數學模型與物理幾何意義

### 2.1 3D 高斯的幾何表徵
在 3D 空間中，一個 3D Gaussian 的空間密度分佈由其中心均值 $\mu \in \mathbb{R}^3$ 與協方差矩陣 $\Sigma \in \mathbb{R}^{3 \times 3}$ 決定 [Kerbl et al., 2023, Eq. 4](../evidence/kerbl2023-3dgs.md#31-3d-高斯數學定義與協方差矩陣分解-sec-4-p-4)：

$$G(x) = \exp\left(-\frac{1}{2}(x - \mu)^T \Sigma^{-1} (x - \mu)\right)$$

物理上，$\mu$ 代表該橢球在三維空間的中心坐標，而 $\Sigma$ 則決定了橢球在各個方向的拉伸長度與空間旋轉方向。

### 2.2 協方差矩陣半正定分解（$RSS^T R^T$）
直接對一般矩陣進行梯度下降最佳化極難保證其數學上的**半正定性（Positive Semi-Definite）**。為了解決這個問題，3DGS 將協方差矩陣分解為縮放矩陣 $S$ 與旋轉矩陣 $R$ [Kerbl et al., 2023, Eq. 6](../evidence/kerbl2023-3dgs.md#31-3d-高斯數學定義與協方差矩陣分解-sec-4-p-4)：

$$\Sigma = R S S^T R^T$$

- **尺度向量（Scale vector）$s = (s_x, s_y, s_z)$**：透過指數映射保證其半徑恆為正實數。
- **旋轉四元數（Quaternion）$q = (q_r, q_i, q_j, q_k)$**：四元數在運算時自動正規化為單位長度，構成標準正交旋轉矩陣 $R$。

### 2.3 2D 投影協方差矩陣（EWA Splatting）
當相機從特定視角觀察場景時，3D 高斯被投影至 2D 影像平面，其 2D 協方差矩陣 $\Sigma'$ 定義為 [Kerbl et al., 2023, Eq. 5](../evidence/kerbl2023-3dgs.md#32-2d-投影協方差矩陣-sec-4-p-4)：

$$\Sigma' = J W \Sigma W^T J^T$$

- $W$：相機的外參矩陣（世界坐標轉相機坐標）。
- $J$：射影相機投影的 Jacobian 仿射近似矩陣。

### 2.4 體積渲染混合（$\alpha$-Blending）
在投影為 2D 橢圓分佈後，每個像素的色彩由深度由前至後的所有高斯加權混合合成 [Kerbl et al., 2023, Eqs. 1–3](../evidence/kerbl2023-3dgs.md#33-體積渲染混合公式tile-based-alpha-blending-sec-3-p-3)：

$$C = \sum_{i \in \mathcal{N}} c_i \alpha_i \prod_{j=1}^{i-1} (1 - \alpha_j)$$

- $\alpha_i = o_i \cdot G^{2D}_i(p)$：高斯的實際透明度貢獻，等於其學習不透明度 $o_i \in [0, 1]$ 與 2D 投影高斯概率評估值的乘積。
- $c_i$：顏色係數，使用球諧函數（Spherical Harmonics, SH）參數化，允許外觀顏色隨觀察角度變化（View-dependent Radiance）。

### 2.5 最佳化損失函數
最佳化目標結合 L1 像素絕對誤差與結構不相似度損失項（D-SSIM）[Kerbl et al., 2023, Eq. 7](../evidence/kerbl2023-3dgs.md#34-最佳化損失函數-sec-4-eq-7-p-5)：

$$\mathcal{L} = (1 - \lambda)\mathcal{L}_1 + \lambda \mathcal{L}_{\text{D-SSIM}}, \quad \lambda = 0.2$$

---

## 3. 自適應密度控制演算法（Adaptive Density Control）

為解決初始化點雲分佈不均勻或幾何欠缺的問題，3DGS 在訓練過程中每隔 100 次迭代執行自適應密度控制 [Kerbl et al., 2023, Sec. 5.2 & Appendix B](../evidence/kerbl2023-3dgs.md#41-兩階段密度控制流程-sec-52-p-5-及-appendix-b-algorithm-1-p-13)。

### 3.1 兩階段密度控制機制
1. **第一層門檻（密度控制觸發條件）**：
   - 測量視角空間位置梯度（View-space positional gradient）的模長：
     $$\|\nabla_p \mathcal{L}\| > \tau_p$$
   - 閾值設定：$\tau_p = 0.0002$。若梯度過大，代表該高斯在目前視角下的重建誤差顯著，需要進行密集化。
2. **第二層門檻（Algorithm 1 第 865 行之 Split / Clone 判斷）**：
   - 判斷式依據協方差矩陣 $\Sigma$ 的模長：
     $$\|\Sigma\| > \tau_S$$
   - **若 $\|\Sigma\| > \tau_S$**：判定為 **過度重建（Over-reconstruction）**。代表單一高斯覆蓋範圍過大跨越多個外觀結構，執行 **`SplitGaussian`**：將其分裂為兩個較小的高斯，尺度除以 $\phi = 1.6$，並依原高斯分佈抽樣新中心位置。
   - **若 $\|\Sigma\| \le \tau_S$**：判定為 **欠重建（Under-reconstruction）**。代表幾何細節處高斯過小或數量不足，執行 **`CloneGaussian`**：直接複製該高斯並沿著位置梯度方向平移以填補空白。
   - *(說明：原始論文未提供 $\tau_S$ 具體固定數值，亦未指定矩陣範數種類，此處如實記載為原論文實作細節)*。

### 3.2 透明度相關控制數值辨析
論文中涉及數個透明度相關參數，各自具備獨立物理功能 [Kerbl et al., 2023, Appendix B, C](../evidence/kerbl2023-3dgs.md#42-透明度相關控制值之明確區隔-sec-52-及-appendix-b-c)：
- **$\epsilon_\alpha$**：低不透明度 Gaussian 修剪門檻（Pruning Threshold）。若高斯透明度過低則將其直接刪除，以控制記憶體與算力開銷（數值未在論文正文公開）。
- **$\epsilon = 1/255$**：光柵化器跳過極小 alpha 更新的數值穩定門檻。
- **$0.99$**：單一高斯 alpha 值的上限截斷常數（Clamp upper bound）。
- **$0.9999$**：前向混合累積不透明度飽和終止門檻（Early Saturation），當累積透射率降至極低時直接停止計算後方高斯。

---

## 4. Tile-Based 可見性感知光柵化架構

3DGS 渲染速度超越 NeRF 的關鍵在於專屬設計的 GPU 軟體光柵化器 [Kerbl et al., 2023, Appendix C](../evidence/kerbl2023-3dgs.md#5-tile-based-可見性感知光柵化器實作規格-appendix-c-p-13)：
1. **螢幕分塊（Tile Partitioning）**：將影像切分為 $16 \times 16$ 像素的小區塊（Tiles）。
2. **實例化（Instantiation）**：若一個投影高斯覆蓋多個 Tile，則在各 Tile 內各配置一個實例。
3. **64-bit 排序 Key**：
   - **低 32 位元**：編碼投影深度（Projected Depth）。
   - **高位元**：編碼對應的 Tile 索引。
4. **單次 GPU 基底排序（Radix Sort）**：利用 NVIDIA CUB 的單次排序，全螢幕的高斯實例自然依 Tile 及深度嚴格排序，避免逐像素排序的高昂成本。
5. **早停機制（Early Saturation）**：每個 Tile 平行執行 $\alpha$-blending，當累積不透明度達到 0.9999 時立即終止，省去後續不可見高斯的計算。

---

## 5. 實驗評估與重要數據

### 5.1 實驗條件與硬體
- **測試硬體**：3DGS 的主要實驗全數在單張 **NVIDIA A6000 GPU** 上執行 [Kerbl et al., 2023, Sec. 7.2](../evidence/kerbl2023-3dgs.md#61-實驗硬體環境-sec-72-p-8-及-appendix-d)。
- **Baseline 對照差異**：Mip-NeRF360 在原文中採用 4-GPU A100 節點執行 12 小時（合計 48 GPU-hours，A100 運算效能優於 A6000）。

### 5.2 Table 1 核心指標數據（Ours-30K）
在保持原生輸入解析度的條件下，3DGS 於三大公開真實場景基準上的表現如下 [Kerbl et al., 2023, Table 1](../evidence/kerbl2023-3dgs.md#62-table-1-評估數據保持原生解析度ours-30ksec-72-table-1-p-8)：

| 資料集 | PSNR (dB) $\uparrow$ | SSIM $\uparrow$ | LPIPS $\downarrow$ | 訓練時間 (Train) | 渲染速度 (FPS) | 最佳化模型參數儲存容量 (Mem) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Mip-NeRF360** | 27.21 | 0.815 | 0.214 | 41m41s | 134 | 734 MB |
| **Tanks & Temples** | 23.14 | 0.841 | 0.183 | 26m54s | 154 | 411 MB |
| **Deep Blending** | **29.41** | **0.903** | **0.243** | **36m02s** | **137** | **676 MB** |

> [!NOTE]
> - **Mem 的定義**：Table 1 的 Mem 統一稱為「**最佳化模型參數的儲存容量（model parameter storage）**」，代表儲存數百萬個高斯屬性所需之容量，不可混同為 GPU 顯存或硬碟檔案大小。
> - **FPS 差異說明**：Tanks & Temples 在少迭代（Ours-7K）時可達 197 FPS；Deep Blending 在充分最佳化（Ours-30K）下為 137 FPS、676 MB。

### 5.3 論文定量評估範圍區隔
- **Table 1**：真實場景新視角合成品質（PSNR, SSIM, LPIPS）、訓練時間、FPS 與最佳化模型參數儲存容量。
- **Table 2**：Synthetic NeRF 合成資料集的新視角合成 PSNR 評估 [Kerbl et al., 2023, Sec. 7.2](../evidence/kerbl2023-3dgs.md#63-定量評估範圍之區隔-sec-7)。
- **Table 3**：演算法組件消融實驗（Ablation Study）PSNR 比較 [Kerbl et al., 2023, Sec. 7.3](../evidence/kerbl2023-3dgs.md#63-定量評估範圍之區隔-sec-7)。

---

## 6. 能力邊界與品質評估警示

### 6.1 視覺逼真 $\neq$ 幾何形狀正確（最重要認知盲點）
> [!WARNING]
> **P2 原論文未進行任何 3D 幾何真值距離指標評估**：
> - 3DGS 論文的所有定量指標（PSNR, SSIM, LPIPS）均為 **2D 投影影像品質指標**。
> - 原論文**完全沒有直接使用倒角距離（Chamfer Distance, CD）或 F-score 等 3D 幾何真值距離指標** 來驗證 3D 空間形狀的正確性 [Kerbl et al., 2023, Sec. 7](../evidence/kerbl2023-3dgs.md#7-研究限制與未評估事項dod-重要防偽邊界)。
> - 3DGS 模型由數十萬至數百萬個具備透明度的高斯橢球組成，在未經表面提取（如 Poisson Reconstruction 或 Marching Tetrahedra）前，**它不是封閉的三角網格（Mesh），不保證表面的水密性與幾何精準度**。

### 6.2 顯存開銷與極限視角限制
1. **訓練顯存峰值（Peak Training VRAM）**：未優化的 PyTorch 原型在密集場景訓練時，計算峰值顯存需求可能超過 **20 GB** [Kerbl et al., 2023, Sec. 7.4](../evidence/kerbl2023-3dgs.md#64-訓練與推論之-gpu-顯存開銷-sec-74-p-11)。
2. **推論光柵化額外顯存**：光柵化器在推論時需額外消耗約 **30–500 MB** 顯存以管理 Tile 分塊與排序暫存。
3. **未見視角偽影（Floaters / Elongated Artifacts）**：若相機視角大幅偏離訓練拍攝路徑，高斯橢球可能顯露出不自然被拉長的邊界。

---

## 7. 知識來源追溯

- 本頁技術數據與公式完全源自：[`evidence/kerbl2023-3dgs.md`](../evidence/kerbl2023-3dgs.md)
- 跨方法條件比較：參見 [`wiki/evaluation-comparison.md`](evaluation-comparison.md)
- 視覺品質指標詳細定義：參見 [`wiki/visual-quality.md`](visual-quality.md)
