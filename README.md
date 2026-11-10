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

格子順序＝JSON 裡的順序；增刪、調序都只改 JSON。改了格子結構、舊資料不相容時，把 `index.html` 裡的 `VERSION` 加一，使用者會看到「資料格式已更新」的提示。
