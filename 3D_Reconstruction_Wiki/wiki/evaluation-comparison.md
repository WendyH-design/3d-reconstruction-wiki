# 3D 重建與新視角合成評估比較表（Evaluation Comparison Matrix）

本頁面為跨論文、跨方法之評估條件與效能比較矩陣。依據 `AGENTS.md` 規範：**所有跨論文比較表格強制附帶完整實驗條件，並禁止在關鍵實驗條件相異的情況下直接以單一數值進行高低排名**。

---

## 1. 跨方法比較之關鍵陷阱與原則

在比較不同 3D 重建與新視角合成方法時，必須檢視以下維度：

1. **輸入與相機條件是否一致**：輸入影像數量、拍攝解析度、相機姿態是精確真值還是估計值（如 COLMAP）。
2. **評估解析度是否一致**：不同解析度（如 $512 \times 512$ vs $800 \times 800$）會顯著影響 PSNR、SSIM 與 LPIPS 數值。
3. **評估指標實作與網路版本是否一致**：例如 LPIPS 是否使用相同的主幹網路（AlexNet vs. VGG）與相同的校準權重版本（`lin` 等），不可跨主幹架構混為一談。
4. **運算資源與訓練時間成本**：訓練耗時、硬體規格與顯存開銷差異，不能將渲染品質與幾何精準度混為一談。
5. **資料集性質與真值（Ground Truth）形式差異**：
   - **純合成 CAD 資料集（如 Synthetic NeRF）**：具備完美的幾何與相機真值，但缺乏真實世界的光學複雜度。
   - **真實商品多模態資料集（如 [ABO](datasets.md), Collins et al., 2022）**：提供真實商品目錄照片與 360° 環物照片，其 3D 模型為**由專業建模人員建立的高品質 3D 網格模型（Artist-created 3D Meshes）**，具有複雜幾何與 PBR 材質，並非對實體商品進行物理掃描所取得的幾何真值；目錄照片絕大多數無姿態標註（僅 6,334 張具備透過 3D 模型與可微分渲染程序估計之姿態標註）。
   - **真實場景光達掃描資料集（如 Tanks and Temples, Knapitsch et al., 2017）**：使用工業級 LiDAR 掃描實體物理表面作為真值，評估點雲幾何距離與 F-score。
6. **使用可信度標籤（Reliability Badges）**：
   - `[Comparable]`：實驗設定、資料集與條件高度一致，可直接客觀比較。
   - `[Partially Comparable]`：部分條件存在差異（例如評估解析度不同），可有限度比較但必須註記條件。
   - `[Non-comparable]`：條件根本不同，不具可比性。
   - `[Not Reported]`：原始論文確實未提供該項數據。

---

## 2. 典型代表性方法實作條件與效能矩陣（NeRF 基準組）

