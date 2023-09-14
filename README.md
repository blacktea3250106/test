# Wisdome - Data Engineer Exercise

# 專案介紹

這個專案是專為從指定 PDF 文件中提取和解析[學科能力測驗－英文](pdf/112學年度學科能力測驗－英文.pdf)的題目內容而設計。

本專案不僅能夠解析`Wisdome`團隊要求的`112學年度學科能力測驗－英文`試題。

為了更具挑戰性，也具有解析`104至112學年度學科能力測驗－英文`試題的相容性。

專案結構主要包含三個主要的 Python 檔案，分別為 `main.py`, `process.py` 和 `output.py`.

## 🚀 `main.py`
**功能**: 主程式，負責引導整體的資料提取、處理與輸出流程。

- **初始化**: 載入所需的依賴庫和進行必要的設定。
- **資料讀取**: 使用 `pdfplumber` 從英語測驗的 PDF 檔案中取得文本內容。
- **處理**: 呼叫 `process.py`，進行測驗文本的清洗和解析。
- **輸出**: 利用 `output.py`，將處理後的測驗資料以結構化方式呈現或保存。

## 🛠 `process.py`
**功能**: 負責資料清洗與解析，專注於對各類測驗題目的處理。

- **資料清洗**: 清除不需要的文字模式，如多餘的空白、換行和冗字。
- **內容提取**: 使用正則表達式從文本中抽取各種測驗部分，如詞彙題、閱讀測驗等。
- **資料結構**: 將解析的測驗內容組裝成 Python 資料結構，例如字典或列表。

## 📤 `output.py`
**功能**: 為格式化及輸出處理後的測驗資料所設計。

- **格式轉換**: 提供資料轉為 JSON 格式的功能，並將其保存。
- **資料儲存**: 將解析及組裝好的資料輸出到特定位置，例如一個指定的文件夾。
- **輸出報告**: 在 Terminal 上以友善的格式呈現各類測驗題目的詳細內容。

---

# 資料架構

資料的結構會以 JSON 格式進行儲存。

- **`source`**: 文件的路徑
- **`date`**: 取得資料的日期
- **`passages`**: 測驗文件中的系列或段落
  - **`id`**: 段落的唯一編號
  - **`content`**: 段落的文字內容
  - **`questions`**: 段落相關的系列問題
    - **`id`**: 問題的唯一編號
    - **`text`**: 問題的文字說明
    - **`options`**: 選擇題的選項文字陣列

---

# 使用說明

為了正確使用本專案，請遵循以下步驟：

1. **準備試題檔案**
    - 將希望解析的`學科能力測驗－英文試題` PDF 文件放入 `pdf` 資料夾中。

2. **指定檔案名稱**
    - 在 `main.py` 中找到 `file_name` 變數。
    - 更改其值為您希望解析的文件名稱。
   
      ```python
      file_name = "{file_name}.pdf"
      ```

3. **運行程式**
    - 在終端機或命令提示字元中，執行以下命令以運行 `main.py`:
      
      ```bash
      python main.py
      ```

4. **獲取結果**
    - 運行完成後，您可以在 `json` 資料夾中找到名為 `{file_name}.json` 的結構化資料。

---

# 測試時遇到的挑戰與解決方法

#### 1. Content 的提取問題 (提取錯誤)
- **挑戰**：正規表達式使用 `\d+.` 誤判日期為題號，造成提取不完整。

- **解決方法**：`第(\d+)至(\d+)題為題組\n(.*?)(?=\n\1\.\s.*?\(A\))`
  1. `第(\d+)至(\d+)題為題組\n`：匹配 `"第x至y題為題組"` 的格式。
  2. `(.*?)`：捕獲後續文字，直到下一指定條件。
  3. `(?=\n\1\.\s.*?\(A\))`：捕獲的第一個題號`start`，匹配到 `"start. (A)"` 則停止提取。

---

#### 2. Questions 的提取問題 (錯誤判斷)
- **挑戰**：正規表達式誤讀`content`文字，使資料格式錯誤。
- **解決方法**：在解析`questions`前，移除`content`文字。

---

#### 3. Questions 的提取問題 (選項遺漏)
- **挑戰**：使用正規表達式提取試題時可能會遺漏某些選項。

- **解決方法**：
  1. 初步提取：先使用 `(\d+)[.](.*?)(?=\n\d+\.|\n-|$)` 提取題目的`id`和其內容。
  2. 細部解析：將提取的內容透過迴圈再進行細部分析，拆解出`text`和`options`。

---

#### 4. Content 的換行問題
- **挑戰**：`content`文字中有原始文本的`\n`和 PDF 格式所限制的非自然的`\n`。

- **解決方法**：為方便檢閱，所有換行`\n`都替換為空格`' '`，但這並非最佳選擇。

---

#### 5. 圖片文字解析問題
- **挑戰**：`content`文字會混入圖片的文字。

- **解決方法**：暫不考慮排除圖片文字。

---

# 💌 致謝
非常開心能收到來自`Tristan`的二次面試邀約的郵件，
也非常高興能受到`Wisdome`團隊的肯定，
希望能夠藉由這次試題，向團隊盡力展現了關於我的技術和熱忱，
我衷心期待能加入`Wisdome`，與你們一同攜手創建更多的可能性，
再次深深地感謝`Tristan`和`Wisdome`團隊給予這難得的機會，期待未來更多的交流和合作。
