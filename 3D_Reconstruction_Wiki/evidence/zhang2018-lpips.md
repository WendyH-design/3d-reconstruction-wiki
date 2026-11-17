# 論文結構化證據卡：P7 - Zhang et al. (2018) LPIPS

本文件為 **P7 奠基論文（The Unreasonable Effectiveness of Deep Features as a Perceptual Metric）** 之單一事實來源證據卡（SSOT Evidence Card）。所有收錄之數學公式、網路架構、訓練設定、BAPPS 資料集統計、章節、頁碼及研究邊界均經原始文獻 `raw/P7_LPIPS_Zhang_CVPR2018.pdf` 核實，為後續 `wiki/` 頁面與教學教材之事實依據。

---

## 1. 文獻基本資料與官方來源（Source Verification）

- **正式論文標題**：*The Unreasonable Effectiveness of Deep Features as a Perceptual Metric*
- **簡稱引用**：Zhang et al. (2018)
- **正式作者完整名單**：
  1. Richard Zhang (UC Berkeley)
  2. Phillip Isola (UC Berkeley / OpenAI)
  3. Alexei A. Efros (UC Berkeley)
  4. Eli Shechtman (Adobe Research)
  5. Oliver Wang (Adobe Research)
- **出版來源**：IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR 2018), pp. 586–595
- **出版年份**：2018 年
- **官方來源 / DOI**：[DOI: 10.1109/CVPR.2018.00068](https://doi.org/10.1109/CVPR.2018.00068)
- **開放取用（Open Access）**：[CVF Open Access](https://openaccess.thecvf.com/content_cvpr_2018/html/Zhang_The_Unreasonable_Effectiveness_CVPR_2018_paper.html)
- **本地原始檔案保存**：`raw/P7_LPIPS_Zhang_CVPR2018.pdf`（10 頁完整版，經 CVF 官方來源核實，符合 `.gitignore` 規範不入 Git）

---

## 2. 研究目的與核心問題

### 2.1 傳統指標的感知盲點 [Sec. 1, p. 586]
- **像素與低階指標限制**：傳統指標如 $L_2$ / PSNR 依賴單點像素差異，而 SSIM 依賴局部均值與變異數。當影像面臨模糊、輕微旋轉、微小形變或複雜生成假影時，像素指標往往報告極大誤差，但人眼卻能察覺「這兩張影像感知上依然是同一個場景」；反之，某些在像素值上很接近的噪聲，人眼卻覺得極為刺眼。
- **核心探討問題**：深度卷積神經網路（CNN）內部提取的階層化特徵，是否能天然作為一個高度符合人類主觀感知的距離度量（Perceptual Metric）？

### 2.2 核心研究貢獻 [Sec. 1, pp. 586–587]
1. **系統性評估深度特徵**：系統性檢驗了不同網路架構（SqueezeNet, AlexNet, VGG）與不同學習目標（監督學習、自監督學習、無監督隨機權重）在人類感知相似性任務上的表現。
2. **提出 LPIPS 指標**：正式提出**學習式感知影像區塊相似度（Learned Perceptual Image Patch Similarity, LPIPS）**，並提供線性校準（linear calibration）等微調機制。
3. **發布大規模 BAPPS 資料集**：構建了包含傳統失真與演算法生成失真、具有人類主觀偏好判斷的大規模基準資料集。

---

## 3. LPIPS 運作原理與核心數學公式

### 3.1 運作流程架構 [Sec. 3, Fig. 3 (Left), p. 591]
計算參考影像區塊 $x$ 與待測影像區塊 $x_0$ 之間的距離流程如下：
1. **輸入前向傳播**：將兩張影像分別輸入同一個深層神經網絡 $\mathcal{F}$。
2. **提取特徵圖（Feature Extraction）**：從預定的 $L$ 個卷積層中提取特徵堆疊。
3. **Channel 維度單位正規化（Unit Normalization）**：在每個空間位置 $(h, w)$ 上，沿著通道（Channel）維度進行 $L_2$ 正規化，得到單位長度特徵向量 $\hat{y}^l, \hat{y}^l_0$。
4. **學習通道加權（Channel Scaling）**：利用學習到的權重向量 $w^l$ 縮放各通道特徵。
5. **計算 $\ell_2$ 距離與空間/層級平均**：計算差值的平方 $L_2$ 範數，在空間維度平均，並在所有選定層加總。

### 3.2 核心數學公式 [Sec. 3, Eq. (1), p. 591]
$$d(x, x_0) = \sum_l \frac{1}{H_l W_l} \sum_{h,w} \left\| w^l \odot (\hat{y}^l_{hw} - \hat{y}^l_{0,hw}) \right\|_2^2$$

- **符號物理意義拆解**：
  - $x, x_0$：兩張待比較的輸入影像或區塊（Image Patches）。
  - $l$：選定的神經網絡特徵層索引（共取 $L$ 層）。
  - $H_l, W_l$：第 $l$ 層特徵圖的空間高度與寬度。
  - $\hat{y}^l_{hw}, \hat{y}^l_{0,hw} \in \mathbb{R}^{C_l}$：在第 $l$ 層、空間座標 $(h,w)$ 處，經過通道維度單位正規化後的特徵向量。
  - $w^l \in \mathbb{R}^{C_l}$：第 $l$ 層的通道加權向量（由人類感知判斷資料學習而得）。若設定 $w^l = \mathbf{1}, \forall l$，則退化為標準的餘弦距離（Cosine Distance）。
  - $\odot$：Hadamard 積（元素逐項相乘）。
  - $\|\cdot\|_2^2$：向量的歐幾里得距離平方（平方 $L_2$ 範數）。

---

## 4. 網路架構與三種訓練變體（lin, tune, scratch）

### 4.1 評估之網路架構與容量 [Sec. 3 & 4, pp. 591–592]
作者系統性評估了三種代表性架構，涵蓋不同參數量與計算複雜度：
- **SqueezeNet**：極輕量架構（模型檔案約 2.8 MB），運算極快。
- **AlexNet**：經典中型卷積架構（模型檔案約 9.1 MB）。
- **VGG**：經典深層大型卷積架構（模型檔案約 58.9 MB）。

### 4.2 核心發現：特徵的「非凡有效性」[Sec. 4 & 5, pp. 591–593]
- 實驗顯示，不論是監督學習（ImageNet 分類）、自監督學習（BiGAN 生成、Puzzle 拼圖、SplitBrain 著色、Watching 視訊追蹤），甚至是**未經訓練的隨機高斯初始化權重（RandNet）**，其特徵空間都展現出優於傳統像素與小波指標的人類感知對齊能力。
- **重要概念**：**LPIPS 絕不僅限於 ImageNet 預訓練的 VGG**，這是深層卷積階層結構對空間與頻率特徵提取的通用特性。

### 4.3 三種實作訓練變體對照表 [Sec. 3, p. 591]

| **變體名稱** | **網路主幹權重 $\mathcal{F}$ 處理方式** | **感知加權參數 $w$ 處理方式** | **訓練資料與參數量** | **特性與情境表現差異（無固定通用排名）** |
| :--- | :--- | :--- | :--- | :--- |
| **`lin` (linear)** | **完全凍結（Fixed）**<br>保持預訓練模型權重不變 | **從人類資料學習**<br>僅訓練各層通道的線性縮放權重 $w^l$ | 參數極少（例如 VGG 僅學習 1,472 個加權參數） | **最標準、最廣泛使用的 LPIPS 實作**；泛化能力最佳。在作者評估的 12 個真實演算法測試集（Real Algorithms）中，有 11 個透過線性校準獲得改善。 |
| **`tune` (fine-tune)** | **端到端微調（Fine-tuned）**<br>以預訓練權重為起點更新所有權重 | **同步學習**<br>與主幹網路一同進行反向傳播更新 | 更新網路所有卷積層權重 | **表現具情境依賴性**：在傳統失真與合成 CNN 失真測試中表現最優；但在真實演算法跨任務遷移測試中，全網微調反而可能過擬合於特定失真而降低泛化表現。 |
| **`scratch`** | **隨機初始化（Random Init）**<br>從高斯分布隨機初始化所有權重 | **從頭學習**<br>不使用任何 ImageNet 預訓練先驗 | 完全依賴感知偏好資料訓練全網路 | **表現依網路架構而異**：在 AlexNet 上表現略遜於線性校準，但在 VGG 上卻略優於線性校準；證明單純依賴感知判斷亦能習得有效特徵，但不應宣稱三者具備固定的優劣排名。 |

---

## 5. BAPPS 資料集與人類主觀感知評估

### 5.1 資料集規模與組成 [Sec. 2, Table 1 & Table 2, pp. 588–589]
作者建立了 **Berkeley-Adobe Perceptual Patch Similarity (BAPPS)** 基準資料集，專注於 $64 \times 64$ 像素的影像區塊（Patch-based）：
- **資料來源**：採集自 MIT-Adobe 5k（5,000 張未壓縮照片）與 RAISE1k 資料集。
- **失真類型**：涵蓋 20 種基礎失真與 308 種組合序列，分為**傳統低階失真（模糊、噪聲、壓縮、仿射變形）**與 **CNN 演算法生成失真（超解析度、去噪、自編碼假影）**。

### 5.2 兩種評估協定之精確區隔 [Sec. 2, Table 1, p. 588]
評估資料嚴格區分為兩種不同性質的心理物理學實驗，**不可混為一談**：

1. **二選一強制選擇（Two-Alternative Forced Choice, 2AFC）**：
   - **任務形式**：給定參考區塊 $x$，同時呈現兩張候選失真區塊 $x_0$ 與 $x_1$，詢問受試者：「哪一張圖片感知上更接近參考圖片？」
   - **資料總量**：共收集約 **484,300 筆（~484k）人類 2AFC 判斷**（包含 349.8k 筆合成失真判斷與 134.5k 筆真實演算法輸出判斷）。
   - **用途**：作為訓練與評估感知距離排序能力的核心基準。
2. **最小可察覺差（Just Noticeable Difference, JND）**：
   - **任務形式**：同時呈現兩張區塊，詢問受試者：「這兩張圖片完全相同還是不同？」（Same / Not same）。
   - **資料總量**：共收集 **28,800 筆（28.8k）JND 判斷**（涵蓋 9.6k 個區塊）。
   - **用途**：測量人眼在微小失真臨界點的可察覺閾值，與 2AFC 偏好排序分開統計。

---

## 6. 數值解讀、評估限制與防誤導機制

### 6.1 數值方向與解讀
- **距離型指標（越低越好，$\downarrow$）**：
  - **0.00**：兩張圖片在選取的深度特徵空間中完全一致。
  - **低於 0.10**：感知差異極小，人眼幾乎認為視覺風格與高頻紋理高度吻合（例如 NeRF 在合成資料集達 0.081）。
  - **0.20–0.30**：存在可察覺的局部紋理模糊或細微生成假影。
  - **高於 0.40**：感知特徵存在明顯結構差異、嚴重模糊或風格撕裂。

### 6.2 LPIPS 6 大診斷問題分析
1. **這個指標到底在測量什麼？**
   - 測量兩張影像在深層神經網路多層次特徵空間中的加權感知距離，反映人類大腦視覺皮層對邊緣、紋理與中階語義的綜合相似度判斷。
2. **它需要什麼樣的參考資料（Ground Truth）？**
   - 必須具備 2D 參考影像（Ground Truth Image）。
3. **分數高或低代表什麼物理意義？**
   - **越低越好（$\downarrow$）**。數值為特徵距離，值越小代表人類感知越相近。
4. **它能發現什麼問題？**
   - 能靈敏辨識出過度平滑導致的高頻紋理喪失、棋盤格偽影（Checkerboard Artifacts）、超解析度假影以及結構模糊。
5. **它無法單獨發現什麼問題（盲點）？**
   - **對絕對幾何位置微調寬容**：深層特徵具備一定的平移與微小變形不變性，因此局部幾何產生微幅形變時，LPIPS 可能變動不大。
   - **完全無法測量 3D 空間幾何正確性**：LPIPS 是純 2D 影像特徵距離，無法評估物體的真實 3D 深度、表面曲率或點雲座標。
6. **什麼情況下不能拿它與其他論文直接比較？**
   - 使用的主幹網路不同（SqueezeNet vs. AlexNet vs. VGG）；
   - 使用的評估版本或權重變體不同（`lin` vs. `tune` vs. 未校準預訓練特徵）；
   - 輸入影像解析度或前處理裁切方式不同。

### 6.3 防誤導紅線：2D 感知距離 $\neq$ 3D 幾何表面精確度
- **二手商品瑕疵檢驗防誤導案例（本專案風險推論）**：
  > [!NOTE]
  > 此處「神經平滑化可能消除刮痕但 LPIPS 仍給出低距離」之描述，為**本專案針對二手電商檢驗情境延伸提出之潛在風險情境與假說推論**（尚待本專案未來實驗實證），**不可解讀為 Zhang et al. (2018) 論文已直接驗證之研究結論**。
  - 在二手商品 3D 重建中，若演算法為了消除浮空噪聲而生成過於平滑但質感「看起來自然」的表面紋理，渲染出的 2D 圖片在 LPIPS 評估中可能取得極佳成績（例如 0.12 的優良低距離）；但原本實體物件上的細微刮痕、微小裂縫或凹坑卻可能被神經網絡平滑抹除。
- **結論**：LPIPS 能優秀地衡量「畫面看起來真不真實」，但**無法取代幾何指標（Chamfer Distance, F-score）來驗證 3D 模型實體幾何形狀的真實性與瑕疵保留度**。

---

## 7. 品質指標體系橫向定位總覽

| 指標名稱 | 完整名稱 | 評估核心內容 | 指標性質與方向 | 品質評估維度 | 代表文獻 |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **PSNR** | Peak Signal-to-Noise Ratio | 逐像素數值均方誤差（$L_2$） | 數值（dB），**越高越好 $\uparrow$** | **視覺品質（Visual Quality）** | 傳統指標 / [Mildenhall 2020](mildenhall2020-nerf.md) |
| **SSIM** | Structural Similarity Index | 局部亮度、對比度與結構相關度 | 標量 $[-1, 1]$，**越高越好 $\uparrow$** | **視覺品質（Visual Quality）** | [Wang et al. (2004)](wang2004-ssim.md) |
| **LPIPS** | Learned Perceptual Image Patch Similarity | 卷積神經網路深度特徵感知距離 | 距離值 $[0, \infty)$，**越低越好 $\downarrow$** | **視覺品質（Visual Quality）** | [Zhang et al. (2018)](zhang2018-lpips.md) |
| **CD** | Chamfer Distance | 3D 空間點雲雙向最近鄰幾何距離 | 距離值，**越低越好 $\downarrow$** | **幾何品質（Geometric Quality）** | Fan et al. (2017)（*待後續 P9 Ingest 建立*） |
| **F-score** | Precision & Recall Harmonic Mean | 給定距離閾值 $\tau$ 下之重合覆蓋率 | 百分比 $[0, 100\%]$，**越高越好 $\uparrow$** | **幾何品質（Geometric Quality）** | Knapitsch et al. (2017)（*待後續 P8 Ingest 建立*） |

---

## 8. 原始章節與頁碼追溯索引

- **傳統指標感知盲點與研究動機**：Sec. 1, pp. 586–587
- **BAPPS 資料集規格與失真分類**：Sec. 2, pp. 587–589, Table 1 & Table 2
- **2AFC（484k）與 JND（28.8k）實驗定義**：Sec. 2, p. 588, Table 1
- **LPIPS 運作流程與特徵正規化**：Sec. 3, p. 591, Fig. 3 (Left)
- **核心距離公式 Eq. (1)**：Sec. 3, p. 591, Eq. (1)
- **三種訓練設定（`lin`, `tune`, `scratch`）**：Sec. 3, p. 591
- **架構評估（SqueezeNet, AlexNet, VGG）與自監督實驗**：Sec. 4 & 5, pp. 591–593, Fig. 4
