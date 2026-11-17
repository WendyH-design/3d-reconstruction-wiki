# 論文結構化證據卡：P8 - Knapitsch et al. (2017) Tanks and Temples

本文件為 **P8 奠基論文（Tanks and Temples: Benchmarking Large-Scale Scene Reconstruction）** 之單一事實來源證據卡（SSOT Evidence Card）。所有收錄之資料集分組、地面真值（Ground Truth）雷射掃描規格、幾何評估公式、距離門檻 $\tau$、章節、頁碼及研究邊界均經原始文獻 `raw/P8_Tanks_and_Temples_Knapitsch_SIGGRAPH2017.pdf` 與官方網站核實，為後續 `wiki/` 頁面與教學教材之事實依據。

---

## 1. 文獻基本資料與官方來源（Source Verification）

- **正式論文標題**：*Tanks and Temples: Benchmarking Large-Scale Scene Reconstruction*
- **簡稱引用**：Knapitsch et al. (2017)
- **正式作者完整名單**：
  1. Arno Knapitsch (Intel Labs)
  2. Jaesik Park (Intel Labs)
  3. Qian-Yi Zhou (Intel Labs)
  4. Vladlen Koltun (Intel Labs)
- **出版來源**：ACM Transactions on Graphics (SIGGRAPH 2017), Vol. 36, Issue 4, Article 78, pp. 1–13
- **出版年份**：2017 年
- **官方來源 / Article DOI**：[Article DOI: 10.1145/3072959.3073599](https://doi.org/10.1145/3072959.3073599)
- **官方研究網站與排行榜**：[https://www.tanksandtemples.org/](https://www.tanksandtemples.org/)
- **本地原始檔案保存**：`raw/P8_Tanks_and_Temples_Knapitsch_SIGGRAPH2017.pdf`（13 頁完整版，經官方儲存庫核實，符合 `.gitignore` 規範不入 Git）

---

## 2. 研究目的與基準核心價值

### 2.1 欲解決之痛點 [Sec. 1 & 2, pp. 1–3]
- **實驗室小型物體資料集的侷限**：早期 3D 重建基準（如 Middlebury）主要集中於受控光照條件下的小型桌上物體，缺乏真實世界複雜大場景的幾何與光學挑戰。
- **真實大場景幾何評估之匱乏**：雖然網路照片集（如 Bundler 資料集）包含大場景，但缺乏高精度的實體 3D 地面真值（Ground Truth），難以對重建演算法進行客觀定量的空間幾何誤差測量。
- **從孤立子問題到完整 Pipeline 評估**：傳統評估常將運動恢復結構（SfM）與多視角立體視覺（MVS）割裂評估，Tanks and Temples 旨在評估「從高解析度影像到密集 3D 點雲」的完整重建管線。

### 2.2 本專案的知識定位
- **從 2D 視覺品質走向 3D 空間幾何品質**：本專案前述之 PSNR、SSIM 與 LPIPS 均為評估渲染影像的 2D 視覺品質；Tanks and Temples 是本專案探討 **3D 空間真實幾何形狀準確度（Geometric Quality）** 的最核心基準。

---

## 3. 資料集分組與場景特性

論文正式基準嚴格區分為兩大測試難度組別，官方網站另提供訓練組，**嚴禁混為一談** [Sec. 3 & 5, Table 1, pp. 4–7]：

### 3.1 三大資料分組規模
1. **中階測試組（Intermediate Dataset，8 個測試場景）**：
   - 包含大型雕塑、車輛、戶外遊樂場與房屋尺度建築（如 *Family*, *Francis*, *Horse*, *Lighthouse*, *M60*, *Panther*, *Playground*, *Train*）。
   - 地面真值對外保密，由官方伺服器統一線上評分。
2. **高階測試組（Advanced Dataset，6 個測試場景）**：
   - 包含極大規模室內環境與複雜大型建築群（如 *Auditorium*, *Ballroom*, *Courtroom*, *Museum*, *Palace*, *Temple*）。
   - 挑戰更嚴苛的光照落差、弱紋理大白牆與大範圍視角盲區。
3. **訓練組（Training Dataset，額外提供 7 個場景）**：
   - 官方網站公開提供點雲 Ground Truth 以供演算法調參訓練（如 *Barn*, *Caterpillar*, *Church*, *Courthouse*, *Ignatius*, *Meetingroom*, *Truck*）。
   - **注意**：訓練組共 7 個場景，與正式測試的 14 個 Benchmark 場景互不重疊。

### 3.2 輸入資料形式：高解析度影片（Video Streams）
- 採用高階電影級設備（搭配專業快速鏡頭與三軸雲台穩定器）拍攝 4K 高解析度環繞視訊。
- 演算法可從連續影片中抽取影格作為輸入，支援傳統 SfM+MVS 離散相機框架，亦相容新興連續視訊即時重建方法。

---

## 4. 地面真值（Ground Truth）取得流程與物理限制

### 4.1 工業級雷射掃描流程 [Sec. 4.1, pp. 6–7]
1. **硬體設備**：採用工業級 **FARO Focus 3D X330 HDR** 雷射掃描儀（有效測距達 330 公尺，測距噪聲在 10.2 公尺處僅 0.1 mm，在 22.7 公尺處僅 0.3 mm）。
2. **多測站掃描（Multi-station Scanning）**：
   - 為克服遮擋與視角盲區，每個場景均從多個重疊位置進行全面掃描（例如小型雕像掃描 4 站；火車掃描 8–10 站；大型宮殿 *Palace* 掃描 14 站，耗時兩天；神廟 *Temple* 掃描 17 站）。
3. **點雲配準與整合（Registration）**：
   - 使用 FARO 官方專業軟體，透過目標標籤與迭代最近點（ICP）演算法完成全域點雲對齊。
4. **離群點清理與評估體積裁切（Cropping Volume）**：
   - 手工剔除動態物體（如穿行路人、移動車輛）、植被與遠景，並設定三維邊界盒（Bounding Volume），僅評估指定核心重建區域。
5. **體素網格均勻重採樣（Voxel Grid Resampling）**：
   - 為了消除測站近處點雲過密、遠處稀疏的不均勻性，使用體素網格進行重採樣。
   - 體素尺寸精確設定為 **$\tau / 2$**（其中 $\tau$ 為該場景之距離門檻），落在同一體素內的點僅保留空間平均點。

### 4.2 地面真值的客觀物理限制（非零誤差模型） [Sec. 4.1, pp. 6–7]
- **光學與材質假影**：鏡面高光、光滑金屬（如 *Horse* 青銅雕像）與透明玻璃會造成雷射光束散射或穿透，導致表面噪聲或局部資料空洞。
- **配準殘留誤差**：多個測站進行全域 ICP 對齊時，累積殘留誤差在數毫米範圍內。
- **教學核心觀念**：**工程上的 Ground Truth 是極高精度的參考基準，但絕對不等於數學上「零誤差」的完美曲面**。

---

## 5. 核心幾何評估指標與數學公式

在將重建結果點雲 $\mathcal{R}$ 與真值點雲 $\mathcal{G}$ 進行剛體對齊後，透過點對點最近鄰距離進行評估 [Sec. 6, pp. 8–9, Eqs. (3)–(7)]：

### 5.1 最近鄰距離定義 [Eqs. (3) & (4), p. 8]
- 重建點 $r \in \mathcal{R}$ 到真值表面 $\mathcal{G}$ 的最短歐氏距離：
  $$e_{r \rightarrow \mathcal{G}} = \min_{g \in \mathcal{G}} \|r - g\|$$
- 真值點 $g \in \mathcal{G}$ 到重建表面 $\mathcal{R}$ 的最短歐氏距離：
  $$e_{g \rightarrow \mathcal{R}} = \min_{r \in \mathcal{R}} \|g - r\|$$

### 5.2 精確度（Precision / Accuracy）公式 [Sec. 6, Eq. (5), p. 9]
$$P(d) = \frac{100}{|\mathcal{R}|} \sum_{r \in \mathcal{R}} [e_{r \rightarrow \mathcal{G}} < d]$$
- **物理幾何意義**：在指定距離門檻 $d$ 下，**重建結果點雲中，有多少比例的點落在真值表面附近**。
- **代表性質**：論文中定義為 **準確度（Accuracy）**。數值越高代表重建點位越精確、空間飄浮假影與幾何噪點越少。

### 5.3 召回率（Recall / Completeness）公式 [Sec. 6, Eq. (6), p. 9]
$$R(d) = \frac{100}{|\mathcal{G}|} \sum_{g \in \mathcal{G}} [e_{g \rightarrow \mathcal{R}} < d]$$
- **物理幾何意義**：在指定距離門檻 $d$ 下，**真值表面上有多少比例的區域，在重建結果中找到了對應點**。
- **代表性質**：論文中定義為 **完整度（Completeness）**。數值越高代表演算法對物體表面的覆蓋越完整、未重建的破洞與缺漏越少。

### 5.4 綜合指標 F-score 公式 [Sec. 6, Eq. (7), p. 9]
$$F(d) = \frac{2 P(d) R(d)}{P(d) + R(d)}$$
- **物理幾何意義**：精確度（Precision）與召回率（Recall）的**調和平均數（Harmonic Mean）**。
- **為什麼不使用算術平均？**：
  - 若演算法只保守重建極少數極度確定的點（極度稀疏點雲），$P(d)$ 可能高達 99%，但 $R(d)$ 趨近於 0；
  - 若演算法在全空間瘋狂灑點填滿空間，$R(d)$ 可能達到 99%，但 $P(d)$ 趨近於 0；
  - 調和平均具備「只要其中一項趨近於 0，$F(d)$ 即暴跌趨近於 0」的嚴格特性，**迫使演算法必須在「準確度」與「完整度」之間取得兼顧**。
- **基準官方總分**：官方排行榜以預設門檻 $\tau$ 下的 **$F(\tau)$** 作為核心排名指標。

---

## 6. 距離門檻 $\tau$ 的關鍵作用與跨場景數據

### 6.1 為什麼距離門檻 $\tau$ 必須隨場景動態設定？ [Sec. 5 & 6, Table 1, p. 7]
物體與場景的物理尺度差異巨大（例如 2 公尺的雕像 vs. 數百公尺的宮殿建築群），統一採用單一絕對距離（如 5 mm）評估毫無物理意義。因此門檻 $\tau$ 依據場景尺度與掃描重採樣密度精準給定：

| 難度組別 | 代表場景名稱 | 物件/場景類型 | 物理尺度規模 | 評估距離門檻 $\tau$ |
| :--- | :--- | :--- | :--- | :---: |
| **Intermediate** | **Family** | 戶外家庭雕塑 | 中小型（約數公尺） | **3 mm** |
| **Intermediate** | **Francis** | 巨型人物雕像 | 中型雕塑 | **5 mm** |
| **Intermediate** | **Horse** | 高反光青銅雕像 | 中小型雕塑 | **3 mm** |
| **Intermediate** | **Lighthouse** | 戶外高聳燈塔 | 大型建築物 | **10 mm** |
| **Intermediate** | **M60** | 戰車金屬車體 | 大型載具 | **5 mm** |
| **Intermediate** | **Panther** | 沼澤戰車車體 | 大型載具 | **5 mm** |
| **Intermediate** | **Train** | 蒸氣火車頭 | 大型機械建築 | **5 mm** |
| **Advanced** | **Auditorium** | 大型劇院禮堂 | 大型室內空間 | **10 mm** |
| **Advanced** | **Palace** | 宏偉宮殿建築群 | 巨型建築群（跨數日拍攝） | **30 mm** |
| **Advanced** | **Temple** | 古老廟宇建築 | 巨型戶外複合建築 | **15 mm** |

### 6.2 跨論文比較防誤導禁忌
- **門檻相異不可直接比分數**：3 mm 門檻下的 F-score 45% 可能比 30 mm 門檻下的 F-score 60% 具備更高難度的幾何精確度。
- **跨論文比較必須強制附帶 $\tau$ 數值與對齊協定**。

---

## 7. 評估限制與本專案二手商品檢驗之延伸應用

### 7.1 Tanks and Temples 的本質限制
1. **尺度不對稱**：Tanks and Temples 專注於大型建築與車輛（公尺至百公尺級），而二手電商商品（相機、手機、精品手袋）為公分至毫米級。
2. **無細微瑕疵標註**：其雷射掃描 Ground Truth 旨在測量宏觀表面形狀，並未專門標註二手商品的表面刮痕、凹痕或磕碰。
3. **全域 F-score 無法反映微小瑕疵保留度**：若演算法在宏觀幾何表面上完全覆蓋（F-score 高達 90%），但表面細微的 1 mm 刮痕被平滑化消除，全域 F-score 數值幾乎不受任何影響。

### 7.2 對二手商品 3D 重建的借鑑與延伸設計
- **以高精度 3D 掃描作為幾何真值（GT）**：借鑑其使用工業級接觸式或雷射掃描儀建立實體商品的幾何 GT。
- **幾何維度的雙重驗證**：
  - 以 **Precision** 檢驗重建模型是否有浮空雜訊或邊緣膨脹；
  - 以 **Recall** 檢驗模型是否存在盲區破洞（結構覆蓋度）。
- **延伸提出「局部缺陷保留（Local Defect Retention）」**：必須在宏觀全域 F-score 之外，獨立定義**局部感興趣區域（ROI）**與高解析度表面法向量/曲率殘差分析，方能真正落實可信任的二手瑕疵檢驗。

---

## 8. 原始章節與頁碼追溯索引

- **大型場景 Benchmark 動機與核心原則**：Sec. 1–3, pp. 1–4
- **FARO Focus 3D X330 HDR 規格與多測站掃描**：Sec. 4.1, pp. 6–7
- **Voxel Grid 重採樣（$\tau/2$）與真值清理**：Sec. 4.1, pp. 6–7
- **Benchmark 測試集清單與門檻 $\tau$（Table 1）**：Sec. 5, Table 1, p. 7
- **最近鄰距離公式 Eqs. (3) & (4)**：Sec. 6, p. 8
- **Precision（Accuracy）公式 Eq. (5)**：Sec. 6, p. 9
- **Recall（Completeness）公式 Eq. (6)**：Sec. 6, p. 9
- **F-score 調和平均公式 Eq. (7)**：Sec. 6, p. 9
- **SfM 與 MVS 評估演算法清單（COLMAP 等）**：Sec. 7, pp. 9–10
