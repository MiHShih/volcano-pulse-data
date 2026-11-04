# 火山脈動（Volcano Pulse）資料包

全球 Holocene 火山的基本資料與最近活動，由個人的資料站每天自動更新一次（約 01:00 UTC），供「火山脈動」3D 地球展示程式讀取。

**本資料非官方預警，警戒資訊以各國火山觀測機構公告為準。**

## 檔案

- `bundle.json`：資料包，網址 <https://mihshih.github.io/volcano-pulse-data/bundle.json>
- 每天的 commit 就是當天的快照，歷史資料可以從 commit 紀錄取回。

## 格式（schema 1）

| 欄位 | 內容 |
|---|---|
| `schema` | 格式版本號。格式有不相容的變更時才會加 1，讀取端遇到不認得的版本應該忽略 |
| `generated_utc` | 資料包產生時間 |
| `meta` | 各來源的抓取時間、筆數、錯誤，以及 GVP 資料庫版本與 DOI；`meta.updated_utc` 是資料更新時間 |
| `volcanoes` | Holocene 火山：`vnum`（GVP Volcano Number）、`name`、`country`、`type`、`lat`、`lon`、`last_eruption`、`evidence` |
| `alerts` | 目前警戒升高中的火山（USGS HANS，只涵蓋美國） |
| `activity` | 活動歷史，`source` 為 `rss`（官方週報）、`hans`（HANS 警戒變動）或 `claude`（網路搜尋補充） |

- 所有時間都是 UTC。
- 各來源一律用 GVP Volcano Number（`vnum`）串接。
- 任一來源抓取失敗時沿用上一份資料，錯誤記在 `meta`。

## Claude 補充（未經官方確認）

`source` 為 `claude` 的紀錄由 Claude 以網路搜尋產生，用來補官方來源還沒涵蓋的最近 7 天活動：

- 一律標記 `verified: false`，每筆都附來源網址（`url`）。
- `eruption` 是 Claude 判斷當天是否有噴發，可能誤判。
- 摘要為繁體中文；`reported_name` 保留 Claude 回報的火山名稱。

請以原始來源為準。

## 資料來源與引用

- Global Volcanism Program, 2026. [Database] Volcanoes of the World (v. 5.4.0; 7 Aug 2026). Distributed by Smithsonian Institution, compiled by Venzke, E. <https://doi.org/10.5479/si.GVP.VOTW5-2026.5.4>（最新版本見 `meta.gvp`）
- Smithsonian / USGS Weekly Volcanic Activity Report：<https://volcano.si.edu/news/WeeklyVolcanoRSS.xml>
- USGS Volcano Hazards Program HANS：<https://volcanoes.usgs.gov/hans-public/>

各來源資料的權利屬於原提供單位。
