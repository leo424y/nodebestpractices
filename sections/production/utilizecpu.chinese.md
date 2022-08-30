# 利用所有的CPU核心

<br/><br/>


### 一段解釋

這應該不會讓人感到意外, 在其基本形式上，Node執行在單程序，單執行緒，單個CPU上。購買了一個強大的包含4個或8個CPU的硬體，只使用一個聽起來是不可思議的，對嗎？適合中型應用最快的解決方案是使用Node的Cluster模組，它在10行程式碼中為每個邏輯核心和路由請求產生一個程序，程序之間以round-robin的形式存在。更好的是使用PM2，它通過一個簡單的介面和一個很酷的監視UI來給cluster模組裹上糖衣。雖然這個解決方案對傳統應用程式很有效，但它可能無法滿足需要頂級效能和健壯的devops流的應用。對於那些高階的用例，考慮使用自定義部署指令碼複製NODE程序，並使用像nginx
這樣的專門的工具進行負載均衡，或者使用像AWS ECS或Kubernetees這樣的容器引擎，這些工具具有部署和複製程序的高階特性。

<br/><br/>


### 比較:使用Node的clustre vs nginx做負載均衡

![Balancing using Node’s cluster vs nginx](../../assets/images/utilizecpucores1.png "Balancing using Node’s cluster vs nginx")

<br/><br/>

### 其他博主說什麼
* 摘自[Node.JS documentation](https://nodejs.org/api/cluster.html#cluster_how_it_works):
> ... 理論上第二種方法， Node clusters，應該是效能最佳的。然而, 在實踐中, 由於作業系統排程程式的反覆無常, 分佈往往非常不平衡。觀察負載, 所有連線中，超過70%在兩個程序中處理, 而總共有八個程序...

* 摘自部落格[StrongLoop](From the blog StrongLoop):
> ...通過Node的cluster模組實現叢集化。這使一個主程序能夠產生工作程序，並在工作程序間分配進入的連線。然而，與其直接使用這個模組，更好的選擇是使用其中的許多工具之一, 它為您自動處理它; 例如，node-pm或cluster-service...

* 摘自the Medium post[node.js程序負載平衡效能:比較cluster moudlle、iptables和Nginx](https://medium.com/@fermads/node-js-process-load-balancing- iptabls -and- Nginx -6746aaf38272)
> ... Node cluster易於實施和配置，不依賴於其他軟體就可以在Node的領域內進行。請記住，你的主程序將會工作幾乎和你的工作程序一樣多，比較於其他的解決方案，少一點請求率...