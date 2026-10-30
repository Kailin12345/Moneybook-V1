# 📱 我的記帳 (My Finance App)

專為學生與個人打造的全方位財務管理與智慧記帳 Android 應用程式。採用現代 Android 官方標準架構（Kotlin + Jetpack Compose Material 3 + Room Database + MVVM）開發，具備專款分帳戶、歷史資料 GZIP 壓縮封存、股票/ETF 投資部位追蹤與 Google 雲端備份等完整功能。

---

## ✨ 核心特色與功能模組

### 1. 🧾 智慧收支記帳與即時月度預算
- **即時收支管理**：支援分類、備註、支付工具（現金、銀行、電子支付、信用卡等）。
- **離線高速語義辨識**：在新增記帳時輸入自然語言（例：「午餐 120 現金」），系統自動離線解析類別、金額與方式。
- **動態月預算比對**：視覺化進度條、超支即時預警與每日預算額度試算。

### 2. 🗂️ 專款專用分帳戶（母子帳戶 / 六罐子與三水桶理財法）
- **母子帳戶體系**：在實體銀行帳戶下建立「生活費」、「緊急備用金」、「旅遊基金」等專款虛擬子帳戶。
- **理財範本一鍵建立**：支援「哈福·艾克六罐子法 (55/10/10/10/10/5)」、「三水桶法 (50/30/20)」等經典理財比例。
- **帳戶資金調撥**：支援帳戶間資金轉帳與專款調度。

### 3. 📦 歷史月份資料壓縮與封存技術
- **當月活躍即時保留**：當前月份明細完整保留，秒級載入對帳。
- **歷史月份 GZIP 串流深度壓縮**：非當月份明細一鍵打包為 JSON 並進行 GZIP 壓縮，節省約 **80%~90%** 儲存空間。
- **免解壓月報查閱**：封存時預先儲存收支與分類統計，隨時可展開檢視。
- **SQLite VACUUM 實體釋放**：自動執行 `PRAGMA wal_checkpoint(FULL)` 與 `VACUUM`，真正將磁碟分頁歸還給手機。
- **無損一鍵還原**：隨時可將任一歷史月份解壓還原回活躍資料表。

### 4. 📈 股票與 ETF 投資部位管理
- **個股與 ETF 追蹤**：支援台股（TW）與美股（US）標的。
- **零股與持股成本計算**：支援小數點股數、平均投入成本、現價市值與未實現損益率。
- **被動現金殖利率預估**：自動推算年化被動股利收入。

### 5. 🔒 資料隱私與備份轉移
- **資產金額隱私遮罩**：支援首頁一鍵遮罩資產總額（••••••）。
- **CSV 匯出與轉入**：支援符合 Excel 與 Google 試算表格式（含 UTF-8 BOM）之 CSV 備份。
- **Google 雲端備份**：一鍵將帳戶、預算、目標、投資與記帳完整資料打包成 JSON 備份檔。

---

## 🏛️ 技術架構與技術棧 (Tech Stack)

| 領域 | 技術與函式庫 | 說明 |
| :--- | :--- | :--- |
| **語言** | Kotlin 2.0+ | 現代化、型別安全、協程支援 |
| **UI 框架** | Jetpack Compose (Material 3) | 聲明式 UI、動態配色、自適應佈局 |
| **架構模式** | MVVM + Repository Pattern | 單向資料流 (UDF)、關注點分離 |
| **非同步處理** | Kotlin Coroutines & StateFlow | 線程分離 (`Dispatchers.IO` / `Main`) |
| **本機資料庫** | Room ORM (SQLite) | 結構化資料持久化、編譯期 SQL 檢驗、平滑 Migration |
| **資料壓縮** | `GZIPOutputStream` / `GZIPInputStream` | 零第三方依賴、二進位串流壓縮 |
| **身分驗證** | Google Sign-In & Firebase Auth | 雲端連結支援 |

---

## 📂 專案目錄結構 (Project Structure)

```text
app/src/main/java/com/example/
├── ai/                      # 智慧語意離線解析服務 (GeminiAiService)
├── auth/                    # Google 帳號管理與身分驗證 (AuthManager)
├── cloud/                   # Google 雲端備份與還原管理 (CloudBackupManager)
├── data/
│   ├── model/               # Room Entity (Transaction, Asset, Goal, Budget, ArchivedMonth)
│   ├── dao/                 # Data Access Objects (FinanceDao, TransactionDao)
│   ├── repository/          # Repository 模式資料中介層 (FinanceRepository)
│   └── AppDatabase.kt       # Room 資料庫單例與升級遷移邏輯 (Migrations)
├── ui/
│   ├── components/          # 獨立 UI 元件、對話框與各功能 Tab
│   │   ├── AssetsTab.kt                      # 資產與專款分帳戶、投資部位
│   │   ├── BudgetTab.kt                      # 每月預算管理
│   │   ├── GoalsTab.kt                       # 儲蓄心願目標
│   │   ├── HistoricalDataCompressorDialog.kt # 歷史封存管理與開發者小講堂
│   │   ├── SettingsDialog.kt                 # 系統設定、資料庫優化與備份
│   │   └── ...
│   ├── theme/               # Material 3 配色、文字規範與樣式 (Theme.kt, Color.kt)
│   ├── util/                # 金額 Cents 格式化與工具類 (CurrencyUtils)
│   ├── viewmodel/           # 業務邏輯與狀態流 (FinanceViewModel)
│   └── MainScreen.kt        # 主畫面導覽與頂部狀態列
└── util/                    # CSV 匯出匯入與 App 安全鎖管理
```

---

## 🚀 如何在本地建置與執行 (Getting Started)

### 需求環境
- **Android Studio**：Ladybug (2024.2.1) 或更高版本
- **JDK**：OpenJDK 17 或 21
- **Android SDK**：Min SDK 24，Compile SDK 36

### 本地執行步驟
1. 複製此儲存庫：
   ```bash
   git clone <your-repository-url>.git
   ```
2. 使用 Android Studio 開啟專案資料夾。
3. 等待 Gradle 依賴自動同步（Sync）。
4. 連接實體 Android 手機或啟動 Android 模擬器。
5. 點擊工具列上的 **Run 'app' (綠色三角形)** 即可安裝運行！

---

## 📄 授權條款 (License)
本專案採 [MIT License](LICENSE) 開源授權，歡迎學習交流與改進！
