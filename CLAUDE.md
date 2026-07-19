# Lakers Daily News Digest — 更新規範

這個專案每天產生一期湖人隊（Los Angeles Lakers）英文新聞摘要，供閱讀器 `index.html` 顯示。
閱讀器支援「純英文」與「英文＋中文翻譯＋文法解析」兩種模式切換。

## 每日更新流程（給自動排程 agent 或手動執行）

1. 用 WebSearch 搜尋最新湖人消息，至少涵蓋：
   - `Lakers news latest <當天日期>`
   - `Lakers rumors trade free agency <當月>`
   - 賽季中加搜 `Lakers game result recap`；休賽季加搜 summer league / offseason
   - 論壇聲音：Silver Screen and Roll、Lakers Nation、HoopsHype、r/lakers（搜得到就用）
2. **新聞時效（重要）：只收錄「過去約 24 小時」的新消息**：
   - 以台灣當天日期為刊號。美國比台灣慢，所以「昨天美國晚上」的比賽與新聞屬於今天這期。
   - 搜尋時務必確認報導日期（可在查詢中加上具體日期，如 `Lakers news July 18 2026`）。
   - **不可把幾天前、上週的舊聞當成當天新聞重炒**。先看 `data/manifest.js` 裡前幾期的日期，
     打開最近一期資料檔確認已報導過的主題，避免重複。
   - 舊事件有「當天的新進展」才能寫，而且要聚焦新進展本身（例如：離隊是舊聞，
     但當天的公開發言、新報價是新聞）。
   - 若當天實在沒有大新聞，改寫當天論壇熱議或深度分析，不要拿舊聞充數。
3. **每天必須包含一篇 tag 為 `Fan Pulse` 的論壇球迷聲音文章**：
   - 注意：Reddit（reddit.com）在此環境被封鎖，無法直接抓取。
   - 改用 WebSearch 間接取得，例如：`Lakers fans react <話題>`、`r/lakers reaction <話題>`、
     `Silver Screen and Roll reacts survey`、HoopsHype 的 social media roundup 等轉述報導。
   - 內容整理球迷圈的主流意見、爭論點、有趣留言（可意譯），出處附轉述來源的連結。
4. 挑選 4–6 則最重要的新聞（含上述 Fan Pulse），每則寫成一篇短文（2–3 段，每段 2–4 句）。
   - **文章內文必須是你自己改寫的英文摘要**，不可整段抄襲原文。
   - 每篇附上出處連結（sources）。
5. 為每一段寫：
   - `zh`：自然通順的繁體中文翻譯
   - `vocab`：2–4 個關鍵單字／片語（term、pos 詞性、zh 中譯、note 補充）
   - `grammar`：1–2 個文法解析點（sentence 摘錄原句、analysis 用繁體中文解釋句構）
6. 產出 `data/YYYY-MM-DD.js`（格式見下方 schema），日期用當天日期。
7. 在 `data/manifest.js` 的陣列**最前面**加入新日期。
8. 不要修改 `index.html` 除非格式有變動。
9. **發布到 GitHub Pages**：產完資料後執行
   `git add -A && git commit -m "digest: YYYY-MM-DD" && git push`
   （remote 已設定為 https://github.com/vzo987/lakers-daily-digest，
   線上閱讀網址：https://vzo987.github.io/lakers-daily-digest/，push 後約 1 分鐘生效。）

## 資料檔 schema（data/YYYY-MM-DD.js）

```js
window.LAKERS_NEWS = window.LAKERS_NEWS || {};
window.LAKERS_NEWS["YYYY-MM-DD"] = {
  date: "YYYY-MM-DD",
  headline: "當期一句話總標題（英文）",
  headline_zh: "總標題中文",
  articles: [
    {
      title: "文章英文標題",
      title_zh: "文章中文標題",
      tag: "分類，如 Breaking / Trade / Summer League / Rumors / Game Recap",
      sources: [ { name: "ESPN", url: "https://..." } ],
      paragraphs: [
        {
          en: "英文段落。",
          zh: "繁體中文翻譯。",
          vocab: [
            { term: "run", pos: "n.", zh: "（一段）時期、共事歲月", note: "口語常見用法" }
          ],
          grammar: [
            { sentence: "原句摘錄", analysis: "繁體中文句構解析" }
          ]
        }
      ]
    }
  ]
};
```

## manifest（data/manifest.js）

```js
window.LAKERS_MANIFEST = ["2026-07-19"]; // 新的日期加在最前面
```

## 注意事項

- 一律使用繁體中文（台灣用語）。
- 資料檔用 script 標籤載入（不是 fetch），所以 `index.html` 直接雙擊開啟（file://）也能用。
- 若當天沒有大新聞，寫論壇討論熱點或深度分析也可以，但至少要有 3 篇。
