# 論文結構化證據卡：P3 - Collins et al. (2022) ABO Dataset

本文件為 **P3 論文（ABO: Dataset and Benchmarks for Real-World 3D Object Understanding）** 之單一事實來源證據卡（SSOT Evidence Card）。所有收錄之數據、規格、章節、頁碼及研究限制均經原始文獻 `raw/P3_ABO_Collins_CVPR2022.pdf` 核實，為後續 `wiki/` 頁面與教學教材之事實依據。

---

## 1. 文獻基本資料與官方來源（Source Verification）

- **正式論文標題**：*ABO: Dataset and Benchmarks for Real-World 3D Object Understanding*
- **簡稱引用**：Collins et al. (2022)
- **正式作者完整名單**：
  1. Jasmine Collins (UC Berkeley)
  2. Shubham Goel (UC Berkeley)
  3. Kenan Deng (Amazon)
  4. Achleshwar Luthra (BITS Pilani)
  5. Leon Xu (UC Berkeley, Amazon)
  6. Erhan Gundogdu (Amazon)
  7. Xi Zhang (Amazon)
  8. Tomas F. Yago Vicente (Amazon)
  9. Thomas Dideriksen (Amazon)
  10. Himanshu Arora (Amazon)
  11. Matthieu Guillaumin (Amazon)
  12. Jitendra Malik (UC Berkeley)
