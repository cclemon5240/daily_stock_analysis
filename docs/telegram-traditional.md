# Telegram 繁體中文通知（此個人部署）

Telegram 文字通知在發送前使用 OpenCC s2tw 轉成臺灣繁體字，包含標題、固定欄位與 AI 分析文字。轉換在 Markdown 處理與訊息分段之前執行，因此單則、多則與純文字回退皆使用繁體內容。

安裝 requirements.txt 會同時安裝 opencc-python-reimplemented==0.1.7。轉換在本機完成，不額外呼叫 Gemini。此調整只影響 Telegram 文字通知，原始報告檔案及其他通知管道維持原有輸出。

恢復原行為時，還原 src/notification_sender/telegram_sender.py 的 OpenCC 匯入、初始化及轉換呼叫，並移除 requirements.txt 中的 OpenCC 依賴。

This personal deployment converts Telegram text notifications to Taiwan Traditional Chinese before Markdown formatting and chunking. It adds no LLM calls; stored reports and other notification channels retain their existing behavior.

## 台股與美股分開排程

所有時間均為臺灣時間（UTC+8），為開始分析的時間；Telegram 在分析完成後收到報告，GitHub 排隊可能延後。

- 台股：週一至週五 17:00（UTC 09:00），從 STOCK_LIST 挑出 .TW 與 .TWO 股票。
- 美股：週二至週六 07:00（UTC 前一天 23:00、週一至週五），分析 STOCK_LIST 的其他股票；此個人部署清單的其他股票均為美股。
- 排程僅執行個股分析，保留既有交易日檢查；美股時段對應美國上一交易日收盤。
- 手動 Run workflow 可用 market=tw/us/all 選擇清單，mode 預設 stocks-only。
- 更新股票仍使用 GitHub Actions 的 STOCK_LIST Secret/Variable，不需要另維護兩份清單。
- 還原：將工作流程 schedule 恢復原 cron，並移除 SCHEDULE_EXPRESSION、REQUESTED_MARKET 與 TARGET_MARKET 分流區塊。

Scheduled runs split the watchlist by market: Taiwan weekdays at 17:00 UTC+8 and US reports Tuesday–Saturday at 07:00 UTC+8. These are start times; delivery follows completion. Manual runs support tw/us/all.
