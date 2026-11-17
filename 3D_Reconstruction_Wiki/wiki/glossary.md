# 專業術語對照辭典（Glossary）

本辭典為本專案知識庫與教學簡報之統一術語基準。全站嚴格遵循繁體中文（台灣慣用語）標準，並提供權威之英文字詞、縮寫、白話釋義與易混淆概念深層解析。

---

## 1. 核心專有名詞對照表

| 繁體中文名稱 | 英文正式名稱 | 常用縮寫 | 初學者白話釋義 | 專案使用規範與代表文獻 |
| :--- | :--- | :---: | :--- | :--- |
| **神經輻射場** | Neural Radiance Fields | **NeRF** | 將 3D 場景的顏色與密度隱式編碼在神經網絡權重中的連續場景表示技術。 | 核心技術名詞；[Mildenhall et al., 2020](../evidence/mildenhall2020-nerf.md) |
| **3D 高斯潑濺** | 3D Gaussian Splatting | **3DGS** | 使用數百萬個具備協方差與透明度的顯式 3D 高斯橢球點集表徵場景，結合快速分塊光柵化實現即時渲染。 | 核心技術名詞；[Kerbl et al., 2023](../evidence/kerbl2023-3dgs.md) |
| **新視角合成** | Novel View Synthesis | **NVS** | 利用多張已知角度的照片，演算法合成並生成「從未被拍攝過之全新視角觀察點」的照片。 | 核心任務概念；[Mildenhall et al., 2020](../evidence/mildenhall2020-nerf.md) |
| **體積渲染** | Volume Rendering | — | 沿著相機視線光線步進，將光線穿透介質微粒時沿途的密度與發光顏色加權累積的渲染技術。 | 渲染原理；[Mildenhall et al., 2020, Sec 4](../evidence/mildenhall2020-nerf.md) |
| **光線步進** | Ray Marching | — | 從相機光心向像素發射光線，沿著視線方向由近至遠依等距或自適應間距採樣計算的數值方法。 | 幾何採樣；[Mildenhall et al., 2020, Sec 4](../evidence/mildenhall2020-nerf.md) |
| **體積密度** | Volume Density | $\sigma$ | 空間中特定位置微粒的凝聚程度；$\sigma$ 越高代表此處越可能是實心物體表面。 | 幾何物理表徵；[Mildenhall et al., 2020, Sec 3](../evidence/mildenhall2020-nerf.md) |
| **視角依賴輻射** | View-dependent Radiance | $\mathbf{c}(r,g,b)$ | 物體表面向不同觀察方向所發散的光線顏色與亮度，用以呈現金屬光澤與鏡面反射。 | 外觀表徵；[Mildenhall et al., 2020, Sec 3](../evidence/mildenhall2020-nerf.md) |
| **位置編碼** | Positional Encoding | **PE** | 利用不同頻率的正弦餘弦函數將低維坐標映射至高維特徵空間，使神經網絡能學習到幾何細節。 | 演算法機制；[Mildenhall et al., 2020, Sec 5.1](../evidence/mildenhall2020-nerf.md) |
| **階層體積採樣** | Hierarchical Volume Sampling | — | 同時計算粗網絡與細網絡，利用粗網絡產生的分段常數 PDF 將更多採樣點集中在對渲染貢獻較大的區域。 | 演算法機制；[Mildenhall et al., 2020, Sec 5.2](../evidence/mildenhall2020-nerf.md) |
| **總平方 RGB 重建損失** | Total Squared RGB Reconstruction Loss | — | 針對特定光線中粗網絡與細網絡渲染相對於真實照片像素間之平方誤差進行總和的損失函數。 | 最佳化損失；[Mildenhall et al., 2020, Eq. 6](../evidence/mildenhall2020-nerf.md) |
| **協方差矩陣** | Covariance Matrix | $\Sigma$ | 決定 3D 高斯橢球在三維空間中拉伸長度與空間旋轉方向的半正定矩陣。 | 幾何數學表徵；[Kerbl et al., 2023, Eq. 4](../evidence/kerbl2023-3dgs.md) |
| **自適應密度控制** | Adaptive Density Control | — | 在訓練過程中動態對高斯進行修剪（Prune）、複製（Clone）與分裂（Split）的演算法機制。 | 演算法機制；[Kerbl et al., 2023, Sec 5.2](../evidence/kerbl2023-3dgs.md) |
| **分塊光柵化** | Tile-based Rasterization | — | 將螢幕分割為 $16 \times 16$ 像素小塊，配合 GPU Radix Sort 進行平行排序與 $\alpha$-blending 的渲染架構。 | 渲染架構；[Kerbl et al., 2023, Appendix C](../evidence/kerbl2023-3dgs.md) |
| **最佳化模型參數的儲存容量** | Model Parameter Storage | **Mem** | 儲存訓練完畢之百萬級高斯屬性或神經網絡權重所需之容量（數百 MB），非 GPU 運行顯存。 | 評估指標；[Kerbl et al., 2023, Table 1](../evidence/kerbl2023-3dgs.md) |
| **結構不相似度損失項** | Structural Dissimilarity Loss | **D-SSIM** | 衡量影像局部結構幾何差異之損失項，常與 L1 損失加權組合。 | 最佳化損失；[Kerbl et al., 2023, Eq. 7](../evidence/kerbl2023-3dgs.md) |
| **運動恢復結構** | Structure-from-Motion | **SfM** | 一種從多角度無序照片中，同時估算相機 3D 姿態與稀疏 3D 特徵點雲的電腦視覺演算法框架。 | 幾何基礎概念；`wiki/foundations.md` |
| **COLMAP** | COLMAP | — | 當前學術界與產業界最廣泛使用的開源 SfM / MVS 軟體工具套件。 | 工具軟體；[Mildenhall et al., 2020](../evidence/mildenhall2020-nerf.md) |
| **真值** | Ground Truth | **GT** | 作為驗證基準的真實物理數據（如精準雷射掃描 3D 點雲、真實相機拍攝的高解析度照片）。 | 評估基準概念；全站通用 |
| **峰值信噪比** | Peak Signal-to-Noise Ratio | **PSNR** | 衡量渲染影像與真實照片在像素階層均方誤差的指標，單位為 dB，數值越高越好。 | 視覺品質指標；`wiki/visual-quality.md` |
| **結構相似性指標** | Structural Similarity Index | **SSIM** | 衡量兩張影像在局部亮度、對比度與空間結構相關性之客觀評估指標，相較於 PSNR 更貼近人眼感知。 | 視覺品質指標；[Wang et al., 2004](../evidence/wang2004-ssim.md) |
| **平均結構相似度** | Mean Structural Similarity Index | **MSSIM** | 在整張影像上滑動局部高斯視窗計算 SSIM 後取算術平均之綜合指標，實務論文中常逕稱 SSIM。 | 視覺品質指標；[Wang et al., 2004](../evidence/wang2004-ssim.md) |
| **局部高斯滑動視窗** | Local Gaussian Sliding Window | — | 計算局部統計量（均值、變異數、協方差）時所採用之 $11 \times 11$ 圓形對稱高斯權重遮罩（$\sigma=1.5$）。 | 演算法機制；[Wang et al., 2004](../evidence/wang2004-ssim.md) |
| **學習式感知影像區塊相似度** | Learned Perceptual Image Patch Similarity | **LPIPS** | 利用預訓練深層卷積神經網路多層次特徵空間中之加權距離，度量影像感知相似度之距離型指標（越低越好）。 | 視覺品質指標；[Zhang et al., 2018](../evidence/zhang2018-lpips.md) |
| **深度特徵** | Deep Feature | — | 由多層卷積神經網路隱藏層萃取出的抽象層次視覺特徵（涵蓋邊緣、紋理至物體語義），具備高度契合人眼感知的表徵能力。 | 核心表徵概念；[Zhang et al., 2018](../evidence/zhang2018-lpips.md) |
| **通道正規化** | Channel Normalization | — | 在特徵圖各空間位置將跨通道向量進行單位化（$L_2$ 範數歸一化），消除絕對數值規模偏差。 | 演算法機制；[Zhang et al., 2018](../evidence/zhang2018-lpips.md) |
| **二選一強制選擇** | Two-Alternative Forced Choice | **2AFC** | 一種心理物理學實驗方法，要求受試者在兩張候選圖片中強制選出較接近參考圖片的一張。 | 心理物理學評估；[Zhang et al., 2018](../evidence/zhang2018-lpips.md) |
| **最小可察覺差** | Just Noticeable Difference | **JND** | 人眼視覺系統能察覺到兩種刺激（如兩張影像）存在差異的最小物理變化量（判斷相同或不同）。 | 心理物理學評估；[Zhang et al., 2018](../evidence/zhang2018-lpips.md) |
| **線性校準** | Linear Calibration | **`lin`** | 凍結主幹網路預訓練權重，僅以人類感知判斷資料微調通道線性權重 $w$ 之輕量校準設定。 | 演算法設定；[Zhang et al., 2018](../evidence/zhang2018-lpips.md) |
| **亞馬遜柏克萊物件資料集** | Amazon Berkeley Objects | **ABO** | 由 Amazon 與 UC Berkeley 合作發布的大規模真實家庭商品多模態 3D 資料集。 | 資料集名詞；[Collins et al., 2022](../evidence/collins2022-abo.md) |
| **專業建模網格模型** | Artist-created 3D Mesh | — | 由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes），具有複雜幾何與 PBR 材質，並非對實體商品進行物理掃描所取得的幾何真值。 | 資料形態概念；[Collins et al., 2022](../evidence/collins2022-abo.md) |
| **360° 環物影像** | 360° Turntable Images | — | 物體放置於旋轉轉盤上，依固定角度間隔（如 5° 或 15°）拍攝之一系列水平環視照片。 | 資料形態概念；[Collins et al., 2022](../evidence/collins2022-abo.md) |
| **結構覆蓋度** | Structural Coverage | — | 衡量物體宏觀三維幾何形狀（正面、背面、盲區）是否被完整重建、有無大面積破洞的指標。 | 品質評估概念；`wiki/secondhand-application.md` |
| **局部缺陷保留** | Local Defect Retention | — | 衡量實體物體表面微小的刮痕、凹痕、裂縫在 3D 重建後是否被忠實保留而非被平滑抹除的指標。 | 品質評估概念；`wiki/secondhand-application.md` |
| **Tanks and Temples** | Tanks and Temples | — | 由 Intel Labs 發布的大規模真實場景 3D 重建幾何評估基準，具備工業級雷射掃描 Ground Truth。 | 資料集名詞；[Knapitsch et al., 2017](../evidence/knapitsch2017-tanks-temples.md) |
| **地面真值點雲** | Ground Truth Point Cloud | — | 透過高精度光達或雷射掃描儀量測實體表面所取得的 3D 空間參考點雲。 | 評估基準概念；[Knapitsch et al., 2017](../evidence/knapitsch2017-tanks-temples.md) |
| **工業級雷射掃描儀** | Industrial Laser Scanner | — | 具備毫米級測距精度、用於採集實體幾何真值之精密光學測量儀器（如 FARO Focus 3D X330 HDR）。 | 硬體設備；[Knapitsch et al., 2017](../evidence/knapitsch2017-tanks-temples.md) |
| **幾何精確度** | Precision / Accuracy | $P(d)$ | 重建點雲中，落在真值表面指定距離門檻 $d$ 內的點所佔之比例（越高越好）。 | 幾何品質指標；[Knapitsch et al., 2017, Eq. 5](../evidence/knapitsch2017-tanks-temples.md) |
| **幾何召回率 / 完整度** | Recall / Completeness | $R(d)$ | 真值表面點雲中，能在重建模型中找到指定距離門檻 $d$ 內對應點之比例（越高越好）。 | 幾何品質指標；[Knapitsch et al., 2017, Eq. 6](../evidence/knapitsch2017-tanks-temples.md) |
| **F-score** | F-score | $F(d)$ | 幾何精確度（Precision）與召回率（Recall）之調和平均數，任一項低則總分必低（越高越好）。 | 幾何品質指標；[Knapitsch et al., 2017, Eq. 7](../evidence/knapitsch2017-tanks-temples.md) |
| **距離門檻** | Distance Threshold | $\tau$ | 評估重建點是否落在真值容許誤差範圍內的判定距離（單位為毫米 mm），數值隨場景尺度動態設定。 | 幾何評估參數；[Knapitsch et al., 2017, Table 1](../evidence/knapitsch2017-tanks-temples.md) |
| **點雲配準** | Point Cloud Registration | — | 將多個不同測站或視角採集之點雲，透過幾何旋轉與平移變換統一對齊至同一坐標系的過程。 | 演算法機制；[Knapitsch et al., 2017](../evidence/knapitsch2017-tanks-temples.md) |
| **體素網格重採樣** | Voxel Grid Resampling | — | 將三維空間劃分為固定體素格點，並以體素內點之均值代表該體素，以實現點雲空間密度均勻化之處理技術。 | 演算法機制；[Knapitsch et al., 2017](../evidence/knapitsch2017-tanks-temples.md) |

