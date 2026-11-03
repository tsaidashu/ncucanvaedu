# ncucanvaedu
國立中央大學 Canva 教育版升級申請系統 (NCU Canva for Education Application System). Built with Serverless architecture and automated data anonymization.
國立中央大學 Canva 教育版 - 教師資格申請系統 🎓

專為國立中央大學教職員設計的 Canva 教育版升級申請表單系統。配合學校 Canva 團隊的建立，提供教職員一個安全、快速的登記升級管道，並內建符合資安規範的自動隱碼機制。

✨ 系統特色

⚬ 前後端物理隔離架構：將「公開填寫表單」與「管理員後台」完全拆分為兩個獨立檔案。GitHub Pages 僅代管不含機敏邏輯的前端表單，管理員審核介面則由本地端執行，徹底杜絕密碼外洩風險。
⚬ Serverless 雲端資料庫：串接 Firebase Firestore，無須建置與維護實體伺服器即可達成資料即時寫入與同步。
⚬ 資安防護與資料生命週期管理：內建自動清理機制，申請名單經管理員標示「已開通」滿 7 天後，系統會自動將資料隱碼（例如：蔡振瑋 -> 蔡O瑋，並截斷 Email 網域），確保個人資料不無限期留存。
⚬ 響應式現代化介面：使用 Tailwind CSS 打造，支援跨裝置流暢填寫與管理。

📂 檔案結構與部署方式

本專案採用純前端架構 (HTML/JS)，分為兩個核心檔案：

1. index.html (申請表單 - 對外公開)

⚬ 用途：提供教職員填寫申請資料。
⚬ 部署：直接部署於 GitHub Pages，產生對外公開網址（可附加於 Canva 製作的宣傳海報按鈕上）。
⚬ 權限：僅具備資料庫的「寫入 (Create)」權限。

2. admin.html (管理員後台 - 內部機密)

⚬ 用途：供團隊管理員審核名單、切換開通狀態，並執行背景隱碼機制。
⚬ 部署：不上傳至 GitHub。請直接存放於管理員的個人電腦或安全的內部地端伺服器，雙擊即可開啟。
⚬ 權限：具備資料庫的讀取、更新 (隱碼) 權限，並設有本地端密碼驗證。

🛠 技術堆疊

⚬ 前端框架：Vanilla JavaScript, HTML5
⚬ CSS 框架：Tailwind CSS (經 CDN 載入)
⚬ 後端與資料庫：Firebase Firestore (Web SDK 10.8.1)
⚬ 代管服務：GitHub Pages

🔒 Firebase 安全規則 (Security Rules)

為確保公開表單的安全性，Firebase Firestore 必須套用以下安全規則：

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /canva_applications/{document=**} {
      // 允許任何人填寫表單新增資料
      allow create: if true;
      // 開放讀取與更新權限給本地端的 admin.html 運作
      allow read, update, delete: if true; 
    }
  }
}
