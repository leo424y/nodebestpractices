# 使用工具自動檢測有漏洞的依賴項

<br/><br/>

### 一段解釋

現代node應用有數十個, 有時是數以百計的依賴。如果您使用的任何依賴項存在已知的安全漏洞, 您的應用也很容易受到攻擊。
下列工具自動檢查依賴項中的已知安全漏洞:
[npm audit](https://docs.npmjs.com/cli/audit) - Node 安全工程
[snyk](https://snyk.io/) - 持續查詢和修復依賴中的漏洞

<br/><br/>

### 其他博主說什麼
摘自部落格 [StrongLoop](https://strongloop.com/strongblog/best-practices-for-express-in-production-part-one-security/) :

> ...被用來管理您應用的依賴是強大且方便的。但是, 您使用的依賴包可能存在嚴重的安全漏洞, 也會影響您的應用。您的應用的安全性僅僅與您的依賴元件中的 "最薄弱的一環" 一樣嚴重。幸運的是, 您可以使用兩種有用的工具來確保您使用的第三方庫: ** 和 requireSafe。這兩種工具在很大程度上都是一樣的, 所以使用兩種方法都可能過於誇張, 但 "安全比抱歉" 更安全...
