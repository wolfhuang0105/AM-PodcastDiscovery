# 鬼鴞說資料 · Podcast 知識庫

《鬼鴞說資料》Podcast 的單頁知識庫：21 集逐集主題切段、故事線、人物、詞彙，與**全站搜尋（含逐字稿）**。
程式集中在一個 `index.html`，逐字稿放在 `transcripts/` 資料夾（前端以 `fetch` 載入）。

## 檔案結構

```
index.html              ← 整個網站（HTML + CSS + JS + 內嵌資料 JSON）
transcripts/
  EP01.txt … EP21.txt   ← 各集逐字稿（被 index.html fetch 載入）
.nojekyll               ← 讓 GitHub Pages 原樣serve所有檔案
鬼鴞說資料_網站規格.md   ← 規格書（參考用，不需部署）
```

## 部署到 GitHub Pages

1. 建一個 repo，把 `index.html`、`transcripts/` 整個資料夾、`.nojekyll` 推上去（放在 repo 根目錄）。
   ```bash
   git init
   git add index.html transcripts .nojekyll
   git commit -m "鬼鴞說資料 podcast 知識庫"
   git branch -M main
   git remote add origin <你的 repo URL>
   git push -u origin main
   ```
2. 到 repo 的 **Settings → Pages**：Source 選 **Deploy from a branch**，Branch 選 `main` / `/ (root)`，Save。
3. 等一兩分鐘，網址會是 `https://<帳號>.github.io/<repo>/`。逐字稿與全站搜尋在此網址下可正常運作。

> 本機預覽不能直接雙擊 `index.html`（瀏覽器會擋本地 `fetch`，逐字稿與全文搜尋會載不到）。請用簡易伺服器：
> ```bash
> python3 -m http.server 8000
> # 開 http://localhost:8000
> ```

## 新增一集（加資料、不改程式）

1. 把新逐字稿放成 `transcripts/EP22.txt`。
2. 打開 `index.html`，找到 `<script id="data" type="application/json">…</script>`，在 `episodes` 陣列加一筆：
   ```jsonc
   {
     "id": 22,
     "title": "…",
     "type": "founding|community|product|external|people",
     "arc": null,
     "host": "Anna",
     "guests": [{ "name": "…", "role": "…" }],
     "keyPoints": ["…","…","…"],
     "segments": [{ "label": "…", "summary": "…", "tags": ["#組織"] }],
     "metaphors": ["…"],
     "links": [8, 20],
     "transcriptFile": "EP22.txt"
   }
   ```
3. （選用）在 `timeline` / `roles` / `glossary` 陣列補上新事件、新人物或新詞彙。
4. 存檔、push。版面與程式都不用改。

## 功能一覽

- **逐集**：每集三大重點、細部主題段落（可點標籤跨集篩選）、金句比喻、關聯集、逐字稿（點擊載入）。每集會顯示日期（取自 RSS）。
- **故事線**：四條敘事線（公司轉型／方法論／產品／外部生態）的垂直時間軸。
- **人物**：炬識「三維立體組織」框架（HACOM／Projecting／Communities）＋ 職稱層級對照（CEO → C-Level／副總 → 事業單位／團隊主管 → 成員），以及每個人的職涯軌跡（職稱演進不抹平）。
- **詞彙**：依類別整理，★ 為跨集核心概念。
- **全站搜尋**：按 `/` 或右上角；涵蓋標題／重點／段落／金句／詞彙／人物，並在背景把全部逐字稿併入索引、支援全文搜尋與命中高亮。
- **日／夜雙主題**：右上角切換，會記住偏好（`localStorage`）。

## 每集日期（RSS）

日期取自 Firstory RSS 的 `<pubDate>`，依標題的 `EP` 編號對應。網頁載入時會嘗試自動抓取：

1. 先讀同源的 `feed.xml`（若你把 RSS 存成這個檔放在根目錄，保證能用、無 CORS 問題）。
2. 再試線上 RSS：`https://feed.firstory.me/rss/user/clxcwv7fg001b01u5bn7z05op`（若 Firstory 允許跨網域存取就會直接成功）。

> 若兩者都抓不到（例如 CORS 被擋且未放 `feed.xml`），日期就不顯示，其餘功能不受影響。
> 也可以在逐字稿產製腳本裡，直接把日期寫進 `index.html` 資料 JSON 每集的 `"date": "YYYY.MM.DD"` 欄位（這樣最穩，不依賴執行期抓取）。
