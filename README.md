# 🔍 失物招領系統 (Lost and Found System)

本專案為失物招領系統，採用前後端分離架構，已全面部署至雲端平台，同時支援本地開發測試。

---

## 🌐 雲端服務與線上網址

- **前端網站 (Vercel)**：`https://你的專案名稱.vercel.app` *(請替換為你的 Vercel 網址)*
- **後端 API (Render)**：`https://lost-and-found-workspace.onrender.com/api`
- **雲端資料庫 (Render PostgreSQL)**：`lost_and_found_ql2o`

---

## 💻 組員開發環境準備 (Environment Setup)

在開發前，請先確保你的電腦已安裝以下軟體與工具：

### 1. 安裝必備環境與工具
- **Node.js**：[官方下載](https://nodejs.org/) *(安裝 LTS 版本，前端執行必備)*
- **PHP**：[官方下載](https://windows.php.net/download/) 或透過 **XAMPP / Herd** 安裝 *(後端開發/測試必備)*
- **Composer**：[官方下載](https://getcomposer.org/) *(Laravel 套件管理)*
- **Git**：[官方下載](https://git-scm.com/)

### 2. VS Code 推薦擴充套件 (Extensions)
建議在 VS Code 安裝以下套件，開發會更順手：
- `Volar` 或 `Vue - Official` (Vue 3 語法高亮)
- `Prettier - Code formatter` (程式碼自動排版)
- `PostgreSQL` (可直接在 VS Code 內檢視雲端資料庫)

---

## 🚀 前端與後端啟動教學 (VS Code)

### 1. 複製專案 (Clone)
開啟 VS Code 終端機 (Terminal)，執行：
```bash
git clone [https://github.com/yuhsun0313-oss/lost-and-found-workspace.git](https://github.com/yuhsun0313-oss/lost-and-found-workspace.git)
cd lost-and-found-workspace

🎨 啟動前端 (Frontend) — 主要開發
進入前端資料夾並安裝套件：

Bash
cd frontend
npm install
啟動前端開發伺服器：

Bash
npm run dev
預覽網頁：
終端機會顯示例如 http://localhost:5173/ 的連結，在瀏覽器開啟即可！

💡 說明：
目前前端 App.vue 中的 API 已預設連線至 Render 雲端後端。
本機前端發送的請求與新增的資料，會直接更新到 Render 雲端資料庫！

⚙️ 啟動後端 (Backend) — 如需修改後端才執行
如果需要修改或測試後端程式碼：

進入後端資料夾並安裝套件：

Bash
cd backend
composer install
複製環境變數設定檔：

Bash
cp .env.example .env
php artisan key:generate
啟動 Laravel 本機伺服器：

Bash
php artisan serve
後端網址預設為 http://127.0.0.1:8000。

🗄️ 查看與管理雲端資料庫 (DBeaver)
若需要直觀地檢視或修改 Render 雲端資料庫資料，請使用 DBeaver 設定以下連線：

連線類型：PostgreSQL

Connect by：Host

Host：dpg-dbd3f6on74is7382fong-a.singapore-postgres.render.com

Port：5432

Database：lost_and_found_ql2o

Username：lost_and_found_ql2o_user

Password：wZLyKDGoJpKvXZFiJbjJrUTKcrUc6v1a

🔄 如何修改程式碼並更新到雲端？
在本地端修改程式碼後，只要推送到 GitHub main 分支，雲端平台就會自動觸發部署更新！

開發前先拉取最新程式碼：

Bash
git pull origin main
進行修改並完成測試。

提交並推送到 GitHub：

Bash
git add .
git commit -m "描述修改內容 (例：新增失物分類搜尋功能)"
git push origin main
自動部署：

前端修改（frontend/）會由 Vercel 自動更新上線。

後端修改（backend/）會由 Render 自動重新建置。

🤝 協作規範
每次開始寫程式前，請務必先執行 git pull origin main，避免衝突。

請勿隨意刪除雲端資料庫中的重要測試資料。