---

## 2. 容易混淆之概念深層辨析（Confusable Concept Disambiguation）

### 2.1 3D 幾何重建 vs. 新視角合成（NVS）
- **3D 幾何重建**：目標是輸出物體在真實 3D 空間中的**實體形狀**（如點雲、Mesh 網格），評估重點是**幾何品質**（表面距離、破洞缺漏）。
- **新視角合成（NVS）**：目標是輸出某個觀察角度下的**逼真 2D 畫面**，評估重點是**視覺品質**（像素是否逼真）。
- **核心警告**：NVS 表現好（高 PSNR）絕對不代表物體在 3D 空間中的幾何形狀正確。

### 2.2 隱式連續場（NeRF）vs. 顯式高斯點集（3DGS）
- **隱式連續場（NeRF）**：場景記憶在 MLP 網絡權重中（檔案極小，約 5 MB），空間解析度連續無限，但每次渲染需步進採樣數百次網絡推論，**渲染極慢**。
- **顯式高斯點集（3DGS）**：場景由數十萬至數百萬個離散的 3D 高斯橢球構成（模型參數儲存約 400–750 MB），透過 GPU 分塊快速光柵化，**實現即時渲染（100+ FPS）**。

### 2.3 模型參數儲存容量 vs. 訓練顯存（VRAM）vs. 推論光柵化額外顯存
- **最佳化模型參數的儲存容量（Model Parameter Storage / Table 1 Mem）**：指模型最佳化後儲存於硬體介質的屬性參數量（NeRF 約 5 MB；3DGS 約 400–750 MB）。
- **訓練顯存峰值（Peak Training VRAM）**：指在訓練反向傳播期間 GPU 所需之最大顯存開銷（3DGS 在大場景計算峰值可超過 20 GB）。
- **推論光柵化額外顯存**：指 3DGS 在推論階段光柵化器管理 Tile 分塊與排序暫存所需之額外顯存（約 30–500 MB）。
- **三者物理概念截然不同，不可混為一談**。

