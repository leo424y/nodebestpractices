# 快速報錯，使用專用庫驗證參數


### 一段解釋

我們都知道如何檢查參數和快速報錯對於避免隱藏的錯誤很重要（見下面的反模式程式碼示例）。如果沒有，請閱讀顯式程式設計和防禦性程式設計。在現實中，由於對其編碼是件惱人的事情（比如考慮驗證分層的JSON物件，它包含像email和日期這樣的欄位），我們傾向於避免做這樣的事情 – 像Joi這樣的庫和驗證器輕而易舉的處理這個乏味的任務。

### 維基百科: 防禦性程式設計

防禦性程式設計是一種改進軟體和原始碼的方法, 在以下方面: 一般質量 – 減少軟體 bug 和問題的數量。使原始碼可理解 – 原始碼應該是可讀的和可理解的, 以便在程式碼稽覈中得到批准。儘管會有意外輸入或使用者操作, 但使軟體的行為具有可預知的方式。  



### 程式碼示例: 使用‘Joi’驗證複雜的JSON輸入

```javascript
var memberSchema = Joi.object().keys({
 password: Joi.string().regex(/^[a-zA-Z0-9]{3,30}$/),
 birthyear: Joi.number().integer().min(1900).max(2013),
 email: Joi.string().email()
});
 
function addNewMember(newMember)
{
 //assertions come first
 Joi.assert(newMember, memberSchema); //throws if validation fails
 //other logic here
}

```

### 反模式: 沒有驗證會產生令人討厭的錯誤

```javascript
//假如折扣為正，重定向使用者去列印他的折扣優惠劵
function redirectToPrintDiscount(httpResponse, member, discount)
{
    if(discount != 0)
        httpResponse.redirect(`/discountPrintView/${member.id}`);
}
 
redirectToPrintDiscount(httpResponse, someMember);
//忘記傳遞參數discount, 為什麼使用者被重定向到折扣頁面？

```

### 部落格引用: "您應該立即拋出這些錯誤"
 摘自部落格: Joyent
 
 > 一個退化情況是有人呼叫一個非同步函數但沒有傳遞一個回撥方法。你應該立即拋出這些錯誤, 因為程式有了錯誤, 最好的偵錯它的時機包括，獲得至少一個stack trace， 和理想情況下，核心檔案裡錯誤的點。為此, 我們建議在函數開始時驗證所有參數的類型。