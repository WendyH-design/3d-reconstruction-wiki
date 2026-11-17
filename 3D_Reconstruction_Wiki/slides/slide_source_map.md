# 投影片知識來源映射表（Slide Source Map）

本文件落實「單一知識來源（Single Source of Truth, SSOT）」原則與教學閉環約束。
本專案教學簡報《從照片到可信任的 3D 模型：3D 重建技術與品質評估入門》（`slides/index.html`）全數 12 頁之教學主張、公式、實驗數據、硬體條件與應用案例，**100% 嚴格映射至已驗收之 `wiki/` 概念知識頁與 6 篇核心文獻 `evidence/` 結構化證據卡**。

---

## 1. 投影片總覽與知識映射矩陣（12 頁教學流）

| 投影片編號 | 主題名稱 | 核心學習目標（初學者導向） | 映射之 Wiki 頁面 | 映射之 Evidence Cards | 學術結論 vs. 延伸推論 | 狀態 |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: |
| **Slide 01** | 封面：從照片到可信任的 3D 模型 | 破題引言、核心痛點提問與全篇學習地圖導引 | `wiki/index.md` | - | 專案引言 | `Verified` |
| **Slide 02** | 3D 重建是什麼？從 2D 像素到 3D 空間 | 釐清相機降維投影本質；區別「3D 幾何重建」與「新視角合成」之目標分流 | `wiki/foundations.md` | `mildenhall2021-nerf.md`<br>`kerbl2023-3dgs.md` | 原始文獻結論 | `Verified` |
| **Slide 03** | 為什麼需要多角度照片？視差、三角測量與相機姿態 | 理解雙眼視差、三角測量原理、相機光線數學表示，以及 SfM 技術概念與 COLMAP 軟體工具之消歧義 | `wiki/foundations.md`<br>`wiki/glossary.md` | `mildenhall2021-nerf.md`<br>`kerbl2023-3dgs.md` | 原始文獻結論 | `Verified` |
| **Slide 04** | 神經輻射場（NeRF）：用神經網路記憶光影 | 掌握連續 5D 隱式輻射場、MLP 表徵、位置編碼、射線步進採樣與體積渲染積分之直觀概念 | `wiki/nerf.md`<br>`wiki/foundations.md` | `mildenhall2021-nerf.md` | 原始文獻結論 | `Verified` |
| **Slide 05** | 3D Gaussian Splatting：即時渲染的突破 | 掌握顯式 3D 高斯橢球表徵、協方差矩陣、自適應密度控制（Split/Clone）與 GPU 分塊光柵化之原理 | `wiki/3dgs.md`<br>`wiki/foundations.md` | `kerbl2023-3dgs.md` | 原始文獻結論 | `Verified` |
| **Slide 06** | NeRF vs. 3DGS：核心特性互動切換比對 | 互動對比兩大範式之表徵型態、訓練耗時、渲染幀率、記憶體消耗與適用情境（客觀呈現硬體條件） | `wiki/evaluation-comparison.md`<br>`wiki/nerf.md`<br>`wiki/3dgs.md` | `mildenhall2021-nerf.md`<br>`kerbl2023-3dgs.md` | 原始文獻結論 | `Verified` |
| **Slide 07** | 3D 重建常用資料集：真實世界與真值邊界 | 認識電商商品資料集（ABO）與大型場景基準（Tanks and Temples）；剖析相機參數真相與幾何 Ground Truth 邊界 | `wiki/datasets.md`<br>`wiki/secondhand-application.md` | `collins2022-abo.md`<br>`knapitsch2017-tanks-temples.md` | 原始文獻結論 | `Verified` |
| **Slide 08** | 如何判斷影像的視覺品質？PSNR、SSIM 與 LPIPS | 深入診斷三大 2D 視覺指標（像素誤差 vs. 局部結構 vs. 深度特徵）；解析 Real Forward-Facing 之指標衝突案例 | `wiki/visual-quality.md` | `wang2004-ssim.md`<br>`zhang2018-lpips.md`<br>`mildenhall2021-nerf.md` | 原始文獻結論 | `Verified` |
| **Slide 09** | 如何判斷 3D 幾何品質？Precision、Recall 與 F-score | 理解點對點最近鄰距離、Precision（準確度）、Recall（完整度）、F-score 調和平均防作弊機制與距離門檻 $\tau$ 的決定性作用 | `wiki/geometric-quality.md` | `knapitsch2017-tanks-temples.md` | 原始文獻結論 | `Verified` |
| **Slide 10** | 視覺 vs. 幾何互動比對：視覺逼真不等於形狀正確 | 核心思辨：剖析視線多對一投影抵消、半透明浮空雜訊假象；揭示為何 30 dB 模型可能在 3D 空間嚴重形變 | `wiki/evaluation-comparison.md`<br>`wiki/visual-quality.md`<br>`wiki/geometric-quality.md` | `mildenhall2021-nerf.md`<br>`kerbl2023-3dgs.md`<br>`knapitsch2017-tanks-temples.md` | 概念整合思辨 | `Verified` |
| **Slide 11** | 模型完整度與局部瑕疵保留：二手商品檢驗挑戰 | 剖析二手商品微小刮痕在點雲中的「0.01% 量級稀釋效應」、演算法平滑化偏見，以及本專案提出的雙軌制可信任檢驗框架 | `wiki/completeness-and-defect-retention.md`<br>`wiki/secondhand-application.md` | `knapitsch2017-tanks-temples.md`<br>`collins2022-abo.md` | 雙軌架構為延伸推論 | `Verified` |
| **Slide 12** | 重點總結、決策樹與隨堂互動測驗 | 整合三維度品質評估決策樹、2–3 題觀念互動測驗（含即時答題回饋）與權威文獻溯源導引 | `wiki/index.md`<br>`wiki/evaluation-comparison.md`<br>`wiki/glossary.md` | 6 篇核心證據卡 | 概念總成 | `Verified` |

