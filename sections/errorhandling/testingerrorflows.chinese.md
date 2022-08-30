# 使用您喜歡的測試框架測試錯誤流


### 一段解釋

測試‘正確’路徑並不比測試失敗更好。良好的測試程式碼覆蓋率要求測試異常路徑。否則，異常確實被處理正確是不可信的。每個單元測試框架，如[Mocha](https://mochajs.org/) & [Chai](http://chaijs.com/)，都支援異常測試（請看下面的程式碼示例）。如果您覺得測試每個內部函數和異常都很乏味，那麼您可以只測試REST API HTTP錯誤。



### 程式碼示例: 使用 Mocha & Chai 確保正確的異常被拋出

```javascript
describe("Facebook chat", () => {
 it("Notifies on new chat message", () => {
 var chatService = new chatService();
 chatService.participants = getDisconnectedParticipants();
 expect(chatService.sendMessage.bind({message: "Hi"})).to.throw(ConnectionError);
 });
});

```

### 程式碼示例: 確保 API 返回正確的 HTTP 錯誤碼

```javascript
it("Creates new Facebook group", function (done) {
 var invalidGroupInfo = {};
 httpRequest({method: 'POST', uri: "facebook.com/api/groups", resolveWithFullResponse: true, body: invalidGroupInfo, json: true
 }).then((response) => {
 //oh no if we reached here than no exception was thrown
 }).catch(function (response) {
 expect(400).to.equal(response.statusCode);
 done();
 });
 });

```