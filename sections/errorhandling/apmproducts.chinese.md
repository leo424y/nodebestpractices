# 使用 APM 產品發現錯誤和宕機時間


### 一段解釋

異常 != 錯誤。傳統的錯誤處理假定存在異常，但應用程式錯誤可能以程式碼路徑慢，API停機，缺少計算資源等形式出現。因為APM產品允許使用最小的設定來先前一步地檢測各種各樣 "深埋" 的問題，這是運用它們方便的地方。APM產品的常見功能包括: 當HTTP API返回錯誤時報警, 在API響應時間低於某個閾值時能被檢測, 覺察到‘code smells’，監視伺服器資源，包含IT度量的操作型智慧儀錶板以及其他許多有用的功能。大多數供應商提供免費方案。

### 關於 APM 的維基百科

在資訊科技和系統管理領域, 應用程式效能管理(APM)是對軟體應用程式的效能和可用性的監視和管理。APM努力檢測和診斷複雜的應用程式效能問題, 以維護預期的服務級別。APM是"將IT度量標準轉換為業務含義"

### 瞭解 APM 市場

APM 產品由3個主要部分構成:

1. 網站或API監控 – 通過HTTP請求不斷監視正常執行時間和效能的外部服務。可以在幾分鐘內安裝。以下是少數選定的競爭者: [Pingdom](https://www.pingdom.com/), [Uptime Robot](https://uptimerobot.com/), 和[New Relic](https://newrelic.com/application-monitoring)；

2. 程式碼檢測 – 這類產品需要在應用程式中嵌入代理, 以實現如檢測執行緩慢的程式碼、異常統計、效能監視等功能。以下是少數選定的競爭者: New Relic, App Dynamics；

3. 操作型智慧儀錶板 – 這些產品系列側重於為ops團隊提供度量和管理內容, 幫助他們輕鬆地保持應用程式效能維持在最佳狀態。這通常涉及聚合多個資訊源 (應用程式日誌、DB日誌、伺服器日誌等) 和前期儀錶板設計工作。以下是少數選定的競爭者: [Datadog](https://www.datadoghq.com/), [Splunk](https://www.splunk.com/), [Zabbix](https://www.zabbix.com/)；


 ### 示例: UpTimeRobot.Com – 網站監控儀錶板
![alt text](../../assets/images/uptimerobot.jpg "Website monitoring dashboard")

 ### 示例: AppDynamics.Com – 與程式碼檢測結合的端到端監視
![alt text](../../assets/images/app-dynamics-dashboard.png "end to end monitoring combined with code instrumentation")
