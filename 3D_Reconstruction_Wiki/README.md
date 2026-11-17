# 從照片到可信任的 3D 模型：3D 重建技術與品質評估入門

> **以學術論文為根基的 3D 重建與品質評估 LLM Wiki 知識庫**  
> 參考 Andrej Karpathy 提出的 [LLM Wiki 構想](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)，建立具備高追溯性、嚴謹證據鏈與單一事實來源（SSOT）的個人研究與教學知識庫。

---

## 📖 專案簡介

在電腦視覺與電腦圖學領域，「從多張照片重建出 3D 模型」已從傳統的運動恢復結構（SfM）與多視角立體視覺（MVS），跨入神經輻射場（NeRF）與 3D 高斯潑濺（3D Gaussian Splatting, 3DGS）的新時代。

然而，許多初學者常陷入一個關鍵盲點：**「在新視角下看起來極度逼真的模型，其 3D 幾何形狀可能完全崩壞或充滿破洞。」**

本專案旨在為完全沒有電腦視覺、3D 重建或深度學習背景的初學者與產品評估者，打造一套由淺入深的知識體系：
1. **理解成像到立體**：多角度照片如何透過視差與幾何約束重建成 3D 模型。
2. **掌握前沿技術**：NeRF（隱式神經連續場）與 3DGS（顯式高斯光柵化）的原理、特性與適用邊界。
3. **認識權威資料集**：真實物件多視角影片（uCO3D）、電商商品模型（ABO）與場景基準（Tanks and Temples）。
4. **剖析品質三維度**：嚴格劃分**視覺品質**（PSNR/SSIM/LPIPS）、**幾何品質**（Chamfer Distance/F-score）與**模型完整度及局部缺陷保留**。
5. **落地二手商品實務**：探討商品表面刮痕、凹陷在重建過程中為何容易消失，以及如何評估 3D 模型在瑕疵檢驗中的可信度。

---

## 🗂️ 專案目錄結構

本專案採用**雙層混合架構（Hybrid Architecture）**，落實「原始文獻 $\to$ 證據卡 $\to$ 百科概念頁 $\to$ 教學簡報」的單向可追溯知識流：

```
3D_Reconstruction_Wiki/
├── .gitignore                      # 排除原始版權 PDF 與大型資料集檔案
├── AGENTS.md                       # 最高指導手冊：代理人維護、引用與品質規範
├── README.md                       # 專案導覽與使用手冊（本文件）
│
├── raw/                            # 【原始文獻層】（本地保存，不入 Git）
│   └── (保留各篇官方論文之全文 PDF 與版本取得紀錄)
│
├── evidence/                       # 【文獻證據層】（公開）
│   └── (每篇論文的結構化證據卡，記載章節、印刷/PDF 頁碼、數據與圖表編號)
│
├── wiki/                           # 【概念知識百科層】（公開，單一事實來源）
│   ├── index.md                    # 知識庫總導覽與主題索引
│   ├── foundations.md              # 3D 重建基礎與多視角影像概念
│   ├── nerf.md                     # 神經輻射場（NeRF）原理與邊界
│   ├── 3dgs.md                     # 3D Gaussian Splatting 原理與渲染機制
│   ├── datasets.md                 # 3D 重建常用資料集客觀解析
│   ├── visual-quality.md           # 視覺品質評估（PSNR, SSIM, LPIPS）
│   ├── geometric-quality.md        # 幾何品質評估（Chamfer Distance, F-score）
│   ├── completeness-and-defect-retention.md # 完整度、結構覆蓋度與瑕疵保留
│   ├── evaluation-comparison.md    # 品質評估三維度整合比較表
│   ├── evaluation-pitfalls.md      # 跨論文評估陷阱防範指南
│   ├── secondhand-application.md   # 二手商品 3D 重建與可信度檢驗案例
│   ├── glossary.md                 # 中英文專業術語對照辭典
│   └── ingest-log.md               # 論文 Ingest 歷程與驗收紀錄
│
└── slides/                         # 【互動教學簡報層】（公開 / GitHub Pages）
    ├── index.html                  # 單一檔案（Single-file）RWD 互動教學簡報
    └── slide_source_map.md         # 投影片與 Wiki/Evidence 的 100% 來源映射表
```

---

## 📚 核心收錄文獻地圖（8 篇核心 + 1 篇補充）

所有文獻均經由 CVF Open Access、ACM Digital Library、IEEE Xplore 等官方管道核對正式元資料：

