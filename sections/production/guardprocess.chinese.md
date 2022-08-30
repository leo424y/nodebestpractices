# 保護和重啟你的失敗程序（用正確的工具）

<br/><br/>


### 一段解釋

在基本級別，必須保護Node程序並在出現故障時重新啟動。簡單地說, 對於那些小應用和不使用容器的應用 – 像這樣的工具 [PM2](https://www.npmjs.com/package/pm2-docker) 是完美的，因為它們帶來簡單性，重啟能力以及與Node的豐富整合。其他具有強大Linux技能的人可能會使用systemd並將Node作為服務執行。對於使用Docker或任何容器技術的應用程式來說，事情會變得更加有趣，因為叢集管理和協調工具（比如[AWS ECS](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)，[Kubernetes](https://kubernetes.io/)等）會完成部署，監視和保持容器健康的功能。擁有所有豐富的叢集管理功能（包括容器重啟），為什麼還要與其他工具（如PM2）混為一談？這裡並沒有可靠的答案。將PM2保留在容器（主要是其容器特定版本[pm2-docker](https://www.npmjs.com/package/pm2-docker)）中作為第一個守護層是有充分的理由的 - 在主機容器要求正常重啟時，重新啟動更快，並提供特定於node的功能比如向程式碼傳送訊號。其他選擇可能會避免不必要的層。總而言之，沒有一個解決方案適合所有人，但瞭解這些選擇是最重要的。

<br/><br/>


### 其它博主說了什麼

* 來自[Express 生成最佳實踐](https://expressjs.com/en/advanced/best-practice-performance.html):
> ... 在開發中，您只需從命令列使用node.js或類似的東西啟動您的應用程式。**但是在生產中這樣做是一種災難。 如果應用程式崩潰，它將掉線**，直到您重新啟動它。要確保應用程式在崩潰時重新啟動，請使用程序管理器。流程管理器是便於部署的應用程式的“容器”，提供高可用性，並使您能夠在執行時管理應用程式。

* 摘自 the Medium blog post [瞭解節點叢集](https://medium.com/@CodeAndBiscuits/understanding-nodejs-clustering-in-docker-land-64ce2306afef#.cssigr5z3):
> ...瞭解Docker-Land中的NodeJS叢集“Docker容器”是流線型的輕量級虛擬環境，旨在將流程簡化為最低限度。管理和協調自己資源的流程不再有價值。**相反，像Kubernetes，Mesos和Cattle這樣的管理層已經普及了這些資源應該在整個基礎設施範圍進行管理的概念**。CPU和記憶體資源由“排程器”分配，網路資源由堆棧提供的負載均衡器管理。
