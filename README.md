# 火山脈動（Volcano Pulse）資料包

全球 Holocene 火山的基本資料與最近活動，由個人的資料站每天自動更新一次（約 01:00 UTC），供「火山脈動」3D 地球展示程式讀取。

**本資料非官方預警，警戒資訊以各國火山觀測機構公告為準。**

## 檔案

- `bundle.json`：資料包，網址 <https://mihshih.github.io/volcano-pulse-data/bundle.json>
- `names.json`：畫面上的火山名稱對照（英文名、中文名），網址 <https://mihshih.github.io/volcano-pulse-data/names.json>。可以直接在 GitHub 網頁編輯，見下方說明
- `decade.json`：IAVCEI 十年火山名單與地圖上的標籤，網址 <https://mihshih.github.io/volcano-pulse-data/decade.json>。同樣可以直接在 GitHub 網頁編輯
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
| `meta.decade_check` | 每年一次的十年火山名單比對：比對時間、維基百科名單、`status`（`same` 或 `diff`）、新增與移除的名稱 |
| `media` | YouTube 直播清單：`media.live` 以 `vnum` 為 key，每座最多 3 個正在直播的影片 |

- 所有時間都是 UTC。
- 各來源一律用 GVP Volcano Number（`vnum`）串接。
- 任一來源抓取失敗時沿用上一份資料，錯誤記在 `meta`。

## 名稱對照（names.json）

展示程式每次開啟都會直接讀這個檔，所以在 GitHub 上改完就會生效，不用等每天的資料更新。

**修改步驟**

1. 打開 <https://github.com/MiHShih/volcano-pulse-data/edit/main/names.json>（或在 repo 點 `names.json` 再按右上角的鉛筆）。
2. 一座火山一行，格式是 `"GVP 編號": {"gvp": "GVP 原名", "name": "畫面上的英文名", "zh": "中文名"}`。
   - `gvp`：只用來核對編號有沒有打錯，不會顯示。
   - `name`：取代畫面上的英文名，可省略；GVP 原名仍然可以搜尋。
   - `zh`：接在英文名後面的中文名，可省略。
   - 每行最後要有逗號，最後一行不要有逗號。
3. 按 **Commit changes**。
4. 約 1 到 10 分鐘後，各台展示端重新整理網頁就會套用。

**檔案寫壞了怎麼辦**：展示端讀不到格式正確的檔案時，會沿用上次成功讀到的那份，不會壞掉。展示程式的「來源」分頁會顯示使用的是哪一份，以及有沒有編號打錯、GVP 原名對不上的項目。GVP 編號可以在 [GVP 網站](https://volcano.si.edu/) 查詢。

## 十年火山名單（decade.json）

IAVCEI 在 1990 年代聯合國「國際減災十年」選定的 16 座火山（Avachinsky-Koryaksky 在 GVP 是兩座，所以有 17 個編號）。展示程式每次開啟都直接讀這個檔，格式跟 `names.json` 相同：

- `"GVP 編號": {"gvp": "GVP 原名", "label": "地圖上的常駐標籤"}`，`label` 的格式是「GVP 名稱 (通稱)」。
- 修改網址：<https://github.com/MiHShih/volcano-pulse-data/edit/main/decade.json>，改完按 **Commit changes**，各台重新整理網頁就會套用。

IAVCEI 沒有每年更新的官方名單，所以資料站每年比對一次維基百科條目 [Decade Volcanoes](https://en.wikipedia.org/wiki/Decade_Volcanoes) 的名單，有差異時寫進 `meta.decade_check`，展示程式的「來源」分頁會提醒；不會自動修改這個檔。

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
- 航空火山灰警報（`ash`、`ash30`）：NOAA Aviation Weather Center Data API，International SIGMETs，<https://aviationweather.gov/data/api/>（各國航空氣象單位發布的火山灰 SIGMET；美國政府資料）
- 日本噴火警戒等級（`levels`，agency 為 JMA）：出典：気象庁ホームページ（<https://www.jma.go.jp/bosai/map.html#contents=volcano>），本站翻譯整理（有加工：等級名稱譯成中文、只保留各火山最高等級）。依「公共データ利用規約（第1.0版）」使用
- 菲律賓警戒等級（`levels`，agency 為 PHIVOLCS）：PHIVOLCS Volcano Monitoring and Eruption Prediction Division，<https://wovodat.phivolcs.dost.gov.ph/bulletin/list-of-bulletin>（使用條件尚未確認）
- 紐西蘭警戒等級（`levels`，agency 為 GeoNet）：GeoNet Volcanic Alert Level API，<https://api.geonet.org.nz/volcano/val>，CC BY 3.0 NZ
- 每日動態與未經官方確認的活動（`obs`、`activity` 裡 source 為 claude 的紀錄）：由 Claude 網路搜尋整理，每則附原始來源連結（`url`）與來源名稱（`agency`），其中多數經 VolcanoDiscovery 火山消息 RSS（<https://www.volcanodiscovery.com/volcanonews.rss>）收錄。內容為摘要改寫，權利屬於各原始來源
- YouTube 直播清單（`media`）：影片編號與標題取自 YouTube Data API，影片權利屬於各頻道

各來源資料的權利屬於原提供單位。
