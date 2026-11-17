# 論文結構化證據卡：P2 - Kerbl et al. (2023) 3D Gaussian Splatting

本文件為 **P2 論文（3D Gaussian Splatting）** 之單一事實來源證據卡（SSOT Evidence Card）。所有收錄之數據、公式、章節、頁碼及限制條件均經原始文獻 `raw/P2_3DGS_Kerbl_SIGGRAPH2023.pdf` 核實，為後續 `wiki/` 頁面與教學教材之事實依據。

---

## 1. 文獻基本資料與官方來源（Source Verification）

- **正式論文標題**：*3D Gaussian Splatting for Real-Time Radiance Field Rendering*
- **簡稱引用**：Kerbl et al. (2023)
- **正式作者完整名單**：
  1. Bernhard Kerbl<sup>*</sup> (Inria, Université Côte d'Azur)
  2. Georgios Kopanas<sup>*</sup> (Inria, Université Côte d'Azur)
  3. Thomas Leimkühler (Max-Planck-Institut für Informatik)
  4. George Drettakis (Inria, Université Côte d'Azur)
  *(註：`* indicates equal contribution`，前兩位作者為共同第一作者)*
- **出版來源**：ACM Transactions on Graphics (TOG), Volume 42, Issue 4, Article 139, pp. 1–14 (Proceedings of ACM SIGGRAPH 2023)
- **出版年份**：2023 年（Publication date: August 2023）
- **官方專屬 Article DOI**：[10.1145/3592433](https://doi.org/10.1145/3592433) *(特別注意：專屬 Article DOI 為 10.1145/3592433，勿與期號 DOI 10.1145/3609020 混淆)*
- **官方研究專案頁面**：https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/
- **本地原始檔案保存**：`raw/P2_3DGS_Kerbl_SIGGRAPH2023.pdf`（24,343,676 位元組，14 頁完整版，含 Appendix A–C，符合 `.gitignore` 規範不入 Git）

---

## 2. 研究目的與核心技術貢獻

### 2.1 欲解決之痛點
傳統神經輻射場（NeRF, Mildenhall et al., 2020）利用連續神經網絡（MLP）隱式表徵體積輻射場，合成新視角時需沿射線進行密集採樣與神經網絡推論，導致渲染速度極慢（每秒數秒至數十秒一張），且訓練需數小時至數天，阻礙了即時互動應用的發展 [Sec. 1, p. 1]。

### 2.2 三大核心創新貢獻 [Sec. 1, p. 2]
1. **顯式 3D 高斯場景表徵（Explicit 3D Gaussians）**：以離散、具備協方差與體積透明度的 3D 高斯橢球點集表徵場景，免除神經網絡隱式前向計算。
2. **自適應密度控制演算法（Adaptive Density Control）**：在最佳化迭代中交替進行修剪（Pruning）、複製（Clone）與分裂（Split），自動調整高斯的數量與幾何分佈。
3. **快速可見性感知光柵化器（Fast Visibility-Aware Tile-Based Rasterizer）**：結合螢幕分塊（Tile-based）與 GPU 基底排序（Radix Sort），實現支援各向異性 Splatting 與快速反向傳播的即時渲染。

---

## 3. 核心數學模型與物理意義

### 3.1 3D 高斯數學定義與協方差矩陣分解 [Sec. 4, p. 4]
在 3D 空間中，以均值 $\mu \in \mathbb{R}^3$（位置）與協方差矩陣 $\Sigma \in \mathbb{R}^{3 \times 3}$ 定義的 3D Gaussian 為（Eq. 4）：
$$G(x) = \exp\left(-\frac{1}{2}(x - \mu)^T \Sigma^{-1} (x - \mu)\right)$$

為確保最佳化過程中 $\Sigma$ 恆為半正定矩陣（Positive Semi-Definite），將其分解為旋轉矩陣 $R$ 與縮放矩陣 $S$（Eq. 6）：
$$\Sigma = R S S^T R^T$$
- **縮放向量**：以 3D 尺度向量 $s = (s_x, s_y, s_z)$ 表示三個軸向尺度，最佳化時經由指數函數變換保證為正數。
- **旋轉四元數**：以四元數 $q = (q_r, q_i, q_j, q_k)$ 表示旋轉，並在運算中正規化至單位長度。

### 3.2 2D 投影協方差矩陣 [Sec. 4, p. 4]
依據 Zwicker et al. (2001) 的 EWA Splatting 局部仿射近似，3D 高斯投影至 2D 影像平面的協方差矩陣 $\Sigma'$ 定義為（Eq. 5）：
$$\Sigma' = J W \Sigma W^T J^T$$
- $W$：世界坐標系至相機坐標系的視角變換矩陣。
- $J$：射影幾何在視線中心切空間的 Jacobian 仿射近似矩陣。

### 3.3 體積渲染混合公式（Tile-Based $\alpha$-Blending）[Sec. 3, p. 3]
像素顏色由重疊的高斯沿深度由前至後進行 $\alpha$-blending 累積合成（Eq. 1, 2, 3）：
$$C = \sum_{i \in \mathcal{N}} c_i \alpha_i \prod_{j=1}^{i-1} (1 - \alpha_j)$$
其中：
$$\alpha_i = o_i \cdot G^{2D}_i(p) = o_i \cdot \exp\left(-\frac{1}{2}(p - \mu'_i)^T (\Sigma'_i)^{-1} (p - \mu'_i)\right)$$
- $o_i \in [0, 1]$：經 Sigmoid 映射後的學習不透明度（Opacity）。
- $c_i$：由球諧函數（Spherical Harmonics, SH）係數與觀測視角計算出的顏色。

### 3.4 最佳化損失函數 [Sec. 4, Eq. 7, p. 5]
結合 L1 損失與結構不相似度損失項（D-SSIM）：
$$\mathcal{L} = (1 - \lambda)\mathcal{L}_1 + \lambda \mathcal{L}_{\text{D-SSIM}}, \quad \lambda = 0.2$$
- 說明：$\mathcal{L}_{\text{D-SSIM}}$ 為結構不相似度損失項（D-SSIM），本專案保留原始論文定義，不額外推導論文未明確提供的獨立公式。

---

## 4. 自適應密度控制與透明度參數（Adaptive Density Control）

### 4.1 兩階段密度控制流程 [Sec. 5.2, p. 5 及 Appendix B Algorithm 1, p. 13]
密度控制每隔 100 次迭代觸發一次：
1. **第一層門檻（密度控制觸發條件）**：
   - 檢驗視角空間位置梯度的平均模長：
     $$\|\nabla_p \mathcal{L}\| > \tau_p$$
   - 論文公布固定閾值：$\tau_p = 0.0002$。
2. **第二層門檻（Algorithm 1 第 865 行之 Split / Clone 判斷式）**：
   - 原始論文正確判斷式為：
     $$\|\Sigma\| > \tau_S$$
     其中 $\Sigma$ 為 Gaussian 的協方差矩陣。
   - **若 $\|\Sigma\| > \tau_S$**：判定為 Over-reconstruction（過度重建/高變異區域），執行 **`SplitGaussian`**。將該 Gaussian 一分為二，尺度縮小除以 $\phi = 1.6$，並以原高斯作為機率密度函數（PDF）抽樣初始化新高斯位置。
   - **若 $\|\Sigma\| \le \tau_S$**：判定為 Under-reconstruction（幾何欠缺區域），執行 **`CloneGaussian`**。複製該 Gaussian 並沿著位置梯度方向平移以填補空白。
   - *重要限制說明*：$\tau_S$ 的具體數值未由論文明確提供；論文亦未明確說明此處使用的矩陣範數種類，嚴禁自行指定或臆測。

### 4.2 透明度相關控制值之明確區隔 [Sec. 5.2 及 Appendix B, C]
1. **$\epsilon_\alpha$**：低不透明度 Gaussian 修剪門檻（Pruning Threshold，Algorithm 1 第 861 行 `if α < ϵ_α`，正文未公布固定絕對數值）。
2. **$\epsilon = 1/255$**：光柵化器前向混合時跳過極小 alpha 更新的數值穩定門檻（Appendix C）。
3. **$0.99$**：Appendix C 中單一高斯 alpha 值的上限截斷常數（Clamp upper bound）。
4. **$0.9999$**：前向累積不透明度飽和終止門檻（Early Saturation in Appendix C，當累積透射率降至極低時終止後續高斯遍歷）。

---

## 5. Tile-Based 可見性感知光柵化器實作規格 [Appendix C, p. 13]

- **畫面分塊**：將螢幕分割為 $16 \times 16$ 像素的獨立分塊（Tiles / Bins）。
- **實例化**：若 2D 投影高斯覆蓋多個 Tile，則在每個 Tile 內各建立一個實例。
- **64-bit 排序 Key**：
  - **低 32 位元（Lower 32 bits）**：編碼投影深度（Projected Depth）。
  - **高位元（Higher bits）**：編碼重疊的 Tile 索引（Tile Index）。
- **GPU 基底排序**：呼叫 NVIDIA CUB GPU Radix Sort 進行單次全域排序，同一 Tile 內的 Gaussian 自然依深度排列，完全免除逐像素排序負擔。

---

## 6. 實驗資料集、評估指標與重要數據

### 6.1 實驗硬體環境 [Sec. 7.2, p. 8 及 Appendix D]
- **3DGS 評估硬體**：所有主要實驗均在單張 **NVIDIA A6000 GPU** 上執行。
- **Mip-NeRF360 Baseline 對照**：原文使用 **4-GPU A100 節點** 執行 12 小時（合計 48 GPU-hours，A100 效能顯著高於 A6000）。

### 6.2 Table 1 評估數據（保持原生解析度，Ours-30K）[Sec. 7.2, Table 1, p. 8]
> [!NOTE]
> Table 1 的 **Mem** 統一稱為「**最佳化模型參數的儲存容量（model parameter storage）**」，指最佳化後的模型參數儲存大小，不可混稱為訓練 VRAM 或硬碟檔案大小。

| 資料集 | PSNR (dB) $\uparrow$ | SSIM $\uparrow$ | LPIPS $\downarrow$ | 訓練時間 (Train) | 渲染速度 (FPS) | 模型參數儲存容量 (Mem) |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Mip-NeRF360** (Ours-30K) | 27.21 | 0.815 | 0.214 | 41m41s | 134 | 734 MB |
| **Tanks & Temples** (Ours-30K) | 23.14 | 0.841 | 0.183 | 26m54s | 154 | 411 MB |
| **Deep Blending** (Ours-30K) | **29.41** | **0.903** | **0.243** | **36m02s** | **137** | **676 MB** |

*(核實備註：Tanks & Temples Ours-7K 為 197 FPS；Deep Blending Ours-30K 為 137 FPS、676 MB，數據核驗正確無誤)*

### 6.3 定量評估範圍之區隔 [Sec. 7]
- **Table 1**：真實場景新視角合成品質（PSNR, SSIM, LPIPS）、訓練時間、FPS 與最佳化模型參數儲存容量（Mem）。
- **Table 2**：Synthetic NeRF 資料集的新視角合成 PSNR 評估 [Sec. 7.2, p. 9]。
- **Table 3**：各項技術組件之消融實驗（Ablation Study）PSNR 比較 [Sec. 7.3, p. 9]。

### 6.4 訓練與推論之 GPU 顯存開銷 [Sec. 7.4, p. 11]
- **訓練峰值顯存**：在大場景未優化的 PyTorch 原型實作中，計算峰值 GPU 顯存需求可能超過 **20 GB**。
- **推論光柵化額外顯存**：推論時光柵化器需額外消耗約 **30–500 MB** 顯存以管理 Tile 分塊與排序暫存。

---

## 7. 研究限制與未評估事項（DoD 重要防偽邊界）

1. **未進行 3D 幾何真值距離評估**：
   - 論文**全數聚焦於 2D 新視角影像品質與渲染幀率**，**未直接使用 Chamfer Distance、F-score 等 3D 幾何真值距離指標驗證重建品質**。
   - 渲染逼真（高 PSNR）絕對不代表 3D 空間形狀正確。
2. **非三角網格曲面**：
   - 輸出為非結構化 3D 高斯橢球集合，不具備拓撲水密性，無法直接導出無缺陷的 CAD/Mesh。
3. **極限視角或未見視角偽影**：
   - 在相機視角大幅偏離訓練路徑時，容易出現伸長的高斯條狀偽影（Elongated artifacts / Floaters）。

---

## 8. 來源章節與頁碼追溯索引

- 核心貢獻與動機：Sec. 1, pp. 1–2
- 體積渲染混合公式：Sec. 3, Eqs. 1–3, p. 3
- 3D 高斯表徵與協方差分解：Sec. 4, Eqs. 4–6, p. 4
- 損失函數：Sec. 4, Eq. 7, p. 5
- 自適應密度控制：Sec. 5.2, p. 5 及 Appendix B Algorithm 1, p. 13
- 軟體光柵化器與 64-bit 排序 Key：Appendix C, p. 13
- Table 1, 2, 3 定量數據與 A6000 硬體：Sec. 7.2–7.3, pp. 8–9
- 訓練峰值顯存與推論開銷：Sec. 7.4, p. 11