### 2.4 SfM（技術概念）vs. COLMAP（實作軟體）
- **SfM**：是一套透過照片視差估算幾何姿態的「演算法原則與數學框架」。
- **COLMAP**：是實現 SfM 的一種「具體軟體工具」。不能說「NeRF 使用 SfM 軟體」，應說「使用 COLMAP 軟體來實作 SfM」。

### 2.5 體積密度（$\sigma$）vs. 階層不透明度（$\alpha$）
- **體積密度（$\sigma$）**：連續物理微粒密度，單位為空間長度的倒數（$[0, \infty)$），反映光線穿透該處時被阻擋的速率。
- **階層不透明度（$\alpha$）**：離散區間值（$\alpha_i = 1 - \exp(-\sigma_i \delta_i) \in [0, 1]$），反映光線穿過距離 $\delta_i$ 之後被吸收或遮蔽的具體比例。

### 2.6 總平方 RGB 重建損失 vs. 均方誤差（MSE）
- **均方誤差（MSE）**：包含除以樣本數（光線數 $\times$ 像素數）的平均運算。
- **總平方 RGB 重建損失（Total Squared Loss）**：NeRF 原始論文 Eq. 6 僅將粗網絡與細網絡的光線平方誤差進行加總，未進行除以樣本數的平均，故數學上稱總平方損失更為精確。

