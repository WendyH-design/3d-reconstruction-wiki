# 論文證據卡：Mildenhall et al. (2020) — NeRF

---

## 1. 文獻基本元資料（Metadata）

- **論文正式標題**：NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis
- **作者完整名單**：Ben Mildenhall<sup>⋆</sup>, Pratul P. Srinivasan<sup>⋆</sup>, Matthew Tancik<sup>⋆</sup>, Jonathan T. Barron, Ravi Ramamoorthi, Ren Ng
  - *等同貢獻標記*：Ben Mildenhall、Pratul P. Srinivasan 與 Matthew Tancik 具同等學術貢獻（PDF 第 1 頁註腳：`⋆ Authors contributed equally to this work.`）
- **第一作者**：Ben Mildenhall (UC Berkeley)
- **簡稱引用**：Mildenhall et al. (2020)
- **發表年份與出版來源**：ECCV 2020, Part I, Lecture Notes in Computer Science (LNCS), Volume 12346, pp. 405–421, Springer, Cham
- **官方來源 / DOI**：[DOI: 10.1007/978-3-030-58452-8_24](https://doi.org/10.1007/978-3-030-58452-8_24)
- **預印本資訊**：arXiv:2003.08934
- **官方研究專案頁**：[https://www.matthewtancik.com/nerf](https://www.matthewtancik.com/nerf)
- **本地原始文獻路徑**：`raw/P1_NeRF_Mildenhall_ECCV2020.pdf`（17 頁，格式完整可讀）
- **頁碼對應規則**：$\text{印刷頁碼} = \text{PDF 頁碼} + 404$（例如 PDF p. 10 對應印刷 p. 414）
- **Ingest 狀態**：`Completed`（通過 Gate 2 審查與 Step 6 DoD 驗收）

---

## 2. 研究問題與核心目的

- **研究問題**：如何僅藉由一組已知相機姿態之稀疏 2D 照片，合成未拍攝視角的相片級逼真新影像（Novel View Synthesis, NVS）？（Sec. 1, p. 405）
- **核心方法與定位**：
  - 提出一種連續 5D 向量值神經輻射場（Neural Radiance Field, NeRF），以多層感知機（MLP）隱式表徵場景。（Sec. 3, p. 408）
  - **方法定位釐清**：NeRF 是連續體積場景表示法與新視角影像合成方法，其本質並非直接輸出傳統 3D 幾何網格（Mesh）的幾何重建方法。（Sec. 1, 3, p. 405, 408）
- **相較既有方法之優勢**：
  - *突破網格限制*：傳統基於網格的微分光柵化優化極易陷入局部極小值，且需預設拓撲範本；NeRF 能表徵任意未知拓撲與非朗伯體材質。（Sec. 2, p. 408）
  - *突破離散體素容量限制*：離散體素網格（如 LLFF）受限於 $O(N^3)$ 記憶體爆炸；NeRF 將場景儲存為約 **5 MB** 的網路權重，在作者特定比較條件下，較 LLFF 構建之 15 GB 體素表示實現約 **3,000 倍**相對壓縮。（Sec. 2, 6.3, p. 408, 417）

---

## 3. 核心技術機制與關鍵公式

### 3.1 5D 連續場景表示（Sec. 3, p. 408–409）
- **輸入**：3D 空間座標 $\mathbf{x} = (x, y, z)$ 與 2D 觀察方向 $(\theta, \phi)$（實作上為 3D 單位笛卡兒向量 $\mathbf{d}$）。
- **輸出**：體積密度 $\sigma \in [0, \infty)$ 與發射顏色 $\mathbf{c} = (r, g, b)$。
- **多視角一致性約束**：體積密度 $\sigma$ 僅為位置 $\mathbf{x}$ 之函數；發射顏色 $\mathbf{c}$ 則由位置 $\mathbf{x}$ 與視線方向 $\mathbf{d}$ 共同決定，使網路能表徵非朗伯體（Non-Lambertian）的高光反射。（Sec. 3, p. 408–409）

### 3.2 體積渲染連續積分與離散求積（Sec. 4, p. 409–410）
- **連續體積渲染積分（Eq. 1, p. 409）**：
  $$C(\mathbf{r}) = \int_{t_n}^{t_f} T(t) \sigma(\mathbf{r}(t)) \mathbf{c}(\mathbf{r}(t), \mathbf{d}) \, dt, \quad \text{其中 } T(t) = \exp\left(-\int_{t_n}^t \sigma(\mathbf{r}(s)) \, ds\right)$$
  - $\mathbf{r}(t) = \mathbf{o} + t\mathbf{d}$ 為相機射線，$t_n, t_f$ 為近遠景深度邊界。
  - $\sigma(\mathbf{r}(t))$ 為體積密度，$T(t)$ 為射線從 $t_n$ 穿透至 $t$ 未受阻擋之累積透射率（Transmittance）。
- **離散求積公式（Eq. 3, p. 410）**：
  $$\hat{C}(\mathbf{r}) = \sum_{i=1}^N T_i \left(1 - \exp(-\sigma_i \delta_i)\right) \mathbf{c}_i, \quad \text{其中 } T_i = \exp\left(-\sum_{j=1}^{i-1} \sigma_j \delta_j\right), \quad \delta_i = t_{i+1} - t_i$$
  - 將連續積分轉化為標準 Alpha 合成運算（$\alpha_i = 1 - \exp(-\sigma_i \delta_i)$），使渲染過程完全可微分。

### 3.3 位置編碼（Positional Encoding, Sec. 5.1, Eq. 4, p. 412）
- 為克服神經網路偏好低頻訊號之頻譜偏差（Spectral Bias），將低維座標映射至高維：
  $$\gamma(p) = \left( \sin(2^0 \pi p), \cos(2^0 \pi p), \dots, \sin(2^{L-1} \pi p), \cos(2^{L-1} \pi p) \right)$$
  - 位置座標 $\mathbf{x}$ 使用 $L=10$（映射為 60 維）；視線方向 $\mathbf{d}$ 使用 $L=4$（映射為 24 維）。

### 3.4 分層體積採樣（Hierarchical Volume Sampling, Sec. 5.2, Eq. 5, p. 412–413）
- 同時最佳化兩套網路：
  - **粗糙網路（Coarse Network）**：於射線上以分層隨機採樣 $N_c = 64$ 個點，計算粗糙渲染權重 $w_i = T_i (1 - \exp(-\sigma_i \delta_i))$。
  - **機率分佈建構**：將權重正規化為 $\hat{w}_i = w_i / \sum_{j=1}^{N_c} w_j$，形成沿射線的 **分段常數機率密度函數（Piecewise-constant PDF）**，將採樣比重配置於預期對最終渲染顏色貢獻較高之區域。
  - **精細網路（Fine Network）**：透過逆變換採樣抽取額外 $N_f = 128$ 個點。
  - **聯合評估**：精細網路在原本的 64 點加上新增的 128 點，**共計 $N_c + N_f = 192$ 個位置進行評估**，計算最終射線色彩 $\hat{C}_f(\mathbf{r})$。

### 3.5 總平方 RGB 重建損失（Total Squared RGB Loss, Sec. 5.3, Eq. 6, p. 413）
$$\mathcal{L} = \sum_{\mathbf{r} \in \mathcal{R}} \left[ \left\| \hat{C}_c(\mathbf{r}) - C(\mathbf{r}) \right\|_2^2 + \left\| \hat{C}_f(\mathbf{r}) - C(\mathbf{r}) \right\|_2^2 \right]$$
- $\mathcal{R}$ 為批次隨機採樣之 4096 條相機射線。
- 對粗糙與精細渲染相對於真實像素 $C(\mathbf{r})$ 的平方誤差直接進行總和加總（非均方誤差 MSE）。

---

## 4. 實驗數據與條件查核（Sec. 6）

### 4.1 測試資料集規格（Sec. 6.1, p. 413–414）
- **Realistic Synthetic 360°**：8 個幾何複雜且具非朗伯體高光材質物件（Ship, Lego, Microphone, Materials, Drums, Ficus, Chair, Hotdog），解析度 $800 \times 800$（100 張訓練，200 張測試），使用真值相機姿態。
- **Real Forward-Facing**：8 個真實手持手機拍攝場景（Fern, Flower, Fortress, Horns, Leaves, Orchids, Room, T-Rex），原始拍攝解析度 $1008 \times 756$，評估時裁切為 $1008 \times 752$（20 至 62 張照片，保留 1/8 為測試集），相機姿態由 COLMAP 估計。
- **Diffuse Synthetic 360°**：DeepVoxels 資料集之 4 個漫反射朗伯體物件，解析度 $512 \times 512$。

### 4.2 定量實驗數據（Table 1, p. 414）

| 測試資料集 | 評估方法 | 評估解析度 | PSNR (dB) $\uparrow$ | SSIM $\uparrow$ | LPIPS (AlexNet) $\downarrow$ | 可信度標籤與條件說明 |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| **Realistic Synthetic 360°**<br>(8 個合成複雜物件) | SRN [41]<br>NV [23]<br>LLFF [27]<br>**NeRF (Ours)** | 512 × 512<br>800 × 800<br>800 × 800<br>**800 × 800** | 22.26<br>26.05<br>24.88<br>**31.01** | 0.846<br>0.893<br>0.911<br>**0.947** | 0.170<br>0.160<br>0.114<br>**0.081** | `[Partially Comparable]`<br>• SRN 因記憶體限制使用 512×512 解析度。<br>• NeRF 全面超越基線。 |
| **Real Forward-Facing**<br>(8 個真實手持場景) | SRN [41]<br>NV [23]<br>LLFF [27]<br>**NeRF (Ours)** | 504 × 376<br>不適用<br>1008 × 752<br>**1008 × 752** | 22.84<br>—<br>24.13<br>**26.50** | 0.668<br>—<br>0.798<br>**0.811** | 0.378<br>—<br>**0.212**<br>0.250 | `[Partially Comparable]`<br>• 原始拍攝尺寸為 1008×756，評估尺寸為 1008×752。<br>• SRN 評估解析度為 504×376。<br>• NV 限於封閉體積，無法評估。<br>• **指標排序衝突實例**：NeRF 在 PSNR/SSIM 最佳，但 **LLFF 的 LPIPS 較佳（0.212 vs 0.250）**。 |

### 4.3 消融實驗數據（Table 2, p. 418）
- 完整模型（Row 9）：PSNR **31.01** / SSIM **0.947** / LPIPS **0.081**。
- 移除位置編碼（No PE, Row 2）：PSNR 降至 **28.77**（證明高頻細節依賴 PE）。
- 移除視角相依（No VD, Row 3）：PSNR 降至 **27.66**（證明非朗伯體高光依賴視角輸入）。
- 移除分層採樣（No Hierarchical, Row 4）：PSNR 降至 **30.06**。
- 極簡模型（無 PE, 無 VD, 無分層採樣, Row 1）：PSNR 僅剩 **26.67**。

### 4.4 運算效率與硬體規格（Sec. 5.3, p. 413）
- 訓練硬體：單張 NVIDIA V100 GPU。
- 訓練時間：單一場景 100k–300k 次迭代，耗時 **1 至 2 天（約 24–48 小時）**。
- 推論速度：渲染單張高解析度影像需對數十萬條射線查詢 192 次 MLP，非即時（數秒至數十秒每張）。

---

## 5. 研究結論與限制歸屬

### 5.1 論文直接支持之結論與限制
1. **單一場景最佳化（Per-scene Optimization）**：模型權重高度擬合特定場景，無法直接跨場景泛化。（Sec. 5.3, p. 412）
2. **高訓練耗時與非即時推論**：訓練需 1–2 天，渲染非即時。（Sec. 5.3, 7, p. 413, 418）
3. **依賴相機內外參輸入**：必須具備精確相機姿態與場景邊界才能建立射線。（Sec. 5.3, p. 413）
4. **靜態場景假設**：本文模型假設場景固定，無法原生表徵動態物體。（Sec. 1, p. 405）
5. **模型內部不可解釋性（Interpretability）**：相比顯式網格或體素，隱式神經網路權重難以直觀分析失效原因。（Sec. 7, p. 418）
6. **工具定位**：COLMAP 僅為本文在真實場景實驗中獲取相機參數之工具，非 NeRF 的唯一必要綁定。（Sec. 5.3, p. 413）

### 5.2 本專案延伸研究邊界（明確標註需其他文獻支持）
1. **「視覺逼真 $\neq$ 幾何正確」的科學邊界**：
   - **P1 僅評估「新視角合成之 2D 影像品質（PSNR, SSIM, LPIPS）」**。
   - **P1 未使用 Chamfer Distance、Precision、Recall 或 F-score 直接驗證 3D 空間幾何表面的精確度**。
   - 絕不能僅憑 P1 的高 PSNR 數值推論其 3D 空間幾何形狀完全正確。
2. **延伸議題說明**：相機姿態誤差對畫質的劣化程度、光照變化/非剛性形變失效、以及體積密度場透過 Marching Cubes 抽取表面網格是否產生浮空雜訊（Floaters）或破洞，**均不屬於 P1 的實驗結論**，後續將標記為專案延伸議題。

---

## 6. 支援與貢獻之 Wiki 頁面

- [**`wiki/foundations.md`**](../wiki/foundations.md)：提供 Novel View Synthesis 定義、相機射線表示法 $\mathbf{r}(t) = \mathbf{o} + t\mathbf{d}$，以及 COLMAP 工具角色說明。
- [**`wiki/nerf.md`**](../wiki/nerf.md)：提供 NeRF 核心概念、連續場景表示定位、位置編碼、分層體積採樣機制、體積渲染公式（Eq. 1, Eq. 3）、總平方 RGB 損失（Eq. 6）與研究限制。
- [**`wiki/visual-quality.md`**](../wiki/visual-quality.md)：提供 PSNR / SSIM / LPIPS 在新視角合成之標準數據，以及 Real Forward-Facing 資料集上 NeRF vs LLFF 的指標排序衝突案例。
- [**`wiki/evaluation-comparison.md`**](../wiki/evaluation-comparison.md)：提供 NeRF 實驗條件矩陣（解析度差異、單卡 V100 耗時、輸出格式）與 `[Partially Comparable]` 標籤。
- [**`wiki/glossary.md`**](../wiki/glossary.md)：提供神經輻射場 (NeRF)、新視角合成 (NVS)、體積渲染、射線步進、位置編碼、分層體積採樣、總平方 RGB 重建損失之繁體中文（台灣）與英文對照與白話釋義。
