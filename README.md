# 🔍 失物招領系統 (Lost and Found System)

本專案為失物招領系統，採用前後端分離架構，並已全面部署至雲端平台。

---

## 🌐 雲端服務與線上網址

- **前端網站 (Vercel)**：`https://你的專案名稱.vercel.app` *(請替換為你的 Vercel 網址)*
- **後端 API (Render)**：`https://lost-and-found-workspace.onrender.com/api`
- **雲端資料庫 (Render PostgreSQL)**：`lost_and_found_ql2o`

---

## 🛠️ 專案目錄結構

```text
lost-and-found-workspace/
├── frontend/         # Vue 3 + Vite 前端專案
└── backend/          # Laravel 後端 API 專案
🚀 組員快速上手（本地開發環境設定）
如果你要在本機（Local）進行開發與測試，請跟著以下步驟設定：

1. 複製專案 (Clone)
Bash
git clone [https://github.com/yuhsun0313-oss/lost-and-found-workspace.git](https://github.com/yuhsun0313-oss/lost-and-found-workspace.git)
cd lost-and-found-workspace
2. 前端環境設定 (Frontend)
Bash
cd frontend
npm install
npm run dev
💡 注意事項：
目前前端 App.vue 中的 API 網址已預設指向 Render 雲端後端。
本機啟動後，直接新增、修改資料，就會同步更新到雲端資料庫！

3. 查看與管理雲端資料庫 (DBeaver)
若組員需要直接查看或修改雲端資料庫內容，請使用 DBeaver 並建立以下連線：

連線類型：PostgreSQL

Connect by：Host

Host：dpg-dbd3f6on74is7382fong-a.singapore-postgres.render.com

Port：5432

Database：lost_and_found_ql2o

Username：lost_and_found_ql2o_user

Password：wZLyKDGoJpKvXZFiJbjJrUTKcrUc6v1a

🔄 如何修改程式碼並更新到雲端？
組員在本地端修改程式碼後，只要推送到 GitHub，雲端平台就會自動觸發更新（Auto-Deploy）！

修改與推送流程：
拉取最新程式碼（開發前先更新）：

Bash
git pull origin main
進行修改與測試（在本地測試功能無誤）。

提交並推送到 GitHub：

Bash
git add .
git commit -m "描述你這次改了什麼 (例如: 新增搜尋過濾功能)"
git push origin main
自動部署生效：

前端修改（frontend/）：Vercel 偵測到 main 分支更新後，約 1 分鐘內會自動完成部署並更新線上網站。

後端修改（backend/）：Render 會自動抓取最新程式碼重新建置並上線。

🤝 協作規範與注意事項
請勿隨意刪除雲端資料庫中的重要測試資料。

每次開發前請務必執行 git pull origin main，避免程式碼衝突。
