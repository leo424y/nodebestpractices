# 清除NODE_MODULE快取

<br/><br/>

### 一段解釋

node包管理器，npm和Yarn，會本地快取安裝過的包，以便在未來的項目中，如果需要同樣的包，就不需要從遠端倉庫重新獲取。儘管這會導致包的重複，消耗更多的儲存 - 作為回報，它維持了一個安裝相同包的本地開發環境。而在Docker容器中，這種儲存是沒什麼價值的，因為它僅僅安裝依賴一次。通過移除這類快取，只需要使用一行程式碼，上十MB的儲存會從image中移除。當這樣做的時候，確保它不會通過非零（non-zero）碼退出，因而由於快取問題導致CI構建失敗 - 這可以通過新增一個force標誌位來避免。

*請注意如果您使用multi-stage構建，只要您在最後階段不安裝新的包，清除快取是沒意義的*

<br/><br/>

### 程式碼示例 - 清除快取

<details>
<summary><strong>Dockerfile</strong></summary>

```dockerfile
FROM node:12-slim AS build
WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm ci --production && npm cache clean --force

# 剩餘部分
```

</details>