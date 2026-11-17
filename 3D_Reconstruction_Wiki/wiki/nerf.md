# 神經輻射場（Neural Radiance Fields, NeRF）

> **核心定位**：NeRF 是一種連續體積場景表示法（Continuous Volumetric Scene Representation）與新視角影像合成（Novel View Synthesis, NVS）技術，**其本質並非直接輸出傳統 3D 幾何網格（Mesh）的幾何重建方法** [Mildenhall et al., 2020, Sec 1, 3](../evidence/mildenhall2020-nerf.md#2-研究問題與核心目的)。

---

## 1. 初學者的直觀理解：什麼是神經輻射場？

想像一下，傳統的 3D 模型就像用樂高積木（體素，Voxel）堆疊，或者用一層薄薄的紙糊成多邊形外殼（三角網格，Mesh）。

**NeRF 採取了一種完全不同的思維**：
它不在硬碟中存儲任何頂點或體素方塊，而是**訓練一個多層感知機（MLP）神經網路，把整個 3D 場景「記」在神經網路的權重參數裡**。

當你想知道空間中「某個特定座標 $(x,y,z)$ 是空無一物還是有物體阻擋、從某個角度看是什麼顏色」時，你只需要把這個 5D 座標輸入神經網路，神經網路就會像一個連續函數一樣，告訴你該點的**不透明度（體積密度）**與**顏色** [Mildenhall et al., 2020, Sec 3, p. 408](../evidence/mildenhall2020-nerf.md#31-5d-連續場景表示sec-3-p-408409)。

---

## 2. 核心架構與運作機制

```
[ 5D 輸入座標 ]
  空間位置: (x, y, z) ──> [位置編碼 L=10] ──> [8 層 MLP 網路 (256 通道)] ──> 體積密度 σ (與視角無關)
                                                        │ (提取 256 維特徵)
  觀察方向: (θ, ϕ)   ──> [位置編碼 L=4]  ───────────────┴──> [1 層額外 FC (128 通道)] ──> 發射顏色 c = (r, g, b)
```

### 2.1 5D 輸入座標與多視角一致性約束
- **輸入**：3D 空間位置 $\mathbf{x} = (x,y,z)$ 與 2D 觀察視線方向 $(\theta, \phi)$（實務上轉為 3D 單位方向向量 $\mathbf{d}$）。
- **輸出**：體積密度 $\sigma$（Volume Density）與視角相關輻射顏色 $\mathbf{c} = (r,g,b)$。
- **多視角一致性約束（Multiview Consistency）**：
  - **體積密度 $\sigma$ 只由位置 $\mathbf{x}$ 決定**：一個物體在空間中的實體形狀與不透明度，不應隨著人眼觀察的角度改變。
  - **發射顏色 $\mathbf{c}$ 同時由位置 $\mathbf{x}$ 與視線方向 $\mathbf{d}$ 決定**：這使 NeRF 能逼真地表現出金屬、反光漆面等**非朗伯體表面（Non-Lambertian）隨角度變化的光澤與高光** [Mildenhall et al., 2020, Sec 3, p. 408–409](../evidence/mildenhall2020-nerf.md#31-5d-連續場景表示sec-3-p-408409)。

### 2.2 位置編碼（Positional Encoding）
深度神經網路天然存在「頻譜偏差（Spectral Bias）」，極度偏好學習低頻平滑的函數。如果直接將 $(x,y,z)$ 傳入神經網路，NeRF 只能渲染出模糊一片的影像。

為此，NeRF 引入高頻正弦與餘弦波將座標投影至高維特徵空間 [Mildenhall et al., 2020, Sec 5.1, Eq. 4, p. 412](../evidence/mildenhall2020-nerf.md#33-位置編碼positional-encoding-sec-51-eq-4-p-412)：

$$\gamma(p) = \left( \sin(2^0 \pi p), \cos(2^0 \pi p), \dots, \sin(2^{L-1} \pi p), \cos(2^{L-1} \pi p) \right)$$

- 位置 $\mathbf{x}$ 使用 $L=10$ 個頻率（映射至 60 維）。
- 方向 $\mathbf{d}$ 使用 $L=4$ 個頻率（映射至 24 維）。
- **消融實驗證明**：若移除位置編碼，PSNR 會自 31.01 驟降至 28.77，物體表面的銳利幾何邊緣與精細幾何細節將完全丟失 [Mildenhall et al., 2020, Table 2, p. 418](../evidence/mildenhall2020-nerf.md#43-消融實驗數據table-2-p-418)。

### 2.3 分層體積採樣（Hierarchical Volume Sampling）
如果沿著相機射線均勻採樣，大部分的計算資源都會浪費在空無一物的空白區域（$\sigma \approx 0$）。NeRF 設計了一套分層採樣機制 [Mildenhall et al., 2020, Sec 5.2, p. 412–413](../evidence/mildenhall2020-nerf.md#34-分層體積採樣hierarchical-volume-sampling-sec-52-eq-5-p-412413)：

1. **粗糙網路（Coarse Network）**：在相機射線上採樣 $N_c = 64$ 個分層隨機點，評估初步渲染權重：
   $$w_i = T_i \left(1 - \exp(-\sigma_i \delta_i)\right)$$
2. **建構分段常數 PDF**：將權重正規化為 $\hat{w}_i = w_i / \sum_{j=1}^{N_c} w_j$，形成沿射線的 **分段常數機率密度函數（Piecewise-constant PDF）**。
   - *觀念辨析*：此 PDF 代表沿射線各區間對最終渲染顏色的預期貢獻程度，並非物體存在的直接幾何機率。
3. **精細網路（Fine Network）**：透過逆變換採樣（Inverse Transform Sampling）依照該 PDF 額外採樣 $N_f = 128$ 個點（高權重區域獲得更多採樣點）。
4. **聯合評估**：精細網路最終在原本的 64 點加上新增的 128 點，**共計 $N_c + N_f = 192$ 個位置進行評估**，並據此合成最終高解析度影像色彩。

---

## 3. 重要評估公式與物理意義

### 3.1 體積渲染連續積分與離散求積
- **連續體積渲染積分（Eq. 1, p. 409）**：
  $$C(\mathbf{r}) = \int_{t_n}^{t_f} T(t) \sigma(\mathbf{r}(t)) \mathbf{c}(\mathbf{r}(t), \mathbf{d}) \, dt, \quad \text{其中 } T(t) = \exp\left(-\int_{t_n}^t \sigma(\mathbf{r}(s)) \, ds\right)$$
  - $T(t)$ 代表光線從相機近平面 $t_n$ 穿透至 $t$ 未遭遇任何阻擋的**累積透射率（Transmittance）**。
- **離散求積合成公式（Eq. 3, p. 410）**：
  $$\hat{C}(\mathbf{r}) = \sum_{i=1}^N T_i \left(1 - \exp(-\sigma_i \delta_i)\right) \mathbf{c}_i, \quad \text{其中 } T_i = \exp\left(-\sum_{j=1}^{i-1} \sigma_j \delta_j\right)$$
  - $\delta_i = t_{i+1} - t_i$ 為相鄰採樣點步進距離。
  - $(1 - \exp(-\sigma_i \delta_i))$ 為局部不透明度 $\alpha_i$。此公式將光線微積分轉化為標準的 Alpha 合成運算，使全流程天然可微。

### 3.2 總平方 RGB 重建損失（Total Squared RGB Reconstruction Loss）
NeRF 的端到端訓練損失為總平方誤差加總（Eq. 6, p. 413）：
$$\mathcal{L} = \sum_{\mathbf{r} \in \mathcal{R}} \left[ \left\| \hat{C}_c(\mathbf{r}) - C(\mathbf{r}) \right\|_2^2 + \left\| \hat{C}_f(\mathbf{r}) - C(\mathbf{r}) \right\|_2^2 \right]$$
- $\mathcal{R}$ 為該批次隨機採樣之 4,096 條射線。
- 同時約束粗糙渲染 $\hat{C}_c(\mathbf{r})$ 與精細渲染 $\hat{C}_f(\mathbf{r})$，確保粗糙網路能持續提供優良的採樣分佈。

---

## 4. 關鍵實驗特性與儲存空間突破

在 ECCV 2020 的定量評估中（Table 1, p. 414），NeRF 在合成資料集達到 **PSNR 31.01 dB / SSIM 0.947 / LPIPS 0.081**，全面超越當時頂尖基線（SRN, NV, LLFF）[Mildenhall et al., 2020, Table 1](../evidence/mildenhall2020-nerf.md#42-定量實驗數據table-1-p-414)。

- **儲存空間突破（Sec. 6.3, p. 417）**：
  - 傳統離散體素方法（如 LLFF）為單一合成場景構建 3D 體素表示時，儲存需求超過 **15 GB**。
  - NeRF 僅需存儲神經網路權重檔案（約 **5 MB**），在該比較條件下實現了約 **3,000 倍**的相對壓縮。
  - *邊界說明*：15 GB 專指 LLFF 在該特定實驗設定下的體素容量，不代表所有體素方法皆需 15 GB。

---

## 5. 技術限制與邊界（Limitations）

### 5.1 論文直接指出之限制
1. **訓練與渲染速度極慢**：
   - 訓練：在單張 NVIDIA V100 GPU 上，單一場景需要 100k–300k 次迭代，耗時 **1 至 2 天（24–48 小時）** [Mildenhall et al., 2020, Sec 5.3, p. 413](../evidence/mildenhall2020-nerf.md#44-運算效率與硬體規格sec-53-p-413)。
   - 渲染推論：合成一張高解析度照片需對數十萬條射線查詢 192 次 MLP，單幀渲染耗時數秒至數十秒，**完全無法達到即時（Real-time）互動需求**。
2. **單一場景過度擬合（Per-scene Optimization）**：
   - 每個場景必須單獨訓練一組權重，無法跨場景通用或直接泛化 [Mildenhall et al., 2020, Sec 5.3, p. 412](../evidence/mildenhall2020-nerf.md#51-論文直接支持之結論與限制)。
3. **依賴精確相機姿態**：
   - 必須透過 COLMAP 等結構恢復演算法事先計算好相機內外參數；若姿態估計失敗，NeRF 將無法收斂。
4. **靜態場景假設**：
   - 本文基礎模型假設場景固定，無法原生表徵動態物體或形變。
5. **黑盒缺乏可解釋性（Interpretability）**：
   - 相比顯式網格或點雲，隱式神經權重難以直觀診斷失效模式 [Mildenhall et al., 2020, Sec 7, p. 418](../evidence/mildenhall2020-nerf.md#51-論文直接支持之結論與限制)。

### 5.2 本專案延伸之品質評估思考（防誤導邊界）
- **關鍵警示：高 PSNR 不代表 3D 幾何形狀正確**：
  - **P1 原始論文僅評估了新視角合成的「2D 影像品質」**。
  - **P1 完全沒有使用 Chamfer Distance 或 F-score 直接驗證 3D 幾何表面準確度**。
  - 因此，初學者絕不能因為 NeRF 的渲染影像極度逼真，就推論其 3D 空間幾何正確無誤。
  - *延伸議題*：後續學界探討從 NeRF 體積密度場抽取表面網格（如 Marching Cubes）時出現浮空雜訊（Floaters）或破洞等現象，需依賴後續幾何重建文獻進行驗證，不屬於 P1 本文的實驗結論。

---

## 6. 本頁面關聯文獻與證據鏈

- [Mildenhall et al., 2020](../evidence/mildenhall2020-nerf.md)：全篇概念、連續 5D 輻射場架構、位置編碼、分層體積採樣、體積渲染公式（Eq. 1, Eq. 3）、總平方 RGB 損失（Eq. 6）與實驗數據。
