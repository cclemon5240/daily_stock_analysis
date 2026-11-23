# Telegram 繁體中文通知（此個人部署）

Telegram 文字通知在發送前使用 OpenCC s2tw 轉成臺灣繁體字，包含標題、固定欄位與 AI 分析文字。轉換在 Markdown 處理與訊息分段之前執行，因此單則、多則與純文字回退皆使用繁體內容。

安裝 requirements.txt 會同時安裝 opencc-python-reimplemented==0.1.7。轉換在本機完成，不額外呼叫 Gemini。此調整只影響 Telegram 文字通知，原始報告檔案及其他通知管道維持原有輸出。

恢復原行為時，還原 src/notification_sender/telegram_sender.py 的 OpenCC 匯入、初始化及轉換呼叫，並移除 requirements.txt 中的 OpenCC 依賴。

This personal deployment converts Telegram text notifications to Taiwan Traditional Chinese before Markdown formatting and chunking. It adds no LLM calls; stored reports and other notification channels retain their existing behavior.