此處收錄 Mildenhall et al. (2020) 原文之完整實驗條件設定與數據 [Mildenhall et al., 2020, Table 1, Sec 5.3, 6.3](../evidence/mildenhall2020-nerf.md#42-定量實驗數據table-1-p-414)：

### 2.1 Realistic Synthetic 360° 資料集（8 個幾何複雜物體）

| 方法名稱 | 發表年份 | 3D 表示法型態 | 相機姿態 | 評估解析度 | 儲存容量 | 單卡計算時間 (V100) | PSNR (dB) $\uparrow$ | SSIM $\uparrow$ | LPIPS $\downarrow$ | 可信度標籤與條件說明 |
| :--- | :---: | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **SRN [41]** | 2019 | 連續表面 (MLP + RNN 步進) | 真值 (GT) | **512 × 512** | 數 MB | > 12 小時 | 22.26 | 0.846 | 0.170 | `[Partially Comparable]`<br>⚠️因記憶體限制使用較低解析度 (512²)。 |
| **NV [23]** | 2019 | 離散 3D 體素網格 (128³) | 真值 (GT) | 800 × 800 | 數百 MB | > 12 小時 | 26.05 | 0.893 | 0.160 | `[Comparable]`<br>✔️條件對齊，但受限於體素網格解析度。 |
| **LLFF [27]** | 2019 | 多平面影像 (MPI) 體素網格 | 真值 (GT) | 800 × 800 | **> 15 GB** | **< 10 分鐘** | 24.88 | 0.911 | 0.114 | `[Comparable]`<br>✔️渲染速度快但場景需極大儲存容量。 |
| **NeRF (Ours)** | 2020 | **連續 5D 輻射場 (MLP)** | 真值 (GT) | 800 × 800 | **~ 5 MB** | **1 至 2 天** | **31.01** | **0.947** | **0.081** | `[Comparable]`<br>✔️視角與細節表現最佳，容量小但訓練極耗時。 |

---

### 2.2 Real Forward-Facing 資料集（8 個真實前向場景）

| 方法名稱 | 發表年份 | 3D 表示法型態 | 相機姿態來源 | 評估解析度 (原始 1008×756) | PSNR (dB) $\uparrow$ | SSIM $\uparrow$ | LPIPS $\downarrow$ | 可信度標籤與條件說明 |
| :--- | :---: | :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| **SRN [41]** | 2019 | 連續表面 (MLP) | COLMAP 估算 | **504 × 376** | 22.84 | 0.668 | 0.378 | `[Partially Comparable]`<br>⚠️SRN 評估解析度減半。 |
| **NV [23]** | 2019 | 離散體素網格 | COLMAP 估算 | — | — | — | — | `[Non-comparable]`<br>⚠️NV 僅能重建封閉物體，無法處理前向大場景。 |
| **LLFF [27]** | 2019 | 多平面影像 (MPI) | COLMAP 估算 | 1008 × 752 | 24.13 | 0.798 | **0.212** | `[Comparable]`<br>⚠️**指標衝突案例**：LPIPS 表現最佳，但照片有條紋偽影。 |
| **NeRF (Ours)** | 2020 | 連續 5D 輻射場 (MLP) | COLMAP 估算 | 1008 × 752 | **26.50** | **0.811** | 0.250 | `[Comparable]`<br>✔️PSNR 與 SSIM 顯著領先，全視角平滑度最佳。 |

---

## 3. NeRF (P1) vs. 3D Gaussian Splatting (P2) 跨方法條件比較矩陣

本節比對隱式連續場代表方法 **NeRF** [Mildenhall et al. (2020)](../evidence/mildenhall2020-nerf.md) 與顯式高斯點集代表方法 **3DGS** [Kerbl et al. (2023)](../evidence/kerbl2023-3dgs.md)。

> [!IMPORTANT]
> 兩篇論文在評估基準、測試硬體與場景設定上存在差異：
> - NeRF 主要在 Synthetic 360°（800×800）與 Real Forward-Facing（1008×752）評估，測試硬體為 V100 / Titan RTX。
> - 3DGS 主要在 Mip-NeRF360、Tanks & Temples 與 Deep Blending 評估（原生高解析度），測試硬體為 NVIDIA A6000。
> - 跨論文數據必須以 `[Partially Comparable]` 標註，不可直接作排名簡化。

| 比較維度 | P1: NeRF (Mildenhall et al., 2020) | P2: 3DGS (Kerbl et al., 2023) | 可信度標籤與條件差異說明 |
| :--- | :--- | :--- | :--- |
| **場景表徵型態** | 連續隱式輻射場（5D MLP 神經網絡） | 離散顯式 3D 高斯橢球點集 | `[Non-comparable]`<br>物理與數學表徵本質完全不同（隱式場 vs. 顯式點集）。 |
| **渲染速度** | 極慢（約數秒至數十秒一幀） | 即時（在 A6000 上達 134–197 FPS） | `[Partially Comparable]`<br>兩者測試硬體（V100 vs. A6000）與場景解析度不同，但 3DGS 具備三個數量級的速度躍進。 |
| **訓練耗時** | 數小時至數天（例如 100k~300k 次迭代） | 數十分鐘（單張 A6000 上約 25–45 分鐘） | `[Partially Comparable]`<br>測試硬體與資料集不完全一致，但 3DGS 最佳化收斂顯著較快。 |
| **最佳化模型參數儲存容量** | **極小**（約 5 MB，神經網絡權重檔案） | **較大**（約 400–750 MB，百萬個高斯屬性） | `[Partially Comparable]`<br>兩者表徵結構不同（MLP 參數儲存 vs. 高斯屬性陣列儲存）。 |
| **訓練顯存需求（VRAM）** | **原論文未提供可直接比較的顯存數據**<br>（標記為 `[Not Reported]`） | **計算峰值顯存可能超過 20 GB**<br>（未優化原型在大場景訓練時） | **`[Non-comparable]`**<br>P1 缺乏直接對照的峰值 VRAM 數據；不可將 P1「單卡可跑」當作可比依據。 |
| **推論額外顯存開銷** | 無分塊暫存開銷 | 額外約 30–500 MB（光柵化 Tile 分塊暫存） | `[Non-comparable]`<br>體積積分射線採樣 vs. 分塊光柵化之硬體記憶體架構不同。 |
| **2D 新視角影像品質** | Synthetic 360°: PSNR 31.01 dB, SSIM 0.947, LPIPS 0.081 | Deep Blending: PSNR 29.41 dB, SSIM 0.903, LPIPS 0.243 | `[Partially Comparable]`<br>測試資料集與解析度不同，但均能達到頂尖的 2D 逼真影像品質。 |
| **3D 幾何真值距離評估** | **無**（未評估 Chamfer Distance / F-score） | **無**（未評估 Chamfer Distance / F-score） | **`[Not Reported in Both Papers]`**<br>⚠️**兩篇論文均未直接證明 3D 空間幾何正確**。高 PSNR 僅代表視角影像逼真，不保證幾何水密與表面精確度。 |

---

## 4. 2D 視覺品質 vs. 3D 幾何品質評估維度對照

初學者最容易混淆的觀念是將「新視角合成渲染圖很漂亮」直接等同於「3D 空間形狀正確」。依據本專案核心文獻，兩者分屬完全不同的品質維度：

| 評估維度 | 代表指標 | 核心評估目標 | 依賴之地面真值（GT） | 代表性文獻 | 評估盲點與局限 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **視覺品質**<br>(Visual Quality) | **PSNR** ($\uparrow$)<br>**SSIM** ($\uparrow$)<br>**LPIPS** ($\downarrow$) | 2D 視角渲染圖片在像素數值、空間結構與深層特徵層面的逼真度。 | 對齊之 2D 真值照片（Ground Truth Photo）。 | [Mildenhall 2020](../evidence/mildenhall2020-nerf.md)<br>[Wang 2004](../evidence/wang2004-ssim.md)<br>[Zhang 2018](../evidence/zhang2018-lpips.md) | **無法驗證 3D 幾何**：渲染逼真可能依靠半透明浮空微粒或神經平滑化達成；高分無法證明表面無破洞或微小刮痕被保留。 |
| **幾何品質**<br>(Geometric Quality) | **Precision** ($\uparrow$)<br>**Recall** ($\uparrow$)<br>**F-score** ($\uparrow$) | 3D 空間重建點雲/網格表面相對於實體物理表面的歐氏距離誤差。 | 工業級雷射掃描 3D 點雲（LiDAR GT），需指定距離門檻 $\tau$。 | [Knapitsch et al. (2017)](../evidence/knapitsch2017-tanks-temples.md) | **全域數值掩蓋局部瑕疵**：全域高 F-score 僅能證明宏觀幾何覆蓋完整，無法單獨證明毫米級局部刮痕被忠實保留。 |

> [!IMPORTANT]
> **跨論文可信度規範**：
> - 視覺指標（PSNR/SSIM/LPIPS）數值**不可**跨解析度、跨裁切條件或跨主幹架構直接排名。
> - 幾何指標（Precision/Recall/F-score）數值**不可**跨不同距離門檻 $\tau$ 直接排名。

---

## 5. 本頁參考之文獻證據卡

- [Mildenhall et al., 2020](../evidence/mildenhall2020-nerf.md)：收錄 Table 1 完整數據、評估解析度設定（Sec. 6.2, p. 414）與儲存容量對比（Sec. 6.3, p. 417）。
- [Kerbl et al., 2023](../evidence/kerbl2023-3dgs.md)：收錄 Table 1 評估數據（Sec. 7.2, p. 8）、A6000 硬體環境與訓練顯存開銷（Sec. 7.4, p. 11）。
- [Collins et al., 2022](../evidence/collins2022-abo.md)：闡明商品級資料集特性、Artist-created 網格模型與相機姿態標註邊界（Sec. 3, pp. 21128–21130）。
- [Wang et al., 2004](../evidence/wang2004-ssim.md)：收錄 SSIM 結構相似性評估架構、局部高斯視窗規格與影像品質診斷維度（Sec. III–IV, pp. 603–606）。
- [Zhang et al., 2018](../evidence/zhang2018-lpips.md)：收錄 LPIPS 深度特徵感知距離評估協定、Eq. (1) 特徵加權與跨架構評估（Sec. 3–5, pp. 586–595）。
- [Knapitsch et al., 2017](../evidence/knapitsch2017-tanks-temples.md)：收錄 Tanks and Temples 基準資料集分組、FARO 雷射掃描真值規格、Precision / Recall / F-score 幾何評估公式與場景距離門檻（Table 1, Sec. 3–6）。
