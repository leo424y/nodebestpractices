# 避免使用變數載入模組

### 一段解釋

避免使用被指定為參數的路徑變數匯入(requiring/importing)另一個檔案, 因為該變數可能源自使用者輸入。此規則可以擴充套件到一般情況下的訪問檔案(例如，`fs.readFile()`)，或者包含源自使用者輸入的動態變數的其他敏感資源。

### 程式碼示例

```javascript
// 不安全, 因為helperPath變數可能通過使用者輸入而改變
const uploadHelpers = require(helperPath);

// 安全
const uploadHelpers = require('./helpers/upload');
```
