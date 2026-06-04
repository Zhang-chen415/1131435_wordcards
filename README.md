# C# Windows Forms 單字卡與 TSV 資料管理系統

本專案是一個基於 C# Windows Forms 開發的多功能單字卡與學習管理系統。系統整合了 TSV（Tab 分隔文字）檔案讀寫技術、物件導向資料集合設計，以及 Windows Media Player 多媒體播放功能，提供使用者流暢的單字檢視、語音朗讀、自動播放 以及動態編輯存檔功能。

## 核心功能特點

### 1. TSV 資料檔讀取與解析 (`frmTSVFile`)
**Unicode 文字格式支援**：支援讀取以 Unicode 文字（`*.txt`）編碼儲存的檔案，確保各國音標與特殊符號能完美顯示。
**動態欄位呈現**：透過自訂的 `UpdateListView` 函式將資料集動態載入至 `ListView` 中，展示「單字」、「音標」、「音檔路徑」與「解釋」。
**Tab 字串自動分割**：核心資料處理機制使用 `Split('\t')` 動態解析單行資料，並使用 `string.Join` 與 `Skip(3)` 處理包含多行換行符號的詳細解釋欄位。

### 2. 直覺式智慧單字卡 (`frmWordCards`)
**極簡無邊框視覺**：單字呈現區採用無邊框與融入式背景色設計（Color: `255, 254, 242`），模擬真實實體單字卡的視覺感受。
**原生多媒體發音**：整合 `WindowsMediaPlayer` 組件，在切換單字時同步確認並播放位於相對路徑（如 `Sound\A\`）的 `.mp3` 真人發音。
**全自動定時播放**：內建 `timPlayer` 計時器（Interval: 2000），開啟自動播放後系統會自動遞增索引，並具備滾動置中演算法（`TopIndex` 調整），確保目前播放的單字始終保持在清單畫面中央。

### 3. 動態雙擊編輯與即時同步存檔 (`frmEditWord`)
**彈出式編輯視窗**：在單字清單上滑鼠雙擊，即可跳出專屬的編輯表單 `frmEditWord`，自動帶入該單字的現有欄位資訊進行修改。
**雙向資料格式轉換**：編輯完成後，系統會呼叫 `ToLineString()` 將修改後的物件還原為符合規範的 Tab 分隔格式字串。
**串流即時寫入**：採用 `StreamWriter` 技術，在按下儲存後即時將全新的單字清單完整覆寫回磁碟檔案（`strWordFile`），達成檔案持久化儲存。

---

## 開發環境與技術棧

* **開發工具**：Visual Studio
* [cite_start]**程式語言**：C#
* [cite_start]**應用程序類型**：Windows Forms App (.NET Framework 4.8)
* [cite_start]**外部組件參考**：`WMPLib` (Windows Media Player COM 元件)

---

## 專案核心架構與類別說明

```text
📂 YourProjectName
 │
 ├── 📄 Program.cs                  # 應用程式啟動進入點
 ├── 📄 frmTSVFile.cs               # TSV檔案讀取與檢視表單 (主介面一)
 ├── 📄 frmWordCards.cs             # 單字卡學習與自動播放表單 (主介面二)
 ├── 📄 frmEditWord.cs              # 單字資料即時編輯與儲存表單
 ├── 📄 frmAbout.cs                 # 系統組件資訊關於表單
 │
 ├── 📄 WordItem.cs                 # 單字實體類別 (封裝單字、音標、音檔與解釋)
 ├── 📄 WordCollection.cs           # 自訂強型別單字集合類別 (繼承自 Collection<WordItem>)
 │
 └── 📂 bin / Debug / (輸出目錄)
      ├── 📄 WordCards.txt          # 核心單字資料檔案 (Unicode / Tab 分隔)
      └── 📂 Sound
           └── 📂 A
                └── 📄 *.mp3        # 各單字對應的真人發音語音檔
<img width="1034" height="614" alt="image" src="https://github.com/user-attachments/assets/64980fc5-6c84-4e03-b0ea-7dd0a4077acf" />