### 2.7 專業人員建立網格模型（Artist-created Meshes）vs. 實體物理掃描真值（Scanned Ground Truth）
- **專業人員建立網格模型（如 ABO 的 7,953 個模型）**：由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes），具有複雜幾何與 PBR 材質，展現的是物體出廠規格的「設計參考標準」，並非對實體商品進行物理掃描所取得的幾何真值。
- **實體物理掃描真值（如 Tanks and Temples 的 LiDAR 掃描）**：使用高精度雷射光達掃描實體物理表面取得的點雲數據，忠實反映物體的真實物理幾何形狀與使用磨損。
- **兩者在評估重建幾何真值時代表截然不同的基準層次**。

### 2.8 宏觀幾何完整度 vs. 二手商品局部瑕疵保留
- **宏觀幾何完整度（結構覆蓋度）**：關注物體外觀組件與整體形狀是否完整，例如正面、側面與底部有無遺失或破洞。
- **二手商品局部瑕疵保留**：關注微小刮痕、掉漆、凹陷等細節是否被如實重建。傳統演算法為了追求光滑常強制平滑消除微小瑕疵，因此高幾何完整度並不等於具備瑕疵檢驗能力。

### 2.9 像素誤差（PSNR）vs. 結構相似性（SSIM）vs. 深度感知距離（LPIPS）
- **PSNR（誤差可見度）**：純粹以對齊像素為單位測量訊號均方誤差（$L_2$ 距離），對極小幾何位移（如 1 像素平移）或整體均勻亮暗變化極端敏感，但對高頻模糊與結構喪失較為遲鈍。
- **SSIM（結構相似性）**：基於人類視覺系統（HVS）擅長提取結構資訊的假設，將比較解耦為亮度、對比度與結構相關度，對模糊與結構斷裂極端靈敏，且對微幅光照變化更具適應性。
- **LPIPS（深度特徵感知距離）**：基於深層卷積神經網路特徵空間，測量多層級語義與紋理特徵的加權距離（越低越好），最貼近人眼主觀視覺滿意度，對微小幾何平移容忍度最高，對過度平滑懲罰最重。
- **共同邊界**：**三者皆為 2D 影像感知指標，均無法獨立保證 3D 空間幾何形狀正確性與微小幾何瑕疵之保留**。

