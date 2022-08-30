# 將公用實用工具封裝成 npm 包

<br/><br/>


### 一段解釋
一旦你開始在不同的伺服器上增加不同的元件並使用不同的伺服器，這些伺服器會消耗類似的工具，那麼你應該開始管理依賴關係 - 你如何保留實用工具程式碼的一個副本，並讓多個使用元件者使用和部署？ 好吧，有一個這樣的框架，它被稱為npm ...首先用自己的程式碼包裝第三方實用工具包，以便將來可以輕鬆替換，併發布自己的程式碼作為私人npm包。 現在，您所有的程式碼庫都可以匯入該程式碼並受益於免費的依賴管理框架。 您可以釋出npm包供您自己的私人使用，而無需公開分享 [私人模組](https://docs.npmjs.com/private-modules/intro), [私人登錄檔](https://npme.npmjs.com/docs/tutorials/npm-enterprise-with-nexus.html) or [本地 npm 包](https://medium.com/@arnaudrinquin/build-modular-application-with-npm-local-modules-dfc5ff047bcc)


<br/><br/>


 ### 在環境和元件中共享你自己的公用實用工具
![alt text](../../assets/images/Privatenpm.png "構建解決方案的元件")