- **出版來源**：IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR 2022), pp. 21126–21136
- **出版年份**：2022 年
- **官方來源 / DOI**：[DOI: 10.1109/CVPR52688.2022.02045](https://doi.org/10.1109/CVPR52688.2022.02045)
- **開放取用（Open Access）**：[CVF Open Access](https://openaccess.thecvf.com/content/CVPR2022/papers/Collins_ABO_Dataset_and_Benchmarks_for_Real-World_3D_Object_Understanding_CVPR_2022_paper.pdf)
- **arXiv 預印本**：[arXiv:2110.06199](https://arxiv.org/abs/2110.06199)
- **本地原始檔案保存**：`raw/P3_ABO_Collins_CVPR2022.pdf`（11 頁完整版，經 CVF 官方來源核實，符合 `.gitignore` 規範不入 Git）

---

## 2. 研究目的與資料集核心貢獻

### 2.1 欲解決之痛點 [Sec. 1, p. 21126–21127]
現有 3D 物件理解研究長期受限於資料集規模與真實感不足：
- **合成 CAD 資料集（如 ShapeNet）**：幾何結構簡化、紋理人工化，缺乏真實世界的材料物理屬性，與真實照片存在顯著的 Domain Gap。
- **真實影像標註資料集（如 Pix3D, IKEA）**：雖然包含真實影像，但規模極小（Pix3D 僅涵蓋 9 個物體類別、約 1 萬張影像），且模型常受限於類別多樣性與對齊精度。

### 2.2 ABO 資料集核心貢獻 [Sec. 1, p. 21127]
推出 **Amazon Berkeley Objects (ABO)** 大型資料集，橋接虛擬 3D 與真實世界的落差：
1. 提供源自 Amazon 真實家電、家具等日常商品之多模態資料（含商品清單、高解析度目錄照片、360° 環物展示影像與 3D 模型）。
2. 提供具備基於物理渲染（PBR）材質的高品質 3D Mesh，支援單視角 3D 重建、材質估計與跨域檢索三大 Benchmark。

---

## 3. 資料集組成與四項核心統計數據

論文 Sec. 3（pp. 21128–21129）明確定義了四項不同資料型態的核心數量，**嚴禁混為一談**：

| 資料類型 | 論文統計數量 | 具體定義與規格 | 物理意涵與邊界 |
| :--- | :---: | :--- | :--- |
| **商品刊登清單（Listings）** | **147,702** | 涵蓋 576 種商品類型（Product Types），源自 Amazon 旗艦與旗下網站（PrimeNow, Whole Foods 等）。 | 包含結構化元資料（材質、顏色、長寬高尺寸、Item ID 等）。 |
| **商品目錄圖片（Catalog Images）** | **398,212** | 高解析度官方商品宣傳與展示照片。 | 單張或多視角電商攝影棚照片，**絕大部分未提供相機姿態**。 |
| **具備 360° 環物影像之商品** | **8,222** | 商品具有水平轉盤拍攝之「360º View」展示照片。 | 以 $5^\circ$ 或 $15^\circ$ 方位角間隔旋轉拍攝之多視角照片序列。 |
| **3D 幾何模型（Artist-created 3D Meshes）** | **7,953** | 涵蓋 63 個 3D 模型類別（3D model categories），以 glTF 2.0 格式儲存。 | 由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes），具有複雜幾何與 PBR 材質，**並非對實體商品進行物理掃描所取得的幾何真值**。 |

> [!NOTE]
> **576 與 63 之統計範圍精確區隔**：
> - **576**：代表整個 ABO 資料集中 147,702 筆商品 listings 涵蓋的商品類型（Product Types）。
> - **63**：代表 7,953 個 3D 模型所歸屬的 3D 模型類別數量（3D model categories，依 WordNet 分類對齊）。
> - **Figure 4**：呈現的是這 63 個 3D model categories 的類別數量分佈長條圖，並非 576 種商品類型的 3D 模型分佈。

---

## 4. 資料型態差異與相機參數辨析

### 4.1 商品目錄圖片 vs. 360° 環物影像 vs. 3D Mesh
1. **商品目錄圖片（398,212 張）**：商業攝影成果，背景通常去背純白，拍攝視角多元但不規則，未記錄精密相機參數。
2. **360° 環物影像（8,222 個商品）**：轉盤等角度拍攝，提供繞物體一圈的水平視角照片，可提供豐富的環視外觀資訊。
3. **Artist-created 3D Meshes（7,953 個）**：由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes），具有複雜幾何與 PBR 材質（Base Color, Roughness, Metallic），並非對實體商品進行物理掃描所取得的幾何真值。

### 4.2 相機參數真相：真實資料 vs. Benchmark 渲染資料 [Sec. 3, p. 21129–21130]
> [!IMPORTANT]
> **嚴禁宣稱「所有商品目錄照片均具備已知相機參數」**：
> - **原始商品目錄照片的姿態標註**：透過 3D 模型、實例遮罩與可微分渲染程序（PyTorch3D）估計，再經人工確認的姿態標註，**僅有 6,334 張商品目錄圖片提供 6-DOF 相機姿態標註**，並非實際拍攝時直接記錄的相機姿態。其餘近 39 萬張目錄照片均無姿態標註。
> - **Benchmark 渲染資料**：論文為了評估材質估計與重建，利用 Blender Cycles 對 3D 模型進行路徑追蹤渲染。**每個物件使用 91 個相機位置（沿著物件上部半球二十面體），並從 108 個室內 HDRI 環境光照中隨機選取 3 個**，最終建立約 210 萬張（2.1 million）合成渲染圖片，並提供對應相機內參、外參及相關 Ground Truth（深度圖、法向量圖、分割遮罩與 PBR 材質圖）。這 2.1M 張渲染照片具有完美的相機參數，但它是從 3D 模型虛擬渲染產生的，不可與原始真實相機照片混淆。

---

## 5. 3D 資料用途與基準評估任務

ABO 在論文中設計了三大評估基準（Benchmarks）[Sec. 4, pp. 21130–21133]：
1. **單視角 3D 重建（Single-view 3D Reconstruction）** [Sec. 4.1, Table 3, p. 21130]：
   - 評估在 ShapeNet 上預訓練的現有方法（3D-R2N2, Occupancy Networks, GenRe, Mesh R-CNN）泛化至真實商品時的幾何重建能力。
   - 評估指標：倒角距離（Chamfer Distance, CD）與法向量一致性（Absolute Normal Consistency）。
   - 結果發現：現有方法直接泛化至 ABO 真實商品時，倒角距離誤差顯著上升（幾何品質嚴重退化），證明真實商品幾何比 ShapeNet 更複雜。
2. **材質估計（Material Estimation）** [Sec. 4.2]：利用 3D Mesh 導出的 PBR 參數（Base color, Roughness, Metallic），評估從單張或多張照片還原真實物體材質的能力。
3. **跨域物件檢索（Cross-Domain Multi-View Object Retrieval）** [Sec. 4.3]：測試從 2D 真實照片檢索對應 3D 模型的能力。

---

## 6. 研究限制與重要防偽邊界（DoD 查核核心）

1. **Artist-created Meshes $\neq$ 物理掃描 Ground Truth**：
   - 由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes），具有複雜幾何與 PBR 材質，**並非對實體商品進行物理掃描所取得的幾何真值**。
   - 存在人工建模簡化、製造公差、材質彈性變形等落差。
2. **非每件商品均具備完整對應**：
   - 147k 清單中，僅有 7,953 個商品具備 3D 模型（約 5.4%），僅有 8,222 個商品具備 360° 影像。三者並非完全一對一重疊。
3. **絕對不是「二手商品瑕疵檢驗」資料集**：
   - ABO 的所有商品均為全新標準商品（Catalog Items），其 3D 模型展現的是完美無損的「設計標準態」。
   - **原論文完全未提供刮痕、凹陷、裂縫、磨損等局部缺陷的實體真值標註**。
   - **結論邊界**：ABO 可作為商品級 3D 先驗與宏觀形狀參考，但**絕不能直接宣稱 ABO 具備驗證二手瑕疵保留程度的能力**。

---

## 7. 與本專案知識庫之關聯性與角色定位

- **支援概念頁面**：
  - [`wiki/datasets.md`](../wiki/datasets.md)：作為真實商品物件資料集的代表性案例，闡明電商多視角影像與 Artist-created 網格模型的性質。
  - [`wiki/secondhand-application.md`](../wiki/secondhand-application.md)：探討二手商品 3D 重建中，宏觀設計參考網格（Artist-created prior）與實體局部缺陷檢驗之間的本質差異。
- **概念定位分層**：
  - 論文原始用途：真實家庭商品多模態大型資料集、單視角重建基準、材質估計。
  - 本專案延伸用途：對比「由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes）」與「物理表面真值」，闡述二手檢驗中瑕疵可能被平滑掉的風險。

---

## 8. 來源章節與頁碼追溯索引

- 核心貢獻與背景：Sec. 1, pp. 21126–21128
- 資料集統計（147,702 清單、398,212 照片、8,222 環物、7,953 模型、63 模型類別）：Sec. 3, pp. 21128–21129
- 6,334 張目錄照片姿態標註與可微分渲染程序：Sec. 3, pp. 21129–21130
- 2.1M 張合成材質渲染影像規格（91 視角、108 HDRI 隨機選 3）：Sec. 3, p. 21130
- 單視角重建基準與 Chamfer Distance 評估：Sec. 4.1, Table 3, p. 21130
- 現有資料集對比：Table 1, p. 21128