### 2.10 二選一強制選擇（2AFC）vs. 最小可察覺差（JND）
- **2AFC（二選一強制選擇）**：給定參考影像 $x$，在候選影像 $x_0$ 與 $x_1$ 之間強制選擇「哪一張更接近參考影像」，測量的是人類主觀感知的**相對偏好排序**（BAPPS 資料集收集約 484k 筆）。
- **JND（最小可察覺差）**：同時呈現兩張影像，詢問受試者「兩者完全相同還是存在差異」，測量的是人眼感官的**絕對辨識閾值**（BAPPS 資料集收集約 28.8k 筆）。
- **兩者在心理物理學上屬於不同維度的測試協定，數據不可混為一談**。

### 2.11 幾何精確度（Precision / Accuracy）vs. 幾何召回率（Recall / Completeness）
- **Precision（準確度）**：衡量「重建出來的點到底有多準」。若重建結果中充斥著漂浮在半空中的噪點、毛刺或虛假結構，Precision 會大幅下跌。
- **Recall（完整度）**：衡量「真實物體表面到底覆蓋了多少」。若模型正面重建良好，但物體背面或底部因為拍攝盲區而留有大破洞，Recall 會大幅下跌。
- **F-score 權衡**：F-score 是兩者的調和平均數，能有效阻止「只重建極少數極準確點」或「在全空間胡亂撒點」等投機作弊策略。

### 2.12 2D 視覺品質指標 vs. 3D 幾何品質指標
- **2D 視覺品質指標（PSNR / SSIM / LPIPS）**：在 2D 視角投影影像上運算，衡量渲染畫面是否好看、像素誤差與特徵紋理是否貼近真值照片；**完全無法直接證明 3D 空間形狀正確**。
- **3D 幾何品質指標（Precision / Recall / F-score）**：在 3D 空間歐氏距離上運算，需依賴高精度工業雷射掃描 Ground Truth 點雲；直接度量空間曲面偏差與覆蓋率。


