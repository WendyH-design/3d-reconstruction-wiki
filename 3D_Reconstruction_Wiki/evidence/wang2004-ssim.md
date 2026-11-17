# 論文結構化證據卡：P6 - Wang et al. (2004) SSIM

本文件為 **P6 奠基論文（Image Quality Assessment: From Error Visibility to Structural Similarity）** 之單一事實來源證據卡（SSOT Evidence Card）。所有收錄之數學公式、常數定義、演算法架構、章節、頁碼及研究邊界均經原始文獻 `raw/P6_SSIM_Wang_TIP2004.pdf` 核實，為後續 `wiki/` 頁面與教學教材之事實依據。

---

## 1. 文獻基本資料與官方來源（Source Verification）

- **正式論文標題**：*Image Quality Assessment: From Error Visibility to Structural Similarity*
- **簡稱引用**：Wang et al. (2004)
- **正式作者完整名單**：
  1. Zhou Wang (New York University / University of Texas at Austin)
  2. Alan C. Bovik (University of Texas at Austin)
  3. Hamid R. Sheikh (University of Texas at Austin)
  4. Eero P. Simoncelli (New York University)
- **出版來源**：IEEE Transactions on Image Processing (TIP), Vol. 13, Issue 4, pp. 600–612
- **出版年份**：2004 年 4 月
- **官方來源 / DOI**：[DOI: 10.1109/TIP.2003.819861](https://doi.org/10.1109/TIP.2003.819861)
- **本地原始檔案保存**：`raw/P6_SSIM_Wang_TIP2004.pdf`（13 頁完整版，經官方出版物核實，符合 `.gitignore` 規範不入 Git）

---

## 2. 研究目的與核心設計哲學

### 2.1 傳統「誤差可見度（Error Visibility）」之限制 [Sec. I & II, pp. 600–602]
- **傳統方法假設**：如均方誤差（MSE）與峰值訊噪比（PSNR），本質上將影像失真視為獨立於背景內容的加性雜訊，測量像素值相減的能量大小。
- **人眼認知矛盾**：自然影像中的像素具有極強的空間相依性（Spatial Dependencies），人眼視覺系統（HVS）並不是逐像素測量誤差，而是辨識場景中物體的結構特徵。
- **典型失效案例**：當整張影像均勻增亮或對比微調時，MSE/PSNR 會報告極大誤差，但人眼卻能毫不費力地辨認出物體；反之，若高頻紋理結構被模糊，MSE 變化可能不大，人眼感知品質卻大幅劣化。

### 2.2 結構相似性（Structural Similarity）新哲學 [Sec. III, p. 602]
- **核心假設**：人類視覺系統（HVS）在演化上高度適應於提取場景中的**結構資訊（Structural Information）**。
- **結構資訊定義**：指場景中獨立於平均亮度（Average Luminance）與對比度（Contrast）之外、反映物體結構形狀的屬性。
- **評估架構（Fig. 3, p. 603）**：將兩信號 $x$ 與 $y$ 的比較解耦為三個獨立互補的分量：
  1. **亮度（Luminance, $l(x,y)$）**
  2. **對比度（Contrast, $c(x,y)$）**
  3. **結構（Structure, $s(x,y)$）**

---

## 3. 核心數學公式與常數定義

### 3.1 局部統計量定義 [Sec. III-A, p. 603]
給定對齊的兩信號區塊 $x = \{x_i \mid i=1, \dots, N\}$ 與 $y = \{y_i \mid i=1, \dots, N\}$：
- **局部均值（估計亮度）**：
  $$\mu_x = \frac{1}{N} \sum_{i=1}^N x_i, \quad \mu_y = \frac{1}{N} \sum_{i=1}^N y_i$$
- **局部標準差（估計對比度）**：
  $$\sigma_x = \left( \frac{1}{N-1} \sum_{i=1}^N (x_i - \mu_x)^2 \right)^{1/2}, \quad \sigma_y = \left( \frac{1}{N-1} \sum_{i=1}^N (y_i - \mu_y)^2 \right)^{1/2}$$
- **局部互協方差（估計結構相關度）**：
  $$\sigma_{xy} = \frac{1}{N-1} \sum_{i=1}^N (x_i - \mu_x)(y_i - \mu_y)$$

### 3.2 三大比較函數公式 [Sec. III-B, pp. 603–605]
1. **亮度比較函數**：
   $$l(x,y) = \frac{2\mu_x\mu_y + C_1}{\mu_x^2 + \mu_y^2 + C_1} \quad \text{[Eq. 6, p. 604]}$$
2. **對比度比較函數**：
   $$c(x,y) = \frac{2\sigma_x\sigma_y + C_2}{\sigma_x^2 + \sigma_y^2 + C_2} \quad \text{[Eq. 9, p. 605]}$$
3. **結構比較函數**：
   $$s(x,y) = \frac{\sigma_{xy} + C_3}{\sigma_x\sigma_y + C_3} \quad \text{[Eq. 10, p. 605]}$$

### 3.3 綜合 SSIM 公式 [Sec. III-B, p. 605]
- **通用乘積形式**：
  $$\text{SSIM}(x,y) = [l(x,y)]^\alpha \cdot [c(x,y)]^\beta \cdot [s(x,y)]^\gamma \quad \text{[Eq. 12, p. 605]}$$
- **經典化簡形式（設定 $\alpha = \beta = \gamma = 1$ 且 $C_3 = C_2 / 2$）**：
  $$\text{SSIM}(x,y) = \frac{(2\mu_x\mu_y + C_1)(2\sigma_{xy} + C_2)}{(\mu_x^2 + \mu_y^2 + C_1)(\sigma_x^2 + \sigma_y^2 + C_2)} \quad \text{[Eq. 13, p. 605]}$$

### 3.4 穩定常數之物理設定 [Sec. III-B & VII-A, pp. 604, 610]
為了防止分母為零並維持數值穩定性，常數 $C_1, C_2, C_3$ 定義為：
- $$C_1 = (K_1 L)^2 \quad \text{[Eq. 7, p. 604]}, \quad C_2 = (K_2 L)^2, \quad C_3 = C_2 / 2$$
- $L$ 為像素值的動態範圍（對於 8-bit 灰階或 RGB 影像，$L = 255$）。
- 論文建議且被廣泛採納的預設參數為：
  $$K_1 = 0.01, \quad K_2 = 0.03$$
- 代入後具體數值為：$C_1 = (0.01 \times 255)^2 = 6.5025$；$C_2 = (0.03 \times 255)^2 = 58.5225$。

### 3.5 數學性質 [Sec. III-C, p. 604]
1. **對稱性（Symmetry）**：$\text{SSIM}(x,y) = \text{SSIM}(y,x)$。
2. **有界性（Boundedness）**：$\text{SSIM}(x,y) \le 1$。
3. **唯一極大值（Unique Maximum）**：當且僅當 $x = y$ 時，$\text{SSIM}(x,y) = 1$。

---

## 4. 實作架構：局部高斯滑動視窗與 MSSIM

### 4.1 為什麼需要局部計算？ [Sec. IV, p. 605]
- 影像的統計特性在空間上具有非平穩性（Space-variant）。
- 人類視覺在特定注視點（Fovea）的注意力集中在局部視野。
- 全圖單一均值與變異數會模糊掉局部細微紋理的變化。

### 4.2 高斯加權局部視窗（Local Window） [Sec. IV, p. 605]
- 論文採用 $11 \times 11$ 圓形對稱標準高斯加權視窗 $w = \{w_i \mid i=1,\dots,N\}$，標準差 $\sigma = 1.5$。
- 權重正規化為 $\sum_{i=1}^N w_i = 1$。
- 加權統計量公式 [Eqs. 14–16, p. 606]：
  $$\mu_x = \sum_{i=1}^N w_i x_i, \quad \sigma_x^2 = \sum_{i=1}^N w_i (x_i - \mu_x)^2, \quad \sigma_{xy} = \sum_{i=1}^N w_i (x_i - \mu_x)(y_i - \mu_y)$$
- 視窗逐像素在影像上滑動，計算出局部的 SSIM 品質圖（SSIM Quality Map）。

### 4.3 全局平均結構相似度（Mean SSIM, MSSIM） [Sec. IV, Eq. 17, p. 606]
$$\text{MSSIM}(X,Y) = \frac{1}{M} \sum_{j=1}^M \text{SSIM}(x_j, y_j)$$
其中 $X, Y$ 為整張真值與待測影像，$x_j, y_j$ 為第 $j$ 個局部視窗中的影像內容，$M$ 為視窗總數。在後續文獻（包含 NeRF 與 3DGS）中，通常直接將 MSSIM 簡稱為 **SSIM**。

---

## 5. 指標數值解讀與評估邊界

### 5.1 數值範圍與物理意義
- **取值範圍**：理論範圍為 $[-1, 1]$，在自然影像失真評估中通常落在 $[0, 1]$ 區間。
- **越接近 1 越好（$\uparrow$）**：
  - **1.0**：兩張影像結構完全相同。
  - **0.95 以上**：人眼極難察覺到幾何結構失真，細節保留極佳。
  - **0.85–0.95**：具有輕度模糊或輕微偽影，但主要結構完整。
  - **0.70 以下**：存在明顯嚴重的邊緣撕裂、模糊或結構變形。

### 5.2 SSIM 6 大診斷問題分析
1. **這個指標到底在測量什麼？**
   - 測量兩張對齊 2D 影像在局部亮度、對比度以及最關鍵的「物體幾何結構相關性」的相似程度。
2. **它需要什麼樣的參考資料（Ground Truth）？**
   - 必須具備完全空間對齊的 2D 真值影像（Ground Truth Image）。
3. **分數高或低代表什麼物理意義？**
   - 越高越好。反映人眼視覺系統（HVS）對影像結構完整性與邊緣紋理保真度的滿意度。
4. **它能發現什麼問題？**
   - 能有效識別出邊緣模糊（Blurring）、結構斷裂、過度平滑化、高頻紋理喪失以及幾何形狀扭曲。
5. **它無法單獨發現什麼問題（盲點）？**
   - **對均勻色偏相對寬容**：若整張影像整體變亮、稍微變暗或對比度微調，SSIM 下降幅度很小，此時需搭配 PSNR 或色差檢測。
   - **無法驗證 3D 空間形狀**：SSIM 僅能檢驗 2D 視角畫面，無法得知物體在 3D 空間中的深度、法向量或空間座標是否正確。
6. **什麼情況下不能拿它與其他論文直接比較？**
   - 局部高斯視窗尺寸（$11 \times 11$ vs $7 \times 7$）或 $\sigma$ 設定相異時；
   - 評估解析度不同時（Downsampling 會濾除高頻，使 SSIM 分數虛高）；
   - 彩色通道處理方式不同時（僅評估 Y 亮度通道 vs. RGB 三通道平均）。

---

## 6. 在 3D 重建與新視角合成中的角色與防誤導機制

### 6.1 在 3D 重建文獻中的地位
- 在 **NeRF** [Mildenhall et al. (2020)](mildenhall2020-nerf.md) 與 **3DGS** [Kerbl et al. (2023)](kerbl2023-3dgs.md) 中，SSIM 均作為評估新視角合成視覺品質的三大核心標準指標之一。
- 在 3DGS 中，其損失函數更直接整合了結構不相似度損失項 $\mathcal{L}_{\text{D-SSIM}} = 1 - \text{SSIM}$ [Kerbl et al., 2023, Eq. 7](kerbl2023-3dgs.md#33-最佳化損失函數-sec-4-p-4-eq-7)，以驅動高斯點集學習銳利物體邊緣。

### 6.2 防誤導紅線：2D 視覺結構 $\neq$ 3D 空間幾何
- **核心警告**：**高 SSIM 絕不代表 3D 模型表面幾何正確**。
- **二手商品瑕疵檢驗案例**：若商品表面有細微裂痕或刮痕，重建演算法將其平滑化後，在 2D 渲染圖中 SSIM 可能依然高達 0.93（因為巨觀輪廓與整體結構依然完好），但關鍵瑕疵卻被徹底抹除。因此，驗證瑕疵與幾何完整性必須仰賴專門的幾何指標（如 Chamfer Distance, F-score）與局部缺陷保留分析，不可單憑 SSIM 斷言。

---

## 7. 原始章節與頁碼追溯索引

- **誤差可見度侷限與 HVS 哲學**：Sec. I & II, pp. 600–602
- **結構資訊定義與三分量分離架構**：Sec. III, p. 602; Fig. 3, p. 603
- **亮度、對比度與結構比較公式**：Sec. III-B, pp. 604–605, Eqs. (6), (9), (10)
- **通用 SSIM 乘積公式與經典化簡公式**：Sec. III-B, p. 605, Eqs. (12), (13)
- **穩定常數 $C_1, C_2, C_3$ 與參數 $K_1, K_2, L$**：Sec. III-B, p. 604, Eq. (7); Sec. VII-A, p. 610
- **高斯加權滑動視窗與 MSSIM 公式**：Sec. IV, p. 606, Eqs. (14)–(17)
- **LIVE 資料集驗證結果**：Sec. V, pp. 606–609, Tables I & II, Figs. 10 & 11
