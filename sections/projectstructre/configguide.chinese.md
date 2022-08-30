# 使用環境感知，安全，分層的配置

<br/><br/>


### 一段解釋

當我們處理配置參數時，常常會很慢並且很煩躁：（1）當需要注入100個keys(而不是隻在配置檔案中提交它們)時，使用程序環境變數設定所有的keys變得非常繁瑣，但是當處理只有devops管理許可權的檔案時，不改變程式碼行為就不不會變。一個可靠的配置解決方案必須結合配置檔案和程序變數覆蓋。（2）列舉一個普通JSON的所有keys時，當目錄變得非常龐雜的時候，查詢修改條目困難。幾乎沒有配置庫允許將配置儲存在多個檔案中，執行時將所有檔案聯合起來。分成幾個部分的分層JSON檔案能夠克服這個問題。請參照下面示例。（3）不推薦儲存像密碼資料這樣的敏感資訊，但是又沒有快速便捷的方法解決這個難題。一些配置庫允許檔案加密，其他庫在Git提交時加密目錄，或者不儲存這些目錄的真實值，在通過環境變數部署期間列舉真實值。（4）一些高階配置場景需要通過命令列（vargs）注入配置值，或者像Redis一樣通過集中快取同步配置資訊，所以不同的伺服器不會儲存不同的資料。

一些配置庫可以免費提供這些功能的大部分功能，請檢視NPM庫（[nconf](https://www.npmjs.com/package/nconf), [config](https://www.npmjs.com/package/config) 和 [convict](https://www.npmjs.com/package/convict)）這些庫可以滿足這些要求中的許多要求。

<br/><br/>

### 程式碼示例 – 分層配置有助於查詢條目和維護龐大的配置檔案

```javascript
{
  // Customer module configs 
  "Customer": {
    "dbConfig": {
      "host": "localhost",
      "port": 5984,
      "dbName": "customers"
    },
    "credit": {
      "initialLimit": 100,
      // Set low for development 
      "initialDays": 1
    }
  }
}
```

<br/><br/>
