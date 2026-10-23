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
