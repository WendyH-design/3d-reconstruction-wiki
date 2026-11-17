# 3D 重建常用資料集客觀評析（3D Reconstruction Datasets Overview）

本頁面介紹 3D 重建與物件理解領域中常見的代表性資料集型態，並以真實商品資料集 **Amazon Berkeley Objects (ABO)** [Collins et al., 2022](../evidence/collins2022-abo.md) 與大型場景幾何評估基準 **Tanks and Temples** [Knapitsch et al., 2017](../evidence/knapitsch2017-tanks-temples.md) 為核心案例，深入探討資料型態、相機參數真值與參考幾何地面真值（Ground Truth）的本質特性與邊界。

---

## 1. 為什麼 3D 重建需要大型資料集與實體真值？

在 2D 電腦視覺（如 ImageNet）中，大規模多樣化資料集推動了深度學習的爆發性進展。然而在 3D 視覺領域，取得**同時具備真實拍攝照片與精確 3D 幾何表面真值**的資料極為困難：
- **純合成 CAD 資料集（如 ShapeNet）**：雖然擁有大量 3D 模型，但紋理簡化、材質人工化，缺乏真實世界的物理光學複雜度，訓練出的演算法泛化至真實照片時性能往往大幅下滑。
- **早期真實影像資料集（如 Pix3D, IKEA）**：雖然將真實照片與 CAD 模型對齊，但涵蓋類別極少（例如 Pix3D 僅 9 類、約 1 萬張影像），無法代表複雜真實世界中的日常物體。
- **真實世界大場景評估之挑戰**：要客觀評估 3D 重建幾何誤差，必須在非實驗室環境下，透過高精度光學雷射掃描設備獲取毫米級真實表面點雲作為 Ground Truth。

---

## 2. ABO 資料集：真實商品級 3D 多模態資料庫

為了解決合成與真實世界的落差，[Collins et al. (2022)](../evidence/collins2022-abo.md) 推出了源自 Amazon 真實日常商品的 **Amazon Berkeley Objects (ABO)** 資料集。

