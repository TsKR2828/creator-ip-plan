# 創作者 IP 規劃工具

一格一格帶創作者想清楚市場、定位與變現，最後匯出自己的 IP 規劃報告。規格見 `創作者 IP 規劃工具 Spec.md`。

## 檔案

| 檔案 | 內容 |
| --- | --- |
| `index.html` | 介面＋邏輯（原生 JS，無框架、無外部請求） |
| `content.json` | 所有格子的說明文字與阿路範例。**改文字只改這裡** |
| `print.css` | 列印／存成 PDF 的樣式，只印報告 |
| `阿路角色設定.md` | 範例角色的固定設定，寫範例時對照，避免前後矛盾 |

## 本機開啟

直接點兩下 `index.html` 會讀不到 `content.json`（瀏覽器擋本機檔案讀取），要開一個本機伺服器：

```bash
python -m http.server 8742 --bind 127.0.0.1 --directory "C:/Users/admin/Desktop/Claude/market"
```

然後打開 <http://127.0.0.1:8742>。Claude 桌面版的預覽名稱是 `ipplan`。

## 部署

GitHub Pages：檔案放 repo 根目錄，Pages 選 main branch 根目錄。推上 main 後約一分鐘自動更新。

線上版：<https://tskr2828.github.io/creator-ip-plan/>

## content.json 欄位

- `sections[].tier`：`core`（核心，填完才進總覽）或 `advanced`（進階，使用者勾選才出現）
- `sections[].desc`：選填。區塊第一格上方會顯示；進階區塊勾選頁也會用
- `cells[]` 四個必填欄位：`asking`（在問什麼）、`why`（為什麼要先想這個）、`stuck`（常見卡點）、`example`（範例・阿路）
- `cells[].calc`：選填，「怎麼算・去哪看」。需要數字的格子用
- `cells[].refs`：選填，填其他格的 id，會在這格顯示使用者在那些格填過的內容（行動指標用）
- `cells[].role: "reportTitle"`：這格的答案當報告標題，不進報告內文
- `appendix`：附錄工具清單，每行 `- ` 開頭
- `sections[].quest`：RPG 外皮（v2）。`no` 章號（同一個 `no` 的區塊會併成一章，0 是序章）、`name` 章名、`sheet` 角色卡上的欄位名、`reward` 獎勵文字
- `cells[].stat`：選填，能力值名稱（羈絆、誓約…），角色卡上會跟原本的格名一起顯示
- `cells[].role: "epithet"`：這格的第一行當角色卡上的稱號

格子順序＝JSON 裡的順序；增刪、調序都只改 JSON。

## 頁面與資料（v2）

規格見 `創作者 IP 規劃工具 RPG 版 Spec.md`（只在本機）。目前做到第 1 期：章名、任務頁、角色卡、等級。

| 路徑 | 頁面 |
| --- | --- |
| `#/` | 沒資料＝開場；有資料＝角色卡 |
| `#/quests` | 任務頁（取代 v1 的總覽與進階勾選；`#/overview`、`#/advanced` 會導到這裡） |
| `#/fill/<格子 id>` | 填寫頁 |
| `#/done/<章號>` | 一章解完的畫面 |
| `#/export` | 執照／匯出報告 |

- 等級＝解完的章數（序章不算）。一章「解完」＝每一格都有寫或按過跳過。等級、階位、任務狀態都是從答案算出來的，不另外存。
- 資料存在 `localStorage` 的 `ipplan.v2`。讀到 v1 的 `ipplan.v1` 會自動轉成 v2（格子 id 沒變，答案原樣帶入），v1 那筆留著當備份，使用者按「全部清除」時兩筆一起刪。
- 之後改了格子結構、舊資料不相容時：把 `index.html` 的 `VERSION` 加一，並在 `loadState()` 補上轉換；轉不了的才會出現「資料格式已更新」的畫面。
