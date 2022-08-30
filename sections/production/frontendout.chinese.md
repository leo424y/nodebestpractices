# 在node外處理您的前端資產

<br/><br/>


### 一段解釋

在一個經典的 web 應用中，後端返回前端資源/圖片給瀏覽器, 在node的世界，一個非常常見的方法是使用 Express 靜態中介軟體, 以資料流的形式把靜態檔案返回到客戶端。但是, node並不是一個典型的 web應用, 因為它使用單個執行緒，對於同時服務多個檔案，未經過任何優化。相反, 考慮使用反向代理、雲端儲存或 CDN (例如Nginx, AWS S3, Azure Blob 儲存等), 對於這項任務, 它們做了很多優化，並獲得更好的吞吐量。例如, 像 nginx 這樣的專業中介軟體在檔案系統和網卡之間的直接掛鉤, 並使用多執行緒方法來減少多個請求之間的干預。

您的最佳解決方案可能是以下形式之一:
1. 反向代理 – 您的靜態檔案將位於您的node應用的旁邊, 只有對靜態檔案資料夾的請求才會由位於您的node應用前面的代理 (如 nginx) 提供服務。使用這種方法, 您的node應用負責部署靜態檔案, 而不是為它們提供服務。你的前端的同事會喜歡這種方法, 因為它可以防止 cross-origin-requests 的前端請求。
2. 雲端儲存 – 您的靜態檔案將不會是您的node應用內容的一部分, 他們將被上傳到服務, 如 AWS S3, Azure BlobStorage, 或其他類似的服務, 這些服務為這個任務而生。使用這種方法, 您的node應用即不負責部署靜態檔案, 也不為它們服務, 因此, 在node和前端資源之間完全解耦, 這是由不同的團隊處理。

<br/><br/>


### 程式碼示例: 對於靜態檔案，典型的nginx配置

```
# configure gzip compression
gzip on;
keepalive 64;

# defining web server
server {
listen 80;
listen 443 ssl;

# handle static content
location ~ ^/(images/|img/|javascript/|js/|css/|stylesheets/|flash/|media/|static/|robots.txt|humans.txt|favicon.ico) {
root /usr/local/silly_face_society/node/public;
access_log off;
expires max;
}
```

<br/><br/>

### 其它博主說了什麼
摘自部落格 [StrongLoop](https://strongloop.com/strongblog/best-practices-for-express-in-production-part-two-performance-and-reliability/):

>…開發模式下, 您可以使用 [res.sendFile()](http://expressjs.com/4x/api.html#res.sendFile) 服務靜態檔案. 但是不要在生產中這樣做, 因為這個函數為了每個檔案請求，必須從檔案系統中讀取, 因此它會遇到很大的延遲, 並影響應用程式的整體效能。請注意, res.sendFile() 沒有用系統呼叫 sendFile 實現, 這將使它更高效。相反, 使用serve-static中介軟體 (或類似的東西), 在express應用中服務檔案，這是優化了的。更佳的選擇是使用反向代理來服務靜態檔案; 有關詳細資訊, 請參閱使用反向代理…

<br/><br/>