### 2.1 四項核心統計數據與資料型態區隔
在理解 ABO 資料集時，必須精確區分以下四種不同資料型態的數量，**不可混為一談** [Collins et al., 2022, Sec. 3, p. 21128](../evidence/collins2022-abo.md#3-資料集組成與四項核心統計數據)：

```
[147,702 商品刊登清單 (Listings)] ──── 涵蓋 576 種商品類型 (Product Types) 之文字元資料
        │
        ├─► [398,212 商品目錄圖片 (Catalog Images)] ── 高解析度宣傳展示照片 (視角多元，絕大多數無姿態)
        │
        ├─► [8,222 具備 360° 環物影像之商品] ───────── 5°/15° 間隔拍攝之水平環視序列
        │
        └─► [7,953 Artist-created 3D Meshes] ────── 涵蓋 63 個 3D 模型類別的高品質網格模型
```

1. **147,702 個商品刊登清單（Listings）**：
   - 涵蓋 **576 種商品類型（Product Types）** 的結構化文字與規格資訊（如長寬高尺寸、包裝重量、材質類別）。
2. **398,212 張商品目錄圖片（Catalog Images）**：
   - 官方商品宣傳照片，通常為白底或生活情境展示，視角多元，絕大多數未提供相機姿態。
3. **8,222 個具有 360° 環物影像之商品**：
   - 放置於水平轉盤上，每隔 $5^\circ$ 或 $15^\circ$ 方位角拍攝一圈的多視角展示照片。
4. **7,953 個由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes）**：
   - 涵蓋 **63 個 3D 模型類別（3D model categories）**，採用 glTF 2.0 格式儲存，具有複雜幾何與 PBR 材質，**並非對實體商品進行物理掃描所取得的幾何真值**。

> [!NOTE]
> **576 與 63 之統計範圍精確區隔**：
> - **576**：代表整個 ABO 商品 listings 涵蓋的商品類型（Product Types）。
> - **63**：代表具有 3D 模型的類別數量（3D model categories）。
> - **論文 Figure 4**：呈現的是這 63 個 3D model categories 的類別數量分佈長條圖，並非 576 種商品類型的 3D 模型分佈。

---

## 3. 深入剖析：相機參數與真值幾何的關鍵邊界

### 3.1 相機參數真相：真實目錄照片 vs. Benchmark 渲染照片
在閱讀論文與評估重建演算法時，初學者最常產生的誤解是「以為資料集裡的所有照片都自帶相機內外參」：
- **原始商品目錄照片（Catalog Images）**：絕大部分均無相機姿態。ABO 作者**透過 3D 模型、實例遮罩與可微分渲染程序（PyTorch3D）估計，再經人工確認姿態標註，僅為其中的 6,334 張照片提供了 6-DOF 相機姿態標註** [Collins et al., 2022, Sec. 3, p. 21129](../evidence/collins2022-abo.md#42-相機參數真相真實資料-vs-benchmark-渲染資料-sec-3-p-2112921130)，**並非實際拍攝時直接記錄的相機姿態**。其餘近 39 萬張目錄照片均無姿態真值。
- **Benchmark 渲染合成影像**：為了進行材質估計與重建評估，作者利用 Blender Cycles 對 3D 模型進行路徑追蹤渲染。**每個物件使用 91 個相機位置，並從 108 個室內 HDRI 環境光照中隨機選取 3 個**，最終建立約 **210 萬張（2.1 million）** 合成渲染圖片，並提供對應相機內參、外參及相關 Ground Truth（深度圖、法向量圖、分割遮罩與 PBR 材質圖）。這 2.1M 張照片帶有完美相機參數，但它們是由虛擬 3D 模型渲染出來的，不可與真實相機拍攝的目錄照片混淆。

### 3.2 關鍵幾何邊界：Artist-created Meshes $\neq$ 物理掃描 Ground Truth
> [!IMPORTANT]
> **ABO 的 3D 模型是由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes）**：
> - 具有複雜幾何與基於物理的渲染（PBR）材質，可以作為宏觀形狀、零件比例與外觀結構的設計參考標準。
> - **它並非對實體商品進行物理掃描所取得的幾何真值（Ground Truth）**。
> - 現實商品在製造過程中存在的製造公差、材料彈性下垂、組裝接縫微小位移，在專業人工建模中通常會被理想化為完美規整的幾何體。

---

## 4. ABO 在 3D 研究中的用途與研究限制

### 4.1 主要研究用途 [Collins et al., 2022, Sec. 4](../evidence/collins2022-abo.md#5-3d-資料用途與基準評估任務)
1. **單視角 3D 重建評估**：檢驗在 ShapeNet 等合成資料上訓練的深度學習演算法，在面對複雜真實商品時的泛化落差。
2. **材質估計（Material Estimation）**：結合 PBR 材質參數，評估從照片估算表面物理屬性。
3. **跨域檢索（2D-to-3D Retrieval）**：評估從真實拍攝照片檢索資料庫中最相似 3D 模型的能力。

### 4.2 本專案特別關注之研究限制：非二手瑕疵資料集
> [!WARNING]
> **ABO 資料集無法直接用來驗證「二手商品微小瑕疵是否被保留」**：
> - ABO 中的所有商品均為官方全新良品（Brand New）。
> - 其 3D 模型展現的是完美無瑕的設計幾何，**完全未標註刮痕、凹陷、裂痕、掉漆等實體使用痕跡（Defects）**。
> - 在本專案中，ABO 是探討「電商商品級 3D 先驗」的絕佳範例，但**絕不能聲稱 ABO 具有評估二手瑕疵保留程度的 Ground Truth**。

---

## 5. Tanks and Temples：大場景 3D 重建幾何評估基準

與 ABO 關注商品級 CAD 網格不同，[Knapitsch et al. (2017)](../evidence/knapitsch2017-tanks-temples.md) 提出的 **Tanks and Temples** 是專為評估真實世界大場景 3D 重建精確度所設計的工業級評估基準。

### 5.1 資料集背景與核心定位
- **跳脫實驗室小型受控物體**：傳統基準（如 Middlebury）大多在室內實驗室拍攝小桌上物件，Tanks and Temples 走向真實戶外與大型複雜室內建築。
- **端到端完整管線評估**：評估演算法直接從「真實視訊影片輸入」到生成「密集 3D 表面點雲」的完整能力，不將 SfM 與 MVS 割裂看待。

### 5.2 資料集分組與場景特性（嚴格區隔 14 個測試與 7 個訓練場景）
Tanks and Temples 在官方論文與評測排行榜中，資料結構明確分為三組 [Knapitsch et al., 2017, Sec. 3 & 5](../evidence/knapitsch2017-tanks-temples.md#3-資料集分組與場景特性)：

1. **中階測試組（Intermediate Dataset，8 個測試場景）**：
   - 包含中大型戶外雕塑、軍事車輛、戶外遊樂場與房屋建築（如 *Family*, *Francis*, *Horse*, *Lighthouse*, *M60*, *Panther*, *Playground*, *Train*）。
   - 地面真值對外保密，演算法結果需上傳至官方伺服器統一評分。
2. **高階測試組（Advanced Dataset，6 個測試場景）**：
   - 包含超大型室內空間與宏偉建築群（如 *Auditorium*, *Ballroom*, *Courtroom*, *Museum*, *Palace*, *Temple*）。
   - 具備極端的光照反差、大面積弱紋理牆面與多重遮蔽結構。
3. **訓練組（Training Dataset，額外提供 7 個場景）**：
   - 官方網站公開釋出真值點雲，供研究人員調整參數與演算法驗證（如 *Barn*, *Caterpillar*, *Church*, *Courthouse*, *Ignatius*, *Meetingroom*, *Truck*）。
   - **注意**：訓練組共 7 個場景，與正式 Benchmark 評測的 14 個場景完全獨立，不可混算。

### 5.3 工業級雷射掃描地面真值（Ground Truth）獲取流程
Tanks and Temples 是如何取得毫米級空間真值的？[Knapitsch et al., 2017, Sec. 4.1](../evidence/knapitsch2017-tanks-temples.md#4-地面真值ground-truth取得流程與物理限制)
1. **硬體設備**：採用工業級 **FARO Focus 3D X330 HDR** 雷射掃描儀（有效測距達 330 公尺，在 10 公尺處測距噪聲僅 0.1 mm）。
2. **多測站全域配準**：每個場景從 4 至 17 個不同站位進行全面掃描，透過目標球與 ICP 演算法將各測站點雲配準統一。
3. **動態物體清理與裁切**：人工過濾穿行路人、動態車輛與遠景植被，定義指定評估邊界盒（Bounding Volume）。
4. **體素網格均勻重採樣**：以 **$\tau / 2$** 為體素大小進行重採樣（$\tau$ 為該場景距離門檻），落在同一體素內的點取空間平均值，以確保點雲密度空間分佈均勻。

### 5.4 地面真值的客觀物理限制（非零誤差模型）
> [!NOTE]
> **工業雷射掃描真值亦存在物理極限**：
> - 鏡面高光（如青銅雕像）與透明玻璃會造成雷射光散射或穿透，形成局部假影或數據缺失。
> - 多測站全域配準存在毫米級殘留對齊誤差。
> - **結論**：Ground Truth 是目前現實世界中極高精度的幾何參考基準，但不可將其神話為「絕對零誤差的數學完美模型」。

### 5.5 Tanks and Temples 在本專案中的角色與限制
- **角色**：作為本專案探討 **3D 空間幾何品質（Precision, Recall, F-score）** 的黃金標準文獻。
- **限制**：Tanks and Temples 專注於公尺至百公尺級的大型建築與車輛，非二手電商小型商品；其全域 F-score 測量的是宏觀幾何覆蓋度，**無法單獨反映物體表面 1 mm 的局部刮痕或凹痕是否被演算法保留**。

---

## 6. 資料集綜合對比矩陣：ABO vs. Tanks and Temples

| 比較維度 | ABO 資料集 [Collins et al., 2022](../evidence/collins2022-abo.md) | Tanks and Temples 基準 [Knapitsch et al., 2017](../evidence/knapitsch2017-tanks-temples.md) |
| :--- | :--- | :--- |
| **核心對象** | 日常電商家庭商品（家電、家具） | 大型真實場景、歷史雕塑、軍事車輛、宏偉建築 |
| **物理尺度** | 幾十公分至 2 公尺（物體級） | 數公尺至上百公尺（場景級與大型建築級） |
| **輸入資料形式** | 目錄宣傳照片、360° 環物照片、合成渲染圖 | 4K 高階電影級手持與雲台環繞連續影片 |
| **相機姿態來源** | 目錄照片絕大多數無姿態（6,334 張經演算法估計）；合成圖具真值 | 無直接相機真值；演算法需自行利用視訊執行 SfM 估計姿態 |
| **3D 參考真值型態** | **Artist-created 3D Meshes**（專業建模人員手工製作） | **工業級 LiDAR 掃描點雲**（FARO Focus 3D X330 HDR 實體量測） |
| **真值本質屬性** | 理想化出廠設計規格（PBR 材質與平滑幾何） | 真實實體世界物理量測（包含實際建造公差與表面磨損） |
| **核心評估維度** | 單視角重建倒角距離（CD）、材質估計、跨域檢索 | 空間幾何精確度（Precision）、完整度（Recall）與 **F-score** |
| **二手瑕疵檢驗能力** | **無**（皆為全新良品，未標註實體瑕疵） | **無**（宏觀幾何覆蓋，未針對毫米級局部刮痕做真值標註） |

---

## 7. 知識來源追溯

- ABO 證據卡：[`evidence/collins2022-abo.md`](../evidence/collins2022-abo.md)
- Tanks and Temples 證據卡：[`evidence/knapitsch2017-tanks-temples.md`](../evidence/knapitsch2017-tanks-temples.md)
- 3D 幾何評估方法論：參見 [`wiki/geometric-quality.md`](geometric-quality.md)
- 二手商品延伸探討：參見 [`wiki/secondhand-application.md`](secondhand-application.md)
- 跨資料集比較：參見 [`wiki/evaluation-comparison.md`](evaluation-comparison.md)
