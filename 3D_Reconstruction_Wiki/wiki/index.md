# 3D 重建與品質評估知識庫導覽（Wiki Index）

歡迎來到《從照片到可信任的 3D 模型：3D 重建技術與品質評估入門》LLM Wiki 核心知識庫。本知識庫採主題式知識網絡結構，所有核心主張與數據均可追溯至 `evidence/` 資料夾中之原始學術文獻。

---

## 📚 知識庫核心主題分區

1. **基礎概念**
   - [3D 重建基礎與多視角幾何概念](foundations.md)：*(已建立，NVS 任務、相機光線、SfM vs COLMAP、隱式神經場 vs. 顯式高斯表徵)*
   - [專業術語對照辭典 (Glossary)](glossary.md)：*(已建立，中英對照、白話釋義、易混淆概念辨析)*

2. **新興重建技術**
   - [神經輻射場 (NeRF)](nerf.md)：*(已建立，連續 5D 輻射場、位置編碼、分層採樣、體積渲染、限制)*
   - [3D Gaussian Splatting (3DGS)](3dgs.md)：*(已建立，顯式 3D 高斯橢球表徵、自適應密度控制、Tile-based 快速光柵化、對應 P2)*

3. **研究與電商資料集**
   - [3D 重建常用資料集客觀評析](datasets.md)：*(已建立，ABO 電商商品資料集、Tanks and Temples 大型場景雷射真值基準、對應 P3, P8)*

4. **品質評估三維度**
   - [視覺品質評估：PSNR / SSIM / LPIPS](visual-quality.md)：*(已建立，PSNR / SSIM / LPIPS 6 大診斷問題、深度特徵加權公式、BAPPS 評估、排序衝突案例、對應 P1, P2, P6, P7)*
   - [幾何品質評估：Precision / Recall / F-score / 閾值](geometric-quality.md)：*(已建立，Tanks and Temples 工業雷射點雲真值、最近鄰距離、距離門檻 τ、調和平均防作弊機制、對應 P8)*
   - [模型完整度、結構覆蓋度與局部瑕疵保留](completeness-and-defect-retention.md)：*(已建立，全域 F-score 盲區、量級稀釋效應、演算法平滑化偏見、二手商品雙軌制檢驗構想)*
   - [品質評估三維度綜合比較表](evaluation-comparison.md)：*(已建立，跨方法比較條件矩陣、2D 視覺 vs. 3D 幾何對照與可信度標籤)*
   - **評估與推論陷阱防誤導指南**：*(待建立)*

5. **二手電商應用與實務**
   - [二手商品 3D 重建與可信任度檢驗實務](secondhand-application.md)：*(已建立，消除檢視盲區、平滑化消除瑕疵陷阱、結構覆蓋度 vs. 局部缺陷保留)*

---

## 📑 論文 Ingest 總覽紀錄
所有文獻之來源檢驗與處理進度請參閱：👉 [論文 Ingest 歷程追蹤與狀態總覽 (ingest-log.md)](ingest-log.md)