---

## 2. 逐頁深度知識映射與來源追溯（Slide-by-Slide Detail）

### Slide 01：封面（Title & Learning Roadmap）
- **頁面主題**：從照片到可信任的 3D 模型：3D 重建技術與品質評估入門
- **初學者核心概念**：
  - 引言：用手機拍幾張照片就能建立 3D 模型，這項技術有多神奇？但在二手交易或工業應用中，這些模型真的可信嗎？
  - 核心痛點：為什麼有些 3D 模型在螢幕上看「栩栩如生」，買回家才發現實體刮痕被修掉、背面破大洞？
  - 全篇學習地圖：從原理入門（視差與三角測量）$\to$ 新興演算法（NeRF vs. 3DGS）$\to$ 常用資料集 $\to$ 視覺品質評估 $\to$ 幾何品質評估 $\to$ 實務瑕疵檢驗防坑指南。
- **映射之 Wiki 頁面**：
  - [`wiki/index.md`](../wiki/index.md)（知識庫導覽、核心問題意識、三維度評估框架）
- **映射之 Evidence Cards**：
  - 6 篇核心論文證據卡之總覽引言。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：無。
  - *本專案延伸解讀*：教學學習地圖編排、引導式提問。

---

### Slide 02：3D 重建是什麼？從 2D 像素到 3D 空間
- **頁面主題**：3D 重建的本質：從平面像素找回遺失的第三維度
- **初學者核心概念**：
  - 拍照的本質是**降維投影**：3D 現實世界經鏡頭投影到 2D 感光元件上，沿視線方向的「深度資訊（Depth）」完全遺失。
  - 兩大任務核心分流（消歧義）：
    1. **3D 幾何重建（3D Geometric Reconstruction）**：精準復原實體表面在 3D 空間的幾何坐標，輸出點雲（Point Cloud）或網格（Mesh）。
    2. **新視角合成（Novel View Synthesis, NVS）**：給定已知角度的照片，生成「未拍攝過視角」下的逼真 2D 照片。
  - **重要警示**：新視角合成追求的是「看起來像真照片」，不保證背後有乾淨水密的 3D 實體模型。
