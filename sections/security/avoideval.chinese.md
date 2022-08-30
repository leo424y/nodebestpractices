# 避免JS eval語法

### 一段解釋

`eval()`，`setTimeout()`和`setInterval()`是全局方法，經常在Node.js中被使用，它接收一個字元串參數，代表一個JavaScript表示式，語句，或者語句序列。使用這些函數的安全問題是, 不受信任的使用者輸入可能會發現進入程式碼執行的方式，從而導致危害伺服器，因為評估使用者程式碼實質上允許攻擊者執行任何他可以的操作。對於這些使用者輸入可以傳遞給函數並執行的方法，建議重構程式碼，而不依賴於它們的使用。

### 程式碼示例

```javascript
// 攻擊者可能輸入的惡意程式碼示例
const userInput = "require('child_process').spawn('rm', ['-rf', '/'])";

// 惡意程式碼被執行
eval(userInput);
```

### 其他部落格作者的說法

摘自[Liran Tal](https://leanpub.com/nodejssecurity)的書籍Essential Node.js Security:
> 從安全的角度出發，在JavaScript語法中，eval()可能是最讓人不悅的函數。
它將javascript字元串解析為文字，並將其作為javascript程式碼執行。
和不受信任的使用者輸入摻和在一起，可能會發現使用eval()是一個導致災難的方式，最終伺服器遭受破壞。
