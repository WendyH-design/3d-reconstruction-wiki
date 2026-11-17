# 論文 Ingest 歷程追蹤與狀態總覽（Ingest Log）

本文件追蹤本知識庫（LLM Wiki）所有文獻之 Ingest 執行歷程、審查關卡與完成狀態（DoD）。
每篇論文必須依序通過六階段作業，並由人類使用者確認後，狀態方可轉為 `Completed`。

---

## 狀態說明
- `Completed`：完全通過 Definition of Done (DoD)，已完成證據卡建立、Wiki 整合與 QA 檢驗。
- `In Progress`：正在進行來源核驗、PDF 取得或閱讀分析。
- `Needs Discussion`：已完成閱讀分析，等待 Gate 2 人工確認方向與方案。
- `Pending`：已列入清單，尚未啟動 Ingest 流程。
- `Deferred`：暫緩處理（保留候選清單，後續有需要再評估重啟）。
- `Blocked`：遭遇授權限制或其他障礙無法合法取得全文。

---

## 論文 Ingest 總表（調整後正式清單：6 篇核心 + 1 篇補充 + 2 篇暫緩）

| 編號 | 論文簡稱 | 正式標題 | 第一作者 | 出版年份 | 官方來源 / DOI | 證據卡檔案 | 整合 Wiki 頁面 | 當前狀態 | 備註 / 執行進度 |
| :---: | :--- | :--- | :--- | :---: | :--- | :--- | :--- | :---: | :--- |
| **P1** | Mildenhall et al. (2020) | *NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis* | Ben Mildenhall | 2020 | [DOI: 10.1007/978-3-030-58452-8_24](https://doi.org/10.1007/978-3-030-58452-8_24) | [`evidence/mildenhall2020-nerf.md`](../evidence/mildenhall2020-nerf.md) | `nerf.md`, `foundations.md`, `visual-quality.md`, `evaluation-comparison.md`, `glossary.md` | **`Completed`** | **核心文獻 1/6**；已通過 Gate 2 審查與 DoD 驗收 |
| **P2** | Kerbl et al. (2023) | *3D Gaussian Splatting for Real-Time Radiance Field Rendering* | Bernhard Kerbl | 2023 | [Article DOI: 10.1145/3592433](https://doi.org/10.1145/3592433) | [`evidence/kerbl2023-3dgs.md`](../evidence/kerbl2023-3dgs.md) | `3dgs.md`, `foundations.md`, `evaluation-comparison.md`, `glossary.md`, `index.md` | **`Completed`** | **核心文獻 2/6**；已通過 Gate 2 審查與 Step 4–6 DoD 驗收 |
| **P3** | Collins et al. (2022) | *ABO: Dataset and Benchmarks for Real-World 3D Object Understanding* | Jasmine Collins | 2022 | [DOI: 10.1109/CVPR52688.2022.02045](https://doi.org/10.1109/CVPR52688.2022.02045) | [`evidence/collins2022-abo.md`](../evidence/collins2022-abo.md) | `datasets.md`, `secondhand-application.md`, `evaluation-comparison.md`, `glossary.md`, `index.md` | **`Completed`** | **核心文獻 3/6**；已通過 Step 1–6 DoD 驗收與局部勘誤 |
| **P6** | Wang et al. (2004) | *Image Quality Assessment: From Error Visibility to Structural Similarity* | Zhou Wang | 2004 | [DOI: 10.1109/TIP.2003.819861](https://doi.org/10.1109/TIP.2003.819861) | [`evidence/wang2004-ssim.md`](../evidence/wang2004-ssim.md) | `visual-quality.md`, `evaluation-comparison.md`, `glossary.md`, `index.md` | **`Completed`** | **核心文獻 4/6**；已通過 Step 1–6 DoD 驗收 |
| **P7** | Zhang et al. (2018) | *The Unreasonable Effectiveness of Deep Features as a Perceptual Metric* | Richard Zhang | 2018 | [DOI: 10.1109/CVPR.2018.00068](https://doi.org/10.1109/CVPR.2018.00068) | [`evidence/zhang2018-lpips.md`](../evidence/zhang2018-lpips.md) | `visual-quality.md`, `evaluation-comparison.md`, `glossary.md`, `index.md` | **`Completed`** | **核心文獻 5/6**；已通過 Step 1–6 DoD 驗收 |
| **P8** | Knapitsch et al. (2017) | *Tanks and Temples: Benchmarking Large-Scale Scene Reconstruction* | Arno Knapitsch | 2017 | [Article DOI: 10.1145/3072959.3073599](https://doi.org/10.1145/3072959.3073599) | [`evidence/knapitsch2017-tanks-temples.md`](../evidence/knapitsch2017-tanks-temples.md) | `datasets.md`, `geometric-quality.md`, `completeness-and-defect-retention.md`, `evaluation-comparison.md`, `glossary.md`, `index.md` | **`Completed`** | **核心文獻 6/6**；已通過 Step 1–6 DoD 驗收 |
| **P9** | Fan et al. (2017) | *A Point Set Generation Network for 3D Object Reconstruction From a Single Image* | Haoqiang Fan | 2017 | [DOI: 10.1109/CVPR.2017.264](https://doi.org/10.1109/CVPR.2017.264) | `evidence/fan2017-psgn.md` | `geometric-quality.md` | `Pending` | **補充文獻**；採精簡版 Ingest（公式與頁碼核實） |
| **P4** | Liu et al. (2025) | *UnCommon Objects in 3D* | Xingchen Liu | 2025 | [DOI: 10.1109/CVPR52734.2025.01316](https://doi.org/10.1109/CVPR52734.2025.01316) | `evidence/liu2025-uco3d.md` | `datasets.md`, `completeness-and-defect-retention.md` | `Deferred` | **暫緩處理**；移至後續待辦 |
| **P5** | Farshian et al. (2023) | *Deep-Learning-Based 3-D Surface Reconstruction—A Survey* | Anis Farshian | 2023 | [DOI: 10.1109/JPROC.2023.3321433](https://doi.org/10.1109/JPROC.2023.3321433) | `evidence/farshian2023-survey.md` | `foundations.md`, `geometric-quality.md`, `evaluation-comparison.md` | `Deferred` | **暫緩處理**；移至後續待辦 |

---

## Ingest 詳細執行紀錄歷史歷程

### 2026-11-14：P1（NeRF）完成驗收
- **處理論文**：Mildenhall et al. (2020), *NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis* (ECCV 2020)
- **Step 1（來源核驗）**：核認官方 DOI `10.1007/978-3-030-58452-8_24`、arXiv:2003.08934、6 位作者全名、LNCS 12346 pp. 405–421。
- **Step 2（原始保存）**：下載保存於本地 `raw/P1_NeRF_Mildenhall_ECCV2020.pdf`（27.9 MB，17 頁完整可讀）。
- **Step 3（提報審議）**：完成 Gate 2 分析提報，經使用者審查提出 6 大修訂並通過。
- **Step 4（證據卡）**：建立 [`evidence/mildenhall2020-nerf.md`](../evidence/mildenhall2020-nerf.md)。
- **Step 5（Wiki 整合）**：建立/更新 `foundations.md`、`nerf.md`、`visual-quality.md`、`evaluation-comparison.md`、`glossary.md`。
- **Step 6（DoD 驗收）**：7 項檢查全數 PASS，標記為 `Completed`。

---

### 2026-11-14：P2（3DGS）完成驗收
- **處理論文**：Kerbl et al. (2023), *3D Gaussian Splatting for Real-Time Radiance Field Rendering* (ACM TOG / SIGGRAPH 2023)
- **Step 1（來源核驗）**：核認官方專屬 Article DOI `10.1145/3592433`、4 位作者全名（Bernhard Kerbl 與 Georgios Kopanas 共同第一作者）、Volume 42 Issue 4 Article 139 pp. 1–14。
- **Step 2（原始保存）**：合法下載保存於本地 `raw/P2_3DGS_Kerbl_SIGGRAPH2023.pdf`（24.3 MB，14 頁完整可讀）。
- **Step 3（提報審查）**：完成兩輪審查，精確校正 Algorithm 1 Split 判斷式 $\|\Sigma\| > \tau_S$、D-SSIM 損失項、Table 1 Deep Blending 數據（137 FPS / 676 MB）與顯存隔離，通過 Gate 2。
- **Step 4（證據卡）**：建立 [`evidence/kerbl2023-3dgs.md`](../evidence/kerbl2023-3dgs.md)。
- **Step 5（Wiki 整合）**：新增 `wiki/3dgs.md`，更新 `foundations.md`、`evaluation-comparison.md`、`glossary.md`、`index.md`。
- **Step 6（DoD 驗收）**：7 項檢查全數 PASS，標記為 `Completed`。

---

### 2026-11-14：P3（ABO Dataset）完成驗收與局部勘誤
- **處理論文**：Collins et al. (2022), *ABO: Dataset and Benchmarks for Real-World 3D Object Understanding* (CVPR 2022)
- **Step 1（來源核驗）**：
  - 核認官方 CVF Open Access、IEEE DOI `10.1109/CVPR52688.2022.02045`。
  - 12 位作者名單完全相符：Jasmine Collins, Shubham Goel, Kenan Deng, Achleshwar Luthra, Leon Xu, Erhan Gundogdu, Xi Zhang, Tomas F. Yago Vicente, Thomas Dideriksen, Himanshu Arora, Matthieu Guillaumin, Jitendra Malik。
  - 會議與頁碼：IEEE/CVF CVPR 2022, pp. 21126–21136。
- **Step 2（原始保存）**：合法下載保存於本地 `raw/P3_ABO_Collins_CVPR2022.pdf`（11 頁完整版，經 CVF 官方來源驗證可讀）。
- **Step 3（閱讀分析與 Gate 2 自檢）**：
  - 完成 5 項核心問題核對：
    1. ABO 是以 Amazon 真實家庭商品為基礎建立的大型多模態 3D 資料集，旨在彌合合成 CAD 與真實世界的落差。
    2. 嚴格確認並區隔四項核心數據：**147,702 個商品刊登清單（Listings）**、**398,212 張商品目錄圖片（Catalog Images）**、**8,222 個具有 360° 環物影像之商品**、**7,953 個 Artist-created 3D Meshes**。
    3. 幾何真值與相機邊界：3D Mesh 為專業建模人員手工建立的高品質 3D 網格模型（具有複雜幾何與 PBR 材質），**並非對實體商品進行物理掃描所取得的幾何真值**；商品目錄照片絕大多數無相機姿態（僅 6,334 張具備透過 3D 模型與可微分渲染程序估計再經人工確認之姿態標註，非實拍直接記錄），Benchmark 中 2.1M 張照片為從 3D 模型虛擬渲染產物。
    4. 二手應用限制：ABO 商品皆為原廠全新良品，**完全未提供物理刮痕、凹痕等缺陷 Ground Truth**，無法直接用以驗證重建演算法之瑕疵保留度。
    5. 教學教材定位：支援電商級 3D 資料集介紹，並作為二手檢驗中「設計參考網格 vs. 實體瑕疵檢驗」對比之案例。
  - 自檢 5 項指標全數通過，Gate 2 順利通過。
- **Step 4（產出證據卡）**：
  - 建立 [`evidence/collins2022-abo.md`](../evidence/collins2022-abo.md)，完整收錄數據、規格、相機標註與限制邊界。
- **Step 5（Wiki 整合）**：
  - 新增 [`wiki/datasets.md`](datasets.md)：系統性評析 3D 資料集型態與 ABO 核心組成及相機邊界。
  - 新增 [`wiki/secondhand-application.md`](secondhand-application.md)：深入探討二手電商 3D 重建的痛點、演算法平滑化消除瑕疵陷阱、結構覆蓋度 vs. 局部缺陷保留。
  - 更新 [`wiki/evaluation-comparison.md`](evaluation-comparison.md)：在第 1 節補充不同資料集型態與 Ground Truth 特性對比，並增列 ABO 來源引用。
  - 更新 [`wiki/glossary.md`](glossary.md)：新增 ABO、Artist-created Mesh、360° 環物影像、結構覆蓋度、局部缺陷保留等術語與深度辨析。
  - 更新 [`wiki/index.md`](index.md)：將資料集與二手應用條目更新為「已建立」。
  - **保護機制**：P1、P2 之證據卡與專屬 Wiki 頁面保持完全未受修改。
- **Step 6（DoD 驗收檢查）**：
  1. Metadata Verification：PASS（CVF 與 IEEE 官方元資料核驗）。
  2. Source Availability：PASS（本地 `raw/` 包含完整 11 頁 PDF）。
  3. Content Understanding：PASS（四項核心數據、相機與真值限制完全正確）。
  4. Evidence Traceability：PASS（所有主張與數據均標註章節、頁碼、表格編號）。
  5. Wiki Integration：PASS（`datasets.md` 與 `secondhand-application.md` 建立，雙向連結完整）。
  6. Quality Verification：PASS（繁體中文、無臆測、區隔論文用途與本專案延伸、P1/P2 內容完整）。
  7. Ingest Log：PASS（正式登記於本文件）。
- **2026-11-14 局部勘誤紀錄（精準度提升）**：
  1. **2.1M 合成渲染光照設定校正**：依論文 Sec. 3 核實，每個物件使用 91 個相機位置，從 108 個室內 HDRI 環境光照中隨機選取 3 個，最終建立約 2.1 million 張合成渲染圖片，並提供對應相機內外參及相關 Ground Truth（深度圖、法向量圖、分割遮罩與 PBR 材質圖）。
  2. **576 與 63 統計範圍精確區隔**：明確標明 576 為整個 ABO 商品 listings 涵蓋的商品類型（Product Types）；63 為具有 3D 模型的類別數量（3D model categories），論文 Figure 4 呈現的是 63 個類別的數量分布，非 576 種商品類型之分布。
  3. **Artist-created Mesh 用詞統一**：全面統一名稱為「由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes），具有複雜幾何與 PBR 材質，並非對實體商品進行物理掃描所取得的幾何真值」；6,334 張圖片的 6-DOF 姿態標註描述為透過 3D 模型、實例遮罩與可微分渲染程序估計，再經人工確認的姿態標註，並非實際拍攝時直接記錄的相機姿態。
  4. 同步更新檔案：`evidence/collins2022-abo.md`、`wiki/datasets.md`、`wiki/secondhand-application.md`、`wiki/evaluation-comparison.md`、`wiki/glossary.md`。
- **最終狀態判定**：**`Completed`**（核心文獻進度：**3/6**）

---

### 2026-11-14：P6（SSIM）完成驗收
- **處理論文**：Wang et al. (2004), *Image Quality Assessment: From Error Visibility to Structural Similarity* (IEEE Transactions on Image Processing)
- **Step 1（來源核驗）**：
  - 核認 IEEE DOI `10.1109/TIP.2003.819861`。
  - 4 位作者完整名單相符：Zhou Wang, Alan C. Bovik, Hamid R. Sheikh, Eero P. Simoncelli。
  - 出版來源：IEEE Transactions on Image Processing, Vol. 13, Issue 4, pp. 600–612, 2004 年 4 月。
- **Step 2（原始保存）**：合法下載保存於本地 `raw/P6_SSIM_Wang_TIP2004.pdf`（13 頁完整版，經官方出版物核實可讀，不入 Git）。
- **Step 3（閱讀分析與 Gate 2 自檢）**：
  - 完成 5 項核心問題與公式之嚴格核對：
    1. 核心哲學：跳脫傳統 MSE/PSNR 之誤差可見度（Error Visibility）加性獨立雜訊假設，確立人類視覺系統（HVS）高度適應提取「結構資訊（Structural Information）」的設計哲學。
    2. 三大比較面向：亮度（Luminance, $l(x,y)$）、對比度（Contrast, $c(x,y)$）與結構（Structure, $s(x,y)$），並精確記錄局部均值、標準差與互協方差定義公式（Sec. III-A & III-B）。
    3. 經典公式與常數：核實化簡公式 Eq. (7)，以及穩定常數 $C_1 = (K_1 L)^2, C_2 = (K_2 L)^2$（$K_1=0.01, K_2=0.03, L=255$），對應數值為 $C_1=6.5025, C_2=58.5225$。
    4. 實作架構：核實 $11 \times 11$ 圓形對稱標準高斯滑動視窗（標準差 $\sigma=1.5$ 且權重和為 1），以及全圖平均 MSSIM 指標公式 Eq. (12)。
    5. 能力邊界與防誤導：確認 SSIM 為 2D 影像感知指標，不保證 3D 空間幾何正確性；在二手商品檢驗中，演算法過度平滑化消除微小刮痕時，SSIM 依然可能維持高分，故不可單憑 SSIM 斷言物體幾何形狀或瑕疵保留之完整性。
  - 自檢 5 項指標全數通過，Gate 2 順利通過。
- **Step 4（產出證據卡）**：
  - 建立 [`evidence/wang2004-ssim.md`](../evidence/wang2004-ssim.md)，完整收錄研究背景、三分量比較公式、化簡公式、穩定常數、局部視窗架構、6 大診斷問題與章節頁碼索引。
- **Step 5（Wiki 整合）**：
  - 更新 [`wiki/visual-quality.md`](visual-quality.md)：新增第 3 節「結構相似性指標（SSIM）深度診斷（6 大診斷問題）」、第 4 節「PSNR vs. SSIM 特性對比矩陣」，更新關聯文獻與證據鏈。
  - 更新 [`wiki/evaluation-comparison.md`](evaluation-comparison.md)：在第 1 節加入評估解析度對 SSIM 之影響說明，第 4 節增列 Wang et al. (2004) 證據卡引用。
  - 更新 [`wiki/glossary.md`](glossary.md)：新增結構相似性指標（SSIM）、平均結構相似度（MSSIM）、局部高斯滑動視窗等術語，並增列「2.9 像素誤差（PSNR）vs. 結構相似性（SSIM）」易混淆概念深層辨析。
  - 更新 [`wiki/index.md`](index.md)：更新視覺品質評估章節導覽說明。
  - **保護機制**：P1（NeRF）、P2（3DGS）、P3（ABO）之專屬證據卡與專屬 Wiki 頁面完全未受干擾。
- **Step 6（DoD 驗收檢查）**：
  1. Metadata Verification：PASS（IEEE 官方元資料完整核對）。
  2. Source Availability：PASS（本地 `raw/` 包含完整 13 頁 PDF 且可讀）。
  3. Content Understanding：PASS（三分量架構、化簡公式、常數、高斯視窗與 2D 邊界完全掌握）。
  4. Evidence Traceability：PASS（所有公式、常數與診斷分析均精確標註章節與頁碼）。
  5. Wiki Integration：PASS（`visual-quality.md`, `evaluation-comparison.md`, `glossary.md`, `index.md` 深度整合且雙向連結完整）。
  6. Quality Verification：PASS（繁體中文標準用語、無臆測、嚴格區隔 2D 視覺與 3D 幾何、P1–P3 專屬頁面零損壞）。
  7. Ingest Log：PASS（正式登記於本文件）。
- **最終狀態判定**：**`Completed`**（核心文獻進度：**4/6**）

---

### 2026-11-15：P7（LPIPS）完成驗收
- **處理論文**：Zhang et al. (2018), *The Unreasonable Effectiveness of Deep Features as a Perceptual Metric* (IEEE/CVF CVPR 2018)
- **Step 1（來源核驗）**：
  - 核認 CVF Open Access 官方來源與 IEEE DOI `10.1109/CVPR.2018.00068`。
  - 5 位作者完整名單完全相符：Richard Zhang, Phillip Isola, Alexei A. Efros, Eli Shechtman, Oliver Wang。
  - 出版來源：IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR 2018), pp. 586–595。
- **Step 2（原始保存）**：合法下載保存於本地 `raw/P7_LPIPS_Zhang_CVPR2018.pdf`（10 頁完整版，經 pypdf 驗證全文可讀，不入 Git）。
- **Step 3（閱讀分析與 Gate 2 自檢）**：
  - 完成 6 項核心問題核對：
    1. LPIPS 概念：利用深度卷積神經網路多層次特徵空間中之加權距離，跳脫純像素誤差（PSNR）與手工統計結構（SSIM）的盲點，度量符合人眼大腦感知的相似度。
    2. 核心公式：核實 Eq. (1) $d(x, x_0) = \sum_l \frac{1}{H_l W_l} \sum_{h,w} \| w^l \odot (\hat{y}^l_{hw} - \hat{y}^l_{0,hw}) \|_2^2$，拆解通道單位正規化、學習權重 $w^l$、空間平均與層級累加。
    3. 網路架構與特徵泛化性：SqueezeNet（2.8 MB）、AlexNet（9.1 MB）、VGG（58.9 MB）；證明監督、自監督及隨機初始化特徵均展現非凡感知有效性，不限於 ImageNet VGG。
    4. 三種訓練變體：明確區隔 `lin`（凍結特徵主幹，僅學線性通道權重）、`tune`（端到端微調全網）、`scratch`（隨機初始化從頭學）。
    5. BAPPS 資料集：精確區隔約 484,300 筆 2AFC 人類主觀偏好判斷（$64 \times 64$ patches）與 28,800 筆 JND 判斷（同一/相異測試），無數據混用。
    6. 數值解讀與邊界：LPIPS 為距離型指標（越低越好 $\downarrow$），需要 2D 參考圖，非 3D 空間幾何指標；無法保證 3D 微小刮痕或幾何缺陷被保留。
  - 自檢通過，Gate 2 順利通過。
- **Step 4（產出證據卡）**：
  - 建立 [`evidence/zhang2018-lpips.md`](../evidence/zhang2018-lpips.md)，完整收錄核心問題、計算流程、Eq. (1) 符號意義、lin/tune/scratch 對照表、BAPPS 統計、6 大診斷問題與章節頁碼索引。
- **Step 5（Wiki 整合）**：
  - 更新 [`wiki/visual-quality.md`](visual-quality.md)：新增第 4 節「學習式感知影像區塊相似度（LPIPS）深度診斷（6 大診斷問題）」、更新第 5 節為「三大視覺品質指標綜合橫向對比矩陣（PSNR vs. SSIM vs. LPIPS）」、於防誤導紅線說明神經平滑化對刮痕保留之影響、更新關聯文獻與證據鏈。
  - 更新 [`wiki/evaluation-comparison.md`](evaluation-comparison.md)：在第 1 節補充評估網路與版本一致性說明，第 4 節增列 Zhang et al. (2018) 證據卡引用。
  - 更新 [`wiki/glossary.md`](glossary.md)：新增 LPIPS、深度特徵、通道正規化、2AFC、JND、線性校準（`lin`）等術語，並增列「2.9 PSNR vs. SSIM vs. LPIPS」與「2.10 2AFC vs. JND」深度辨析。
  - 更新 [`wiki/index.md`](index.md)：更新視覺品質評估章節導覽說明。
  - **保護機制**：P1（NeRF）、P2（3DGS）、P3（ABO）、P6（SSIM）專屬內容與檔案受到嚴格保護，零破壞。
- **Step 6（DoD 驗收檢查）**：
  1. Metadata Verification：PASS（CVF 與 IEEE 官方元資料完整核對）。
  2. Source Availability：PASS（本地 `raw/` 包含完整 10 頁 PDF 且可讀）。
  3. Content Understanding：PASS（Eq. 1、通道正規化、lin/tune/scratch、BAPPS 2AFC vs. JND、2D 感知邊界完全掌握）。
  4. Evidence Traceability：PASS（所有公式、變體與統計數據均精確標註章節與頁碼）。
  5. Wiki Integration：PASS（`visual-quality.md`, `evaluation-comparison.md`, `glossary.md`, `index.md` 深度整合且雙向連結完整）。
  6. Quality Verification：PASS（繁體中文標準用語、無臆測、嚴格區隔 2D 感知與 3D 幾何、P1–P3/P6 專屬內容零損壞）。
  7. Ingest Log：PASS（正式登記於本文件）。
- **最終狀態判定**：**`Completed`**（核心文獻進度：**5/6**）

---

### 2026-11-15：P8（Tanks and Temples）完成驗收
- **處理論文**：Knapitsch et al. (2017), *Tanks and Temples: Benchmarking Large-Scale Scene Reconstruction* (ACM Transactions on Graphics / SIGGRAPH 2017)
- **Step 1（來源核驗）**：
  - 核認 ACM Article DOI `10.1145/3072959.3073599` 與官方網站 `https://www.tanksandtemples.org/`。
  - 4 位作者完整名單完全相符：Arno Knapitsch, Jaesik Park, Qian-Yi Zhou, Vladlen Koltun (Intel Labs)。
  - 出版來源：ACM Transactions on Graphics, Vol. 36, Issue 4, Article 78, pp. 1–13, 2017 年。
- **Step 2（原始保存）**：合法下載保存於本地 `raw/P8_Tanks_and_Temples_Knapitsch_SIGGRAPH2017.pdf`（13 頁完整版，經 pypdf 驗證全文可讀，不入 Git）。
- **Step 3（閱讀分析與 Gate 2 自檢）**：
  - 完成 6 項核心問題核對：
    1. Tanks and Temples 是大場景 3D 重建幾何評估基準，跳脫實驗室小型物體，評估完整視訊到密集點雲的重建管線，是本專案走向 3D 幾何品質評估的核心文獻。
    2. 資料集分組精確區隔：中階測試組（Intermediate，8 個場景）、高階測試組（Advanced，6 個場景）與訓練組（Training，官方釋出 7 個場景），測試組 14 場景與訓練組 7 場景互不重疊。
    3. 地面真值（GT）取得流程：使用工業級 FARO Focus 3D X330 HDR 掃描儀進行多測站掃描、配準、清理與 Voxel Grid（$\tau/2$）重採樣；明確指出真值存在噪聲、反射穿透與配準殘差，非絕對零誤差模型。
    4. 核心幾何公式：核實最近鄰歐氏距離 $e_{r \rightarrow \mathcal{G}}$ 與 $e_{g \rightarrow \mathcal{R}}$（Eqs. 3–4）、Precision 精確度（Accuracy, Eq. 5）、Recall 召回率（Completeness, Eq. 6）以及 F-score 調和平均數（Eq. 7），並理解調和平均防作弊機制。
    5. 距離門檻 $\tau$：核實 Table 1 門檻依場景尺度設定（Family 3 mm, Francis 5 mm, Lighthouse 10 mm, Palace 30 mm），明確指出不同門檻下的 F-score 絕不可直接跨場景比較。
    6. 邊界與二手延伸：Tanks and Temples 為大場景，其全域 F-score 無法單獨反映物體表面 1 mm 的微小刮痕是否被保留；本專案延伸提出雙軌制檢驗框架（全域結構覆蓋度 + 局部感興趣區域缺陷真值比對）。
  - 自檢通過，Gate 2 順利通過。
- **Step 4（產出證據卡）**：
  - 建立 [`evidence/knapitsch2017-tanks-temples.md`](../evidence/knapitsch2017-tanks-temples.md)，完整收錄分組規模、FARO 掃描規格、體素尺寸 $\tau/2$、公式 Eqs. (3)–(7)、Table 1 門檻數據與章節頁碼索引。
- **Step 5（Wiki 整合）**：
  - 更新 [`wiki/datasets.md`](datasets.md)：新增第 5 節 Tanks and Temples 評析與第 6 節 ABO vs. Tanks and Temples 綜合對比矩陣。
  - 新增 [`wiki/geometric-quality.md`](geometric-quality.md)：深入拆解點對點最近鄰距離、Precision、Recall、F-score、距離門檻 $\tau$、調和平均防作弊機制、6 大診斷問題及與 Chamfer Distance 之聯繫。
  - 新增 [`wiki/completeness-and-defect-retention.md`](completeness-and-defect-retention.md)：系統性辨析全域幾何完整度 vs. 局部缺陷保留，剖析點雲數量量級稀釋效應與演算法平滑化偏見，並提出二手商品雙軌制檢驗架構構想。
  - 更新 [`wiki/evaluation-comparison.md`](evaluation-comparison.md)：新增第 4 節「2D 視覺品質 vs. 3D 幾何品質評估維度對照」，更新第 5 節證據卡引用。
  - 更新 [`wiki/glossary.md`](glossary.md)：新增 Tanks and Temples、地面真值點雲、工業級雷射掃描儀、Precision、Recall、F-score、距離門檻 $\tau$、點雲配準、體素網格重採樣等術語，並增列「2.11 Precision vs. Recall」與「2.12 視覺品質 vs. 幾何品質」深度辨析。
  - 更新 [`wiki/index.md`](index.md)：更新資料集與幾何評估章節導覽說明與活動連結。
  - **保護機制**：P1、P2、P3、P6、P7 專屬內容與檔案受到嚴格保護，零破壞。
- **Step 6（DoD 驗收檢查）**：
  1. Metadata Verification：PASS（ACM 官方 Article DOI `10.1145/3072959.3073599` 與作者完整名單核對）。
  2. Source Availability：PASS（本地 `raw/` 包含完整 13 頁 PDF 且可讀）。
  3. Content Understanding：PASS（資料集 8+6+7 分組、FARO 掃描、Precision/Recall/F-score 公式、門檻 $\tau$ 意義完全掌握）。
  4. Evidence Traceability：PASS（所有公式、門檻數據與真值規格均精確標註章節、表格與頁碼）。
  5. Wiki Integration：PASS（`datasets.md`, `geometric-quality.md`, `completeness-and-defect-retention.md`, `evaluation-comparison.md`, `glossary.md`, `index.md` 深度整合且雙向連結完整）。
  6. Quality Verification：PASS（繁體中文標準用語、無臆測、嚴格區隔 2D 視覺與 3D 幾何、P1–P3/P6/P7 專屬內容零損壞）。
  7. Ingest Log：PASS（正式登記於本文件）。
- **最終狀態判定**：**`Completed`**（核心文獻進度：**6/6**，六篇核心論文全數完成）
