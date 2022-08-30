
# 監控！

<br/><br/>

### 一段解釋

基本來說，當在生產環境中發生意外時，監控意味著你能夠很*容易*識別它們。比如，通過電子郵件或Slack獲得通知。挑戰在於選擇既能滿足你的需求又不會破壞防護的合適工具集。我建議, 首先定義一組核心的度量標準, 這些指標必須被監視, 以確保健康狀態 – CPU, 伺服器RAM, Node程序RAM（小於1.4GB），最後一分鐘的錯誤數量，程序重啟次數，平均響應時間。然後去看看你可能喜歡的一些高階功能，並新增到你的願望清單。一些高階監控功能的例子：DB分析，跨服務測量（即測量業務事務），前端整合，將原始資料展示給自定義BI客戶端，Slack 通知等等。

要實現高階功能需要冗長的設定或購買諸如Datadog，Newrelic之類的商業產品。不幸的是，實現基本功能也並不容易，因為一些測量標準是與硬體相關的（CPU），而其它則在node程序內（內部錯誤），因此所有簡單的工具都需要一些額外的設定。例如，雲供應商監控解決方案（例如[AWS CloudWatch](https://aws.amazon.com/cloudwatch/), [Google StackDriver](https://cloud.google.com/stackdriver/))能立即告訴您硬體度量標準，但不涉及內部應用程式行為。另一方面，基於日誌的解決方案（如ElasticSearch）預設缺少硬體檢視。解決方案是通過缺少的指標來增加您的選擇，例如，一個流行的選擇是將應用程式日誌傳送到[Elastic stack](https://www.elastic.co/products)並配置一些額外的代理（例如[Beat](https://www.elastic.co/products)）來共享硬體相關資訊以獲得完整的展現。

<br/><br/>

### 監控示例：AWS cloudwatch預設儀表板。很難提取應用內指標 

![AWS cloudwatch default dashboard. Hard to extract in-app metrics](../../assets/images/monitoring1.png)

<br/><br/>

### 監控示例：StackDriver預設儀表板。很難提取應用內指標

![StackDriver default dashboard. Hard to extract in-app metrics](../../assets/images/monitoring2.jpg)

<br/><br/>

### 監控示例：Grafana作為視覺化原始資料的UI層


![Grafana as the UI layer that visualizes raw data](../../assets/images/monitoring3.png)

<br/><br/>
### 其他博主說了什麼
  摘自部落格 [Rising Stack](https://blog.risingstack.com/node-js-performance-monitoring-with-prometheus/):

> ...我們建議您為所有服務監聽這些訊號：
> 錯誤率：因為錯誤是使用者面對的，並立即會影響您的客戶。
> 響應時間：因為延遲會直接影響您的客戶和業務。
> 吞吐量：流量可幫助您瞭解增加的錯誤率和延遲的上下文。
> 飽和度：飽和度告訴你你的服務有多“滿”。如果CPU使用率是90％，您的系統可以處理更多的流量嗎？...