- **映射之 Wiki 頁面**：
  - [`wiki/foundations.md#1-核心任務從-2d-照片到-3d-空間`](../wiki/foundations.md#1-核心任務從-2d-照片到-3d-空間)
  - [`wiki/glossary.md`](../wiki/glossary.md)（新視角合成、點雲、網格定義）
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/mildenhall2021-nerf.md#2-研究問題與核心技術貢獻`](../evidence/mildenhall2021-nerf.md#2-研究問題與核心技術貢獻)：Mildenhall et al. (2021), Sec. 1, p. 405（定義 NVS 任務）。
  - [`evidence/kerbl2023-3dgs.md#21-研究背景與問題意識`](../evidence/kerbl2023-3dgs.md#21-研究背景與問題意識)：Kerbl et al. (2023), Sec. 1, p. 2。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：新視角合成（NVS）以生成高品質 2D 視角影像為最佳化目標（Mildenhall et al., 2021, Sec. 1）。
  - *本專案延伸解讀*：強調「幾何重建」與「新視角合成」之應用目標差異，提醒初學者勿將 NVS 影像直接等同於實體 CAD 網格。

---

### Slide 03：為什麼需要多角度照片？視差、三角測量與相機姿態
- **頁面主題**：立體視覺的奧祕：電腦如何靠多張照片計算立體深度？
- **初學者核心概念**：
  - **視差（Disparity）**：就像把手指放在眼前，閉左眼換閉右眼，手指會左右跳動；離相機越近的物體，在不同照片中的位移量越大。
  - **三角測量（Triangulation）**：只要知道兩台相機的位置與拍攝方向（相機姿態），兩條視線光線在 3D 空間的交會點就是物體的真實坐標。
  - **相機光線數學表達**：$\mathbf{r}(t) = \mathbf{o} + t\mathbf{d}$（光心 $\mathbf{o}$ 出發，沿方向 $\mathbf{d}$ 前進距離 $t$）。
  - **核心術語辨析（SfM vs. COLMAP）**：
    - **SfM（運動恢復結構）**：是一種「從多張無序照片自動計算相機姿態與稀疏點雲」的**演算法技術概念**。
    - **COLMAP**：是實現 SfM 的**主流開源軟體工具**。NeRF 與 3DGS 在處理真實照片時均使用 COLMAP 作為前置姿態估計工具。
- **映射之 Wiki 頁面**：
  - [`wiki/foundations.md#2-為什麼需要多角度照片視差與三角測量`](../wiki/foundations.md#2-為什麼需要多角度照片視差與三角測量)
  - [`wiki/foundations.md#3-相機光線camera-ray數學表示`](../wiki/foundations.md#3-相機光線camera-ray數學表示)
  - [`wiki/foundations.md#4-關鍵概念釐清sfm-技術概念-vs-colmap-軟體工具`](../wiki/foundations.md#4-關鍵概念釐清sfm-技術概念-vs-colmap-軟體工具)
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/mildenhall2021-nerf.md#32-體積渲染機制連續積分與離散數值求積sec-4-p-409410`](../evidence/mildenhall2021-nerf.md#32-體積渲染機制連續積分與離散數值求積sec-4-p-409410)：Mildenhall et al. (2021), Sec. 4, Eq. (1), p. 409（相機光線表示法）；Sec. 5.3, p. 413（真實場景使用 COLMAP 估算姿態）。
  - [`evidence/kerbl2023-3dgs.md#22-三大核心創新貢獻-sec-1-p-2`](../evidence/kerbl2023-3dgs.md#22-三大核心創新貢獻-sec-1-p-2)：Kerbl et al. (2023), Sec. 4, p. 4（使用 COLMAP 稀疏點雲初始化高斯點集）。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：真實場景姿態與深度範圍透過 COLMAP 估算（Mildenhall et al., 2021, p. 413; Kerbl et al., 2023, p. 4）。
  - *本專案延伸解讀*：以生活化「手指視差比喻」輔助初學者直觀理解幾何三角測量本質。

---

### Slide 04：神經輻射場（NeRF）：用神經網路記憶光影
- **頁面主題**：連續隱式神經場：把整個 3D 世界壓縮進神經網路權重
- **初學者核心概念**：
  - **隱式連續場（Implicit Continuous Field）**：不存點雲、不存網格，而是訓練一個多層感知機（MLP），輸入任意空間坐標 $(x, y, z)$ 與觀察視角 $(\theta, \phi)$，直接預測該點的「體積密度 $\sigma$」與「RGB 顏色」。
  - **關鍵法寶 1：位置編碼（Positional Encoding, $\gamma(p)$）**：
    - 解決神經網路的光譜偏見（Spectral Bias，容易學成一團模糊平滑），透過高頻正弦/餘弦函數投影，使網路能夠記憶物體的銳利邊緣與高頻細節。
  - **關鍵法寶 2：射線步進與體積渲染（Volume Rendering）**：
    - 沿著相機光線發射採樣點，利用透射率 $T_i$ 累加不透明度 $\alpha_i$ 與顏色 $c_i$，計算該像素的最終顏色。
    - **階層式採樣（Hierarchical Sampling）**：先粗採樣（Coarse 64 點）找出哪裡有物體，再在物體表面附近進行精細採樣（Fine 128 點），避免在空氣中浪費運算力。
- **映射之 Wiki 頁面**：
  - [`wiki/nerf.md`](../wiki/nerf.md)（隱式神經場、位置編碼 Eq. 4、階層採樣、體積渲染離散求積 Eq. 3）
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/mildenhall2021-nerf.md#31-神經輻射場場景表徵與網絡架構sec-3-p-407409`](../evidence/mildenhall2021-nerf.md#31-神經輻射場場景表徵與網絡架構sec-3-p-407409)：Mildenhall et al. (2021), Sec. 3, p. 407; Sec. 5.1, Eq. (4), p. 411（位置編碼公式，坐標 $L=10$、方向 $L=4$）。
  - [`evidence/mildenhall2021-nerf.md#32-體積渲染機制連續積分與離散數值求積sec-4-p-409410`](../evidence/mildenhall2021-nerf.md#32-體積渲染機制連續積分與離散數值求積sec-4-p-409410)：Mildenhall et al. (2021), Sec. 4, Eq. (3), p. 410（離散數值求積公式）。
  - [`evidence/mildenhall2021-nerf.md#33-最佳化與階層式採樣策略sec-52-p-412413`](../evidence/mildenhall2021-nerf.md#33-最佳化與階層式採樣策略sec-52-p-412413)：Mildenhall et al. (2021), Sec. 5.2, p. 412（粗採樣 $N_c=64$、精細採樣 $N_f=128$）。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：NeRF 透過 5D 輻射場與體積渲染實現高保真視角合成，但渲染單張 $800 \times 800$ 照片需在單張 GPU 上耗時約 30 秒（Mildenhall et al., 2021, Sec. 6.2）。
  - *本專案延伸解讀*：以「黑盒子記憶光影」比喻神經網絡權重，降低數學門檻。

---

### Slide 05：3D Gaussian Splatting：即時渲染的突破
- **頁面主題**：顯式高斯與即時光柵化：百萬顆發光橢球的華麗變形
- **初學者核心概念**：
  - **顯式 3D 高斯橢球（Explicit 3D Gaussians）**：跳脫神經網絡黑盒子，場景直接由數十萬到數百萬顆顯式空間高斯橢球表示。每顆高斯擁有獨立的：
    1. 空間中心位置 $x$
    2. 協方差矩陣 $\Sigma$（拆解為旋轉四元數 $q$ 與三維縮放向量 $s$ 以保證半正定性）
    3. 不透明度 $\alpha$
    4. 視角相關顏色（球諧函數 Spherical Harmonics, SH）
  - **自適應密度控制（Adaptive Density Control）**：
    - 依據視圖空間位置梯度 $\nabla_p L > \tau_p$ 偵測不足或過度重建區域：
      - **Clone（複製）**：小型高斯且位於欠重建區時，複製一顆高斯並沿梯度位移。
      - **Split（分裂）**：大型高斯且位於高方差區時，分裂為兩顆縮小（除以 $\phi=1.6$）的高斯。
      - **定期修剪（Pruning）**：透明度 $\alpha < \epsilon_\alpha$（0.005）的高斯直接刪除，重置透明度防浮空雜訊。
  - **即時分塊光柵化（Tile-based Rasterizer）**：
    - 將畫面切為 $16 \times 16$ 像素小塊，利用 GPU Radix Sort 對高斯深度排序，實現每秒 100+ 幀的即時新視角渲染。
- **映射之 Wiki 頁面**：
  - [`wiki/3dgs.md`](../wiki/3dgs.md)（顯式表徵、協方差分解、自適應控制、分塊光柵化）
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/kerbl2023-3dgs.md#31-可微-3d-高斯場景表徵-sec-4-p-4`](../evidence/kerbl2023-3dgs.md#31-可微-3d-高斯場景表徵-sec-4-p-4)：Kerbl et al. (2023), Sec. 4, Eq. (4)–(6), p. 4（3D 高斯公式與協方差矩陣分解 $\Sigma = R S S^T R^T$）。
  - [`evidence/kerbl2023-3dgs.md#32-自適應密度控制adaptive-control-of-gaussians-sec-5-pp-56`](../evidence/kerbl2023-3dgs.md#32-自適應密度控制adaptive-control-of-gaussians-sec-5-pp-56)：Kerbl et al. (2023), Sec. 5.2, p. 5; Appendix B, Algorithm 1, p. 11（Clone 與 Split 機制、尺度縮小比例 1.6、修剪閾值 0.005）。
  - [`evidence/kerbl2023-3dgs.md#33-快速可見性感知光柵化fast-differentiable-rasterizer-sec-6-pp-67`](../evidence/kerbl2023-3dgs.md#33-快速可見性感知光柵化fast-differentiable-rasterizer-sec-6-pp-67)：Kerbl et al. (2023), Sec. 6, p. 6（$16 \times 16$ tiles、GPU Radix Sort）。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：3DGS 達成即時渲染（7k 步 ~41 FPS，30k 步 ~134 FPS），且品質達到與 SOTA NeRF 相當甚至更優之水準（Kerbl et al., 2023, Table 1）。
  - *本專案延伸解讀*：以「彩色黏土發光球」比喻高斯橢球的位移、拉伸與分裂，降低幾何協方差矩陣的理解難度。

---

### Slide 06：NeRF vs. 3DGS：核心特性互動切換比對
- **頁面主題**：隱式神經場 vs. 顯式高斯：誰才是新視角合成的最佳解答？
- **初學者核心概念**：
  - 核心對比矩陣（互動切換比對）：
    1. **表徵型態**：NeRF 採隱式連續神經網絡權重（MLP 參數僅約 5 MB）；3DGS 採顯式離散幾何高斯點集（存檔需數百 MB 至 1 GB 以上）。
    2. **訓練耗時**：原始 NeRF 在單張 NVIDIA V100 GPU 需耗時 **100–300k 次迭代（1–2 天）**；3DGS 在單張 NVIDIA A6000 GPU 僅需 **30k 步（約 25–45 分鐘）**。
    3. **渲染速度**：NeRF 每生成一張畫面需發射百萬條射線、逐點查詢 MLP，速度僅約 **0.03 FPS（每幀需約 30 秒）**；3DGS 結合 GPU 分塊光柵化，幀率飆升至 **100+ FPS（支援即時互動）**。
    4. **硬體條件真相**：3DGS 的高速渲染建立在高階 GPU 顯存吞吐量上；儲存百萬顆高斯點對網路傳輸與行動端載入帶來新挑戰。
- **映射之 Wiki 頁面**：
  - [`wiki/evaluation-comparison.md#1-nerf-vs-3dgs-表徵範式全方位對比`](../wiki/evaluation-comparison.md#1-nerf-vs-3dgs-表徵範式全方位對比)
  - [`wiki/nerf.md#4-效能限制與資源消耗`](../wiki/nerf.md#4-效能限制與資源消耗)
  - [`wiki/3dgs.md#4-實驗數據硬體條件與定量基準`](../wiki/3dgs.md#4-實驗數據硬體條件與定量基準)
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/mildenhall2021-nerf.md#42-定量實驗數據table-1-p-414`](../evidence/mildenhall2021-nerf.md#42-定量實驗數據table-1-p-414)：Mildenhall et al. (2021), Table 1, p. 414; Sec. 6.2, p. 414（單張 GPU 訓練耗時與 30 秒渲染速度）。
  - [`evidence/kerbl2023-3dgs.md#42-核心定量評估數據table-1-p-8`](../evidence/kerbl2023-3dgs.md#42-核心定量評估數據table-1-p-8)：Kerbl et al. (2023), Table 1, p. 8; Sec. 7.2, p. 8（訓練耗時 25–45 分鐘、FPS 134、檔案大小 Mem）。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：3DGS 克服了 NeRF 的訓練與渲染速度瓶頸，但儲存大小顯著提升（Kerbl et al., 2023, Table 1）。
  - *本專案延伸解讀*：以「算力換時間 vs. 空間換時間」之工程哲學解讀兩者演進本質。

---

### Slide 07：3D 重建常用資料集：真實世界與真值邊界
- **頁面主題**：資料集的真相：演算法的訓練環境與「真值」的邊界
- **初學者核心概念**：
  - 為什麼 3D 資料集這麼少、這麼難做？（缺乏大規模真實照片且具備精確 3D 幾何真值的資料）。
  - **電商商品代表：ABO 資料集（Collins et al., 2022）**：
    - 數據四分法：147,702 商品刊登（涵蓋 576 種商品類型）、398,212 目錄照片、8,222 具 360° 環物影像、7,953 個專業建模 3D 網格（涵蓋 63 個類別）。
    - **相機姿態真相**：真實目錄照片絕大多數無相機姿態（僅 6,334 張人工標註姿態）；2.1M 張多視角合成影像是由虛擬 3D 網格在 Blender 中路徑追蹤渲染而得。
    - **幾何邊界**：7,953 個 3D 網格是「設計師建立的 CAD 網格（Artist-created Meshes）」，**並非實體商品物理掃描真值**，且皆為全新良品、缺乏使用刮痕。
  - **場景幾何評估代表：Tanks and Temples（Knapitsch et al., 2017）**：
    - 涵蓋 14 個測試場景（8 個中階、6 個高階）與 7 個訓練場景。
    - 採用**工業級雷射掃描儀（FARO Focus 3D X330 HDR）** 獲取毫米級真實表面點雲作為 Ground Truth。
- **映射之 Wiki 頁面**：
  - [`wiki/datasets.md`](../wiki/datasets.md)（ABO 統計數據、相機參數邊界、Tanks and Temples 雷射掃描規格）
  - [`wiki/secondhand-application.md#3-電商資料集的角色與侷限以-abo-資料集為例`](../wiki/secondhand-application.md#3-電商資料集的角色與侷限以-abo-資料集為例)
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/collins2022-abo.md#3-資料集組成與四項核心統計數據`](../evidence/collins2022-abo.md#3-資料集組成與四項核心統計數據)：Collins et al. (2022), Sec. 3, p. 21128（147k, 398k, 8.2k, 7.9k 數據）；Sec. 3, p. 21129（6,334 姿態照片與 2.1M Blender 渲染圖）。
  - [`evidence/knapitsch2017-tanks-temples.md#3-資料集分組與場景特性`](../evidence/knapitsch2017-tanks-temples.md#3-資料集分組與場景特性)：Knapitsch et al. (2017), Sec. 3, p. 3; Sec. 5, p. 5（14 測試 + 7 訓練場景、FARO 雷射規格）。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：ABO 3D 網格由專業美術人員手繪建立，用於跨域檢索與材質估計；Tanks and Temples 由工業雷射掃描獲取幾何真值。
  - *本專案延伸解讀*：嚴格區隔「全新電商 CAD 網格」與「二手實體商品瑕疵掃描」之應用邊界。

---

### Slide 08：如何判斷影像的視覺品質？PSNR、SSIM 與 LPIPS
- **頁面主題**：2D 視覺品質三大裁判：數值誤差、結構相似與人眼感知
- **初學者核心概念**：
  - 三大指標 6 大診斷對比：
    1. **PSNR（峰值訊噪比）**：逐像素比對均方誤差（$L_2$ 距離），越高越好（dB）。*盲點*：整張圖平移 1 個像素分數直接暴跌，但均勻微幅模糊扣分極少。
    2. **SSIM（結構相似性，Wang et al., 2004）**：拆解為亮度、對比度、結構三大分量（Eq. 13），越高越好（$[0, 1]$）。*優勢*：捕捉邊緣輪廓保真度；*盲點*：對微幅色彩偏差較寬容，無法驗證 3D 幾何。
    3. **LPIPS（深度感知距離，Zhang et al., 2018）**：提取預訓練深度卷積特徵計算加權感知距離（Eq. 1），越低越好（$\downarrow$）。*優勢*：最貼近人類主觀評價；*盲點*：對微小空間扭曲具平移不變性。
  - **經典指標衝突案例（Real Forward-Facing 資料集）**：
    - 在真實拍攝測試中，LLFF 的 LPIPS（0.212）優於 NeRF（0.250），但 NeRF 的 PSNR（26.50 dB）大幅超越 LLFF（24.13 dB）。
    - 剖析：LLFF 局部紋理對比銳利使深度特徵偏好評分佳，但多視角切換存在動態條紋跳動；NeRF 多視角平滑連續但局部紋理稍顯平滑。單看一個指標必受誤導！
- **映射之 Wiki 頁面**：
  - [`wiki/visual-quality.md`](../wiki/visual-quality.md)（PSNR 公式、SSIM 三分量公式 Eq. 6/9/10/13、LPIPS 公式 Eq. 1、三大指標對比矩陣、Real Forward-Facing 指標衝突）
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/wang2004-ssim.md#32-三大比較函數公式-sec-iii-b-pp-603605`](../evidence/wang2004-ssim.md#32-三大比較函數公式-sec-iii-b-pp-603605)：Wang et al. (2004), Sec. III-B, Eqs. (6), (7), (9), (10), (13), (17), pp. 603–605。
  - [`evidence/zhang2018-lpips.md#32-核心數學公式-sec-3-eq-1-p-591`](../evidence/zhang2018-lpips.md#32-核心數學公式-sec-3-eq-1-p-591)：Zhang et al. (2018), Sec. 3, Eq. (1), p. 591; BAPPS 484k 判斷數據。
  - [`evidence/mildenhall2021-nerf.md#42-定量實驗數據table-1-p-414`](../evidence/mildenhall2021-nerf.md#42-定量實驗數據table-1-p-414)：Mildenhall et al. (2021), Table 1, p. 414（Real Forward-Facing 數據對比）。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：三大指標各自衡量不同維度的影像失真（Wang et al., 2004; Zhang et al., 2018）；在真實場景中不同演算法會發生指標排序衝突（Mildenhall et al., 2021, Table 1）。
  - *本專案延伸解讀*：建立「三位一體」綜合檢驗視角，提醒初學者不能迷信單一數據。

---

### Slide 09：如何判斷 3D 幾何品質？Precision、Recall 與 F-score
- **頁面主題**：走入 3D 空間：雷射真值、點對點距離與防作弊的 F-score
- **初學者核心概念**：
  - **點對點最近鄰歐氏距離**：
    - 重建點到真值點雲最短距離 $e_{r \rightarrow \mathcal{G}} = \min \|r - g\|$（測量準確度）。
    - 真值點到重建點雲最短距離 $e_{g \rightarrow \mathcal{R}} = \min \|g - r\|$（測量完整度）。
  - **三大幾何指標（Tanks and Temples 協定）**：
    1. **精確度（Precision, $P(d)$）**：重建點中誤差小於門檻 $d$ 的比例（$\uparrow$）。衡量表面乾淨度，防浮空雜訊。
    2. **召回率（Recall, $R(d)$）**：真值點中被重建覆蓋的比例（$\uparrow$）。衡量表面完整度，防破洞缺面。
    3. **F-score（調和平均數）**：$F(d) = \frac{2 P(d) R(d)}{P(d) + R(d)}$。
  - **為什麼要用調和平均？（防作弊機制）**：
    - 作弊 A（極端保守）：整座建築只重建 10 個最準的點，Precision 99% 但 Recall 0.1%，調和平均 F-score 直接歸零！
    - 作弊 B（盲目灑點）：全空間隨機撒滿數億個雜點，Recall 99% 但 Precision 0.1%，F-score 同樣歸零！
  - **距離門檻 $\tau$ 的決定性作用**：
    - 雕像場景設 $\tau = 3\text{ mm}$，宮殿大建築設 $\tau = 30\text{ mm}$。**不同門檻下的 F-score 絕對不可直接橫向比較**！
- **映射之 Wiki 頁面**：
  - [`wiki/geometric-quality.md`](../wiki/geometric-quality.md)（最近鄰距離公式 Eqs. 3–4、Precision/Recall/F-score 公式 Eqs. 5–7、防作弊分析、距離門檻 $\tau$ 表格）
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/knapitsch2017-tanks-temples.md#4-幾何評估協定與核心數學公式`](../evidence/knapitsch2017-tanks-temples.md#4-幾何評估協定與核心數學公式)：Knapitsch et al. (2017), Sec. 6, Eqs. (3)–(7), pp. 6–7。
  - [`evidence/knapitsch2017-tanks-temples.md#6-距離門檻-tau-的關鍵作用與跨場景數據`](../evidence/knapitsch2017-tanks-temples.md#6-距離門檻-tau-的關鍵作用與跨場景數據)：Knapitsch et al. (2017), Table 1, p. 7（Family 3mm, Palace 30mm）。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：Tanks and Temples 採用調和平均 F-score 作為基準排名指標，且門檻 $\tau$ 隨場景尺度動態設定（Knapitsch et al., 2017, Sec. 6）。
  - *本專案延伸解讀*：以「作弊策略圖解」剖析調和平均之數學設計意圖，增進初學者直觀理解。

---

### Slide 10：視覺 vs. 幾何互動比對：視覺逼真不等於形狀正確
- **頁面主題**：真相大白：為什麼「照片好看」絕對不等於「形狀正確」？
- **初學者核心概念**：
  - **新視角合成的物理假象**：
    - 2D 照片是 3D 空間沿視線方向的積分。如果空間中有一團半透明彩霧，或者深度向前凹陷了 5 公分，但在拍攝的所有視角下光線加權剛好抵消，演算法依然能合成出 PSNR 高達 30 dB、SSIM 高達 0.95 的絕美影像。
  - **三維度評估對照矩陣（核心思辨）**：
    - 2D 視覺指標（PSNR/SSIM/LPIPS）：測量像素與感知相似度，**無法約束空間頂點深度與水密性**。
    - 3D 幾何指標（Precision/Recall/F-score）：測量真實空間毫米級歐氏距離，**直接檢視模型空間形狀是否準確**。
  - **警示紅線**：NeRF 與 3DGS 論文均以 2D 視覺指標（PSNR/SSIM/LPIPS）為主要驗證手段，全文均未直接提供 Tanks and Temples 的 3D 幾何 F-score。評估模型絕不可張冠李戴！
- **映射之 Wiki 頁面**：
  - [`wiki/evaluation-comparison.md#2-2d-視覺品質-vs-3d-幾何品質對照矩陣`](../wiki/evaluation-comparison.md#2-2d-視覺品質-vs-3d-幾何品質對照矩陣)
  - [`wiki/visual-quality.md#8-防誤導紅線視覺逼真--幾何正確`](../wiki/visual-quality.md#8-防誤導紅線視覺逼真--幾何正確)
  - [`wiki/geometric-quality.md#1-為什麼需要獨立的-3d-幾何品質評估`](../wiki/geometric-quality.md#1-為什麼需要獨立的-3d-幾何品質評估)
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/mildenhall2021-nerf.md#52-本專案延伸研究邊界明確標註需其他文獻支持`](../evidence/mildenhall2021-nerf.md#52-本專案延伸研究邊界明確標註需其他文獻支持)：Mildenhall et al. (2021), Sec. 6（未直接驗證 3D 幾何指標）。
  - [`evidence/kerbl2023-3dgs.md#52-本專案延伸研究邊界明確標註`](../evidence/kerbl2023-3dgs.md#52-本專案延伸研究邊界明確標註)：Kerbl et al. (2023), Table 1（僅列 2D 渲染指標）。
  - [`evidence/knapitsch2017-tanks-temples.md#1-文獻基本資訊與學術定位`](../evidence/knapitsch2017-tanks-temples.md#1-文獻基本資訊與學術定位)：Knapitsch et al. (2017), Sec. 1。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：NeRF 與 3DGS 官方驗證集中於 NVS 2D 品質；幾何精確度需透過 3D 真值點雲獨立驗證。
  - *本專案延伸解讀*：提出「浮空雜訊抵消假象」與「跨維度思辨架構」，作為初學者辨析模型真偽的關鍵心法。

---

### Slide 11：模型完整度與局部瑕疵保留：二手商品檢驗挑戰
- **頁面主題**：二手交易的信任考驗：演算法會把刮痕「自動美容」掉嗎？
- **初學者核心概念**：
  - 二手電商痛點：買家最怕買到刮痕機身、裂痕螢幕，需要 $360^\circ$ 旋轉無死角檢視。
  - **核心矛盾：演算法的平滑化偏見（Over-smoothing Bias）**：
    - 演算法為了追求好看、消除噪點，傾向將微小的幾何突變視為噪聲而抹平。
  - **全域 F-score 盲區：0.01% 量級稀釋效應**：
    - 一條長 5 mm、寬 0.2 mm 的刮痕，在數百萬點雲中佔比**低於 0.01%**。
    - 即使演算法把這條刮痕徹底抹平，全域 F-score 依然能拿下 **90%+ 的超高分**！買家在 3D 模型上看似完美，收到實體卻發現嚴重瑕疵。
  - **本專案延伸：可信任二手 3D 檢驗雙軌制框架**：
    - **第一軌（全域結構覆蓋度）**：檢驗宏觀外觀與盲區有無大破洞（Tanks and Temples 協定，Recall > 90%）。
    - **第二軌（局部缺陷保留度）**：在易損關鍵區域（ROI），以嚴苛門檻（如 $\tau_{\text{local}} = 0.2\text{ mm}$）檢驗實體刮痕深度殘差與法向量曲率。
- **映射之 Wiki 頁面**：
  - [`wiki/completeness-and-defect-retention.md`](../wiki/completeness-and-defect-retention.md)（稀釋效應、平滑化偏見、二手相機案例、雙軌檢驗框架）
  - [`wiki/secondhand-application.md`](../wiki/secondhand-application.md)（二手電商資訊不對稱、消除盲區、ABO 局限性）
- **映射之 Evidence Cards 與具體章節出處**：
  - [`evidence/knapitsch2017-tanks-temples.md#4-幾何評估協定與核心數學公式`](../evidence/knapitsch2017-tanks-temples.md#4-幾何評估協定與核心數學公式)：Recall 幾何覆蓋度定義。
  - [`evidence/collins2022-abo.md#52-本專案延伸研究邊界明確標註`](../evidence/collins2022-abo.md#52-本專案延伸研究邊界明確標註)：ABO 僅含全新良品、缺乏刮痕標註之客觀局限。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：Knapitsch et al. (2017) 定義全域 Recall；Collins et al. (2022) ABO 為全新電商商品資料集。
  - *本專案延伸解讀*：0.01% 點雲稀釋效應、二手相機刮痕案例、雙軌制檢驗框架（全域覆蓋 + 局部 ROI 缺陷保留）為**本專案針對二手電商應用所提出之延伸分析架構與假說推論**。

---

### Slide 12：重點總結、決策樹與隨堂互動測驗
- **頁面主題**：融會貫通：可信任 3D 模型評估決策樹與觀念隨堂測驗
- **初學者核心概念**：
  - **可信任評估三步決策樹**：
    1. 你的應用需要什麼？（展示照片看 2D 視覺；實體量測/夾取看 3D 幾何；二手檢驗看局部缺陷）。
    2. 檢查 2D 指標：PSNR 是否 > 28 dB？SSIM 是否 > 0.90？LPIPS 是否 < 0.15？多視角影片有無動態閃爍？
    3. 檢查 3D 幾何：是否有雷射真值？指定門檻 $\tau$ 是多少？F-score 是否兼顧 Precision 與 Recall？
    4. 檢查局部瑕疵：重要盲區有無破洞？微小凹痕與刮痕是否有空間深度？
  - **隨堂互動測驗（3 題即時回饋）**：
    - Q1：若一個 3D 模型在新視角渲染下取得 PSNR 35 dB、SSIM 0.98，是否代表其 3D 幾何表面必定精準無誤？（答案：否，視覺逼真不保證幾何正確）。
    - Q2：在幾何品質評估中，若演算法在全空間胡亂撒滿密集噪點，哪一項指標可能會虛高？（答案：Recall 虛高，但 Precision 與 F-score 暴跌）。
    - Q3：在 Tanks and Temples 評估中，可以在 3 mm 門檻下的得分與 30 mm 門檻下的得分直接比較排名嗎？（答案：絕對不行，門檻尺度不同無可比性）。
  - **權威文獻溯源導引**：指引讀者查閱 Wiki 專頁與 Evidence Cards。
- **映射之 Wiki 頁面**：
  - [`wiki/index.md`](../wiki/index.md)
  - [`wiki/evaluation-comparison.md#3-跨方法評估可信度評級矩陣`](../wiki/evaluation-comparison.md#3-跨方法評估可信度評級矩陣)
  - [`wiki/glossary.md`](../wiki/glossary.md)
- **映射之 Evidence Cards 與具體章節出處**：
  - 綜合 6 篇核心證據卡之核心結論。
- **學術原始結論 vs. 本專案延伸解讀**：
  - *學術原始結論*：各指標定義與實驗協定。
  - *本專案延伸解讀*：三步決策樹與互動隨堂測驗設計。

---

## 3. 知識缺口查核（Knowledge Gap Analysis）

在啟動 HTML 簡報生成前，依據 `AGENTS.md` 規範對全篇 12 頁簡報所需知識點進行完備性查核：

| 檢查項目 | 簡報所需知識點 | Wiki 與 Evidence Cards 現狀 | 查核結論 |
| :--- | :--- | :--- | :---: |
| **多視角幾何基礎** | 視差、三角測量、相機光線公式、SfM vs COLMAP | `wiki/foundations.md`（Sec. 1–4 完整包含公式與定義） | **充足，無缺口** |
| **NeRF 演算法原理** | 5D 輻射場、位置編碼、體積渲染求積、階層採樣、V100 硬體 | `wiki/nerf.md` 與 `evidence/mildenhall2021-nerf.md` 完整記載 | **充足，無缺口** |
| **3DGS 演算法原理** | 顯式高斯橢球、協方差矩陣、自適應控制、分塊光柵化、A6000 硬體 | `wiki/3dgs.md` 與 `evidence/kerbl2023-3dgs.md` 完整記載 | **充足，無缺口** |
| **資料集真值性質** | ABO 統計數據（147k/398k/8.2k/7.9k）、姿態真相、Tanks and Temples 雷射規格 | `wiki/datasets.md` 與 `collins2022-abo.md` / `knapitsch2017-tanks-temples.md` 完整記載 | **充足，無缺口** |
| **2D 視覺品質評估** | PSNR 公式、SSIM 三分量公式（Eq. 6/9/10/13）、LPIPS 特徵距離（Eq. 1）、指標衝突 | `wiki/visual-quality.md` 與 `wang2004-ssim.md` / `zhang2018-lpips.md` 完整記載 | **充足，無缺口** |
| **3D 幾何品質評估** | 最近鄰距離、Precision/Recall/F-score 公式、調和平均防作弊、門檻 $\tau$ 尺度 | `wiki/geometric-quality.md` 與 `knapitsch2017-tanks-temples.md` 完整記載 | **充足，無缺口** |
| **二手瑕疵與檢驗** | 0.01% 點雲稀釋效應、演算法平滑化偏見、二手相機案例、雙軌制檢驗框架 | `wiki/completeness-and-defect-retention.md` 與 `wiki/secondhand-application.md` 完整記載 | **充足，無缺口** |

**查核結論**：
**目前 6 篇核心文獻（P1, P2, P3, P6, P7, P8）與 Wiki 既有 11 個主題頁面之知識儲備 100% 完備，完全涵蓋 12 頁簡報所需的所有概念、公式、數據與案例。無需額外 Ingest 新文獻（包括 P9），無任何阻礙簡報製作的知識缺口。**

---

## 4. 單一事實來源（SSOT）與邊界防護承諾

1. **嚴格閉環**：簡報中的每一段文字、數據與圖表，均可在本映射表中找到精確對應之 Wiki 章節與 Evidence Card 頁碼。
2. **嚴禁超綱**：簡報不引用未 Ingest 之文獻（如 P4 uCO3D, P5 Survey, P9 PSGN）。
3. **明確標籤抽屜**：在每頁 HTML 簡報下方，均設計可展開的「知識來源抽屜（Source Drawer）」，公開展示對應的作者、年份、章節、頁碼或延伸推論標籤。
4. **邊界誠實性**：對於「二手電商應用」、「0.01% 刮痕稀釋效應」及「雙軌制檢驗框架」，明確標示為「本專案延伸分析架構」，絕不偽裝為原始論文作者已完成之實驗結論。