| 編號 | 論文名稱 | 引用簡稱 | 出版來源 / 年份 | DOI / 官方連結 | 核心概念與用途 |
| :---: | :--- | :--- | :---: | :--- | :--- |
| **P1** | *NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis* | Mildenhall et al. (2020) | ECCV 2020 | [10.1007/978-3-030-58452-8_24](https://doi.org/10.1007/978-3-030-58452-8_24) | NeRF 開創性論文：連續 5D 輻射場與體積渲染。 |
| **P2** | *3D Gaussian Splatting for Real-Time Radiance Field Rendering* | Kerbl et al. (2023) | ACM TOG (SIGGRAPH 2023) | [Article: 10.1145/3592433](https://doi.org/10.1145/3592433) | 3DGS 開創性論文：顯式高斯表示與即時光柵化渲染。 |
| **P3** | *ABO: Dataset and Benchmarks for Real-World 3D Object Understanding* | Collins et al. (2022) | CVPR 2022 | [10.1109/CVPR52688.2022.02045](https://doi.org/10.1109/CVPR52688.2022.02045) | 電商商品多視角與 3D 模型資料集。 |
| **P4** | *UnCommon Objects in 3D* | Liu et al. (2025) | CVPR 2025 (arXiv 2023) | [10.1109/CVPR52734.2025.01316](https://doi.org/10.1109/CVPR52734.2025.01316) | 跨 1,000 種物件類別之多視角影片與 3D 標註資料集。 |
| **P5** | *Deep-Learning-Based 3-D Surface Reconstruction—A Survey* | Farshian et al. (2023) | Proc. IEEE 2023 | [10.1109/JPROC.2023.3321433](https://doi.org/10.1109/JPROC.2023.3321433) | 3D 表面重建權威綜述：表示法與幾何評估協定。 |
| **P6** | *Image Quality Assessment: From Error Visibility to Structural Similarity* | Wang et al. (2004) | IEEE TIP 2004 | [10.1109/TIP.2003.819861](https://doi.org/10.1109/TIP.2003.819861) | 結構相似性（SSIM）奠基論文：結構互補評估框架。 |
| **P7** | *The Unreasonable Effectiveness of Deep Features as a Perceptual Metric* | Zhang et al. (2018) | CVPR 2018 | [10.1109/CVPR.2018.00068](https://doi.org/10.1109/CVPR.2018.00068) | 深度特徵感知距離（LPIPS）奠基論文。 |
| **P8** | *Tanks and Temples: Benchmarking Large-Scale Scene Reconstruction* | Knapitsch et al. (2017) | ACM TOG (SIGGRAPH 2017) | [Article: 10.1145/3072959.3073599](https://doi.org/10.1145/3072959.3073599) | 場景級光達真值基準：Precision / Recall / F-score ($\tau$)。 |
| **P9** | *A Point Set Generation Network for 3D Object Reconstruction From a Single Image* | Fan et al. (2017) | CVPR 2017 | [10.1109/CVPR.2017.264](https://doi.org/10.1109/CVPR.2017.264) | 點集生成補充文獻：Chamfer Distance 於點集任務之公式與應用。 |

---

## 🎯 品質評估三維度消歧義架構

本專案拒絕將所有評估指標混為一談，明確區分三個獨立且互補的評估面向：

```
                              ┌──────────────────────────────────────────────┐
                              │            3D 重建品質全面評估架構             │
                              └──────────────────────┬───────────────────────┘
                                                     │
         ┌───────────────────────────────────┼───────────────────────────────────┐
         ▼                                   ▼                                   ▼
┌──────────────────┐               ┌──────────────────┐                ┌──────────────────┐
│   視覺品質評估    │               │   幾何品質評估    │                │ 完整度與缺陷保留 │
├──────────────────┤               ├──────────────────┤                ├──────────────────┤
│• 關注：影像逼真度 │               │• 關注：表面形狀真值│                │• 結構覆蓋度      │
│• 指標：PSNR      │               │• 指標：Chamfer   │                │  (背面/底部破洞) │
│        SSIM      │               │        F-score(τ)│                │• 局部缺陷保留    │
│        LPIPS     │               │• 幾何完整度 (τ)  │                │  (微小刮痕/裂痕) │
│• 盲點：無法證明   │               │• 盲點：巨觀精準不│                │• 區分：標準指標  │
│        幾何正確  │               │        代表微瑕保留│                │        vs 專案延伸│
└──────────────────┘               └──────────────────┘                └──────────────────┘
```

---

## 🚦 自動化檢驗與人工確認關卡（Human-in-the-Loop）

為了確保知識庫的客觀性與無幻覺，專案設計了嚴密的六大人工確認關卡與七項自動化檢驗：

### 人工確認節點（Human Gates）
- **Gate 1**：候選論文收錄決策（已審定通過）。
- **Gate 2**：單篇論文 Ingest 前之閱讀分析方案確認。
- **Gate 3**：跨論文數據衝突與學術爭議裁決。
- **Gate 4**：Wiki 概念頁面與比較表階段性驗收。
- **Gate 5**：HTML 簡報大綱與來源映射表確認。
- **Gate 6**：HTML 簡報瀏覽器操作體驗與最終驗收。

### 狀態追蹤準則（Unresolved Tracker）
- **`Needs Verification` / `Pending` / `Blocked` / `Unknown`**：嚴禁進入 HTML 簡報作為已確認事實。
- **`Not Reported`**：若已核實原始論文確實未交代該條件，允許在簡報中如實呈現為「原論文未提供」，作為客觀限制說明。

---

## 🛠️ 開發與維護守則

詳細之代理人操作指引、論文 Ingest 六階段流程、DoD 清單與簡報退回路由，請參閱：
👉 [**AGENTS.md：3D 重建與品質評估 LLM Wiki 維護與執行手冊**](file:///c:/Users/USER/Desktop/3D_Reconstruction_Wiki/AGENTS.md)
