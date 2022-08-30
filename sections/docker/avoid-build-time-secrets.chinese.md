# 清理編譯過程中的祕鑰，避免使用祕鑰作為參數

<br/><br/>

### 一段解釋

Docker映像不僅僅是一堆檔案，而是展示構建期間所發生的層級關係。在普通場景中，開發人員在構建過程中需要知道npm令牌（主要是對於私有registry）- 會通過把令牌作為構建參數來傳遞，但這種實現是錯誤的。這可能看起來是沒什麼問題，並且安全，但此令牌可以從開發人員機器中的Docker歷史記錄、Docker registry和CI中獲得。獲取到該令牌的攻擊者就能夠擁有寫入此組織私有npm registry的許可權。還有兩個更安全的替代方法：完美無瑕的一個替代方案是使用Docker --secret功能（截至2020年7月還在實驗階段），它只允許在構建期間掛載（mount）檔案。第二種方法是使用帶args的多階段（multi-stage）構建，然後只將必要的檔案複製到生產環境。後一種技術將不會同時一起提供祕鑰與映象，但祕鑰將出現在本地Docker的歷史記錄中 - 對大多陣列織來說，這通常被認為足夠安全。

<br/><br/>

### 程式碼示例 – 使用Docker掛載祕鑰（實驗功能但穩定）

<details>

<summary><strong>Dockerfile</strong></summary>

```dockerfile
# syntax = docker/dockerfile:1.0-experimental

FROM node:12-slim

WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN --mount=type=secret,id=npm,target=/root/.npmrc npm ci

# 剩餘部分
```

</details>

<br/><br/>

### 程式碼示例 – 使用多階段（multi-stage build）安全構建

<details>

<summary><strong>Dockerfile</strong></summary>

```dockerfile
FROM node:12-slim AS build

ARG NPM_TOKEN

WORKDIR /usr/src/app
COPY . /dist
RUN echo "//registry.npmjs.org/:\_authToken=\$NPM_TOKEN" > .npmrc && \
 npm ci --production && \
 rm -f .npmrc


FROM build as prod

COPY --from=build /dist /dist
CMD ["node", "index.js"]

# ARG和.npmrc在最終的映象中不會出現，但會在Docker daemon的未打標籤（un-tagged）映象列表中找到它們 - 確保刪除他們 
```

</details>

<br/><br/>

### 程式碼示例 反模式 - 使用構建參數

<details>

<summary><strong>Dockerfile</strong></summary>

```dockerfile
FROM node:12-slim

ARG NPM_TOKEN

WORKDIR /usr/src/app
COPY . /dist
RUN echo "//registry.npmjs.org/:\_authToken=\$NPM_TOKEN" > .npmrc && \
 npm ci --production && \
 rm -f .npmrc

# 在拷貝命令的同時刪除.npmrc檔案不會在layer裡面儲存它, 但在映象歷史裡面還是會找到它

CMD ["node", "index.js"]
```

</details>

<br/><br/>

### 部落格引用: "祕鑰不會儲存在最終的Docker中"

摘自部落格, [Alexandra Ulsh](https://www.alexandraulsh.com/2019/02/24/docker-build-secrets-and-npmrc/?fbclid=IwAR0EAr1nr4_QiGzlNQcQKkd9rem19an9atJRO_8-n7oOZXwprToFQ53Y0KQ)

> 在2018年11月，Docker18.09在docker構建過程中引入一個新的標誌（flag）--secret。它允許使用者通過一個檔案傳遞祕鑰到Docker包中（build）。這些祕鑰不會儲存在最終的Docker映象，中間映象和映象提交歷史裡面。藉助構建參數secret，您現在可以使用私有npm包安全地構建Docker映像，而無需構建參數和多階段（multi-stage）構建。

```

```
