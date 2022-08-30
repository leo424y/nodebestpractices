# 委託任何可能的 (例如靜態內容，gzip) 到反向代理

<br/><br/>


### 一段解釋

過度使用Express，及其豐富的中介軟體去提供網路相關的任務，如服務靜態檔案，gzip 編碼，throttling requests，SSL termination等，是非常誘人的。由於Node.js的單執行緒模型，這將使CPU長時間處於忙碌狀態 (請記住，node的執行模型針對短任務或非同步IO相關任務進行了優化)，因此這是一個效能消耗。一個更好的方法是使用專注於處理網路任務的工具 – 最流行的是nginx和HAproxy，它們也被最大的雲供應商使用，以減輕在Node.js程序上所面臨的負載問題。

<br/><br/>


### 程式碼示例 – 使用 nginx 壓縮伺服器響應

```
# 配置 gzip 壓縮
gzip on;
gzip_comp_level 6;
gzip_vary on;

# 配置 upstream
upstream myApplication {
    server 127.0.0.1:3000;
    server 127.0.0.1:3001;
    keepalive 64;
}

#定義 web server
server {
    # configure server with ssl and error pages
    listen 80;
    listen 443 ssl;
    ssl_certificate /some/location/sillyfacesociety.com.bundle.crt;
    error_page 502 /errors/502.html;

    # handling static content
    location ~ ^/(images/|img/|javascript/|js/|css/|stylesheets/|flash/|media/|static/|robots.txt|humans.txt|favicon.ico) {
    root /usr/local/silly_face_society/node/public;
    access_log off;
    expires max;
}
```

<br/><br/>

### 其他部落格作者說什麼

* 摘自部落格 [Mubaloo](http://mubaloo.com/best-practices-deploying-node-js-applications):
> …很容易落入這個陷阱 – 你看到一個包比如Express，並認為 "真棒!讓我們開始吧" – 你編寫了程式碼，你實現了一個應用程式，做你想要的。這很好，老實說，你已經完成了大部分的事情。但是，如果您將應用程式上傳到伺服器並讓它偵聽 HTTP 埠，您將搞砸這個應用，因為您忘記了一個非常關鍵的事情: node不是 web 伺服器。**一旦任何流量開始訪問你的應用程式， 你會發現事情開始出錯:  連線被丟棄，資源停止服務，或在最壞的情況下，你的伺服器崩潰。你正在做的是試圖讓node處理所有複雜的事情，而這些事情讓一個已驗證過了的 web 伺服器來處理，再好也不會過。為什麼要重新造輪子？It**
> **這只是為了一個請求，為了一個影象，並銘記在腦海中，您的應用程式可以用於重要的東西，如讀取資料庫或處理複雜的邏輯; 為了方便起見，你為什麼要削弱你的應用？**


* 摘自部落格 [Argteam](http://blog.argteam.com/coding/hardening-node-js-for-production-part-2-using-nginx-to-avoid-node-js-load):
> 雖然 express.js 通過一些connect中介軟體處理靜態檔案，但你不應該使用它。**Nginx 可以更好地處理靜態檔案，並可以防止請求動態內容堵塞我們的node程序**…
