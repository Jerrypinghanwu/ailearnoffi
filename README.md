# AI learn 課程銷售落地頁

現代化、具備極佳 UI/UX、微互動動畫與高說服力的高轉換率課程銷售網站。

---

## 🌟 核心特色

1. **白話包裝拒絕焦慮**：將為期 6 個月（132 小時 / 22 週）的課程，以「學習AI的兩大階段」進行直覺包裝。
2. **完整 44 堂課綱與直覺預約滾動**：
   - 點擊各大綱按鈕平滑跳轉至預約表單，自動預填諮詢需求並聚焦姓名欄位。
   - 課表數據取自真實課綱 Excel 檔案。
3. **現代大廠科技美學 (Linear / Vercel 風格)**：
   - 深邃夜空黑底色 (`#0B0F19`) 搭配網格背景與微光光暈。
   - 漸層微光邊框卡片與浮動陰影。
   - `IntersectionObserver` 捲動平滑進場動畫 (Scroll Reveal)。
4. **高轉換預約表單與官方 LINE@ 整合**：
   - 包含姓名、Email、手機電話、手動填寫職業背景、大綱備註與防呆驗證。
   - 填寫完成後立即彈出官方 LINE@ 專屬 QR Code 彈窗，引導領取近期折扣。
   - 支援直接填入 Google Sheets Apps Script Webhook URL，非同步傳送資料。

---

## 🚀 本地端啟動方式

本專案採用輕量純前端架構（HTML5 + Tailwind CSS CDN + Vanilla JS），零建構步驟，直接開啟即可預覽：

### 方法一：使用 Python 內建 HTTP 伺服器
```bash
python -m http.server 4173
```
開啟瀏覽器前往：`http://localhost:4173`

### 方法二：使用 Node.js / npx serve
```bash
npx -y serve . -p 4173
```

---

## 📁 專案檔案結構

```
c:\Users\jerry\Desktop\sale demo\
├── index.html            # 9 大核心銷售區塊單頁式 HTML
├── css/
│   └── custom.css        # 自訂微光邊框、毛玻璃、動畫與捲動條樣式
├── js/
│   ├── curriculum.js     # 44 堂課完整結構化資料庫
│   └── app.js            # 核心互動（測驗推薦器、課表抽屜、表單驗證、倒數計時）
├── lessons.json          # 原始結構化 JSON 資料
├── 課程安排 (目前使用).xlsx # 原始課表資料庫
└── README.md             # 本說明文件
```

---

## 🔗 Google Sheets Webhook 設定步驟

若要將學員預約名單自動同步至 Google 試算表：
1. 建立一個 Google Sheet，並在「擴充功能」點選「Apps Script」。
2. 貼上下方接收腳本並部署為「網路應用程式 (Web App)」，存取權限設為「所有人 (Anyone)」：
```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  sheet.appendRow([
    data.timestamp,
    data.name,
    data.email,
    data.phone,
    data.role,
    data.notes,
    data.source
  ]);
  return ContentService.createTextOutput(JSON.stringify({ result: 'success' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```
3. 取得部署產生的 Webhook 網址，填入表單下方「自訂 Google Sheets Webhook URL」欄位即可完成串接！
