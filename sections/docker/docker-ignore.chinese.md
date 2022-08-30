# 使用.dockerignore防止洩漏機密

<br/><br/>

### 一段解釋

Docker的build命令會通過一個虛擬網路（virtual network）拷貝本地檔案到構建的上下文環境。注意 - 開發和CI資料夾會包含機密檔案，比如.npmrc，.aws，.env，以及其他一些敏感檔案。最終，Docker映象可能會包含機密資訊，並在不安全的區域暴露它們（例如，Docker repository，partners servers）。一個更好的方式是，Dockerfile應該明確地描述哪些檔案需要被複制。除此之外，包含一個.dockerginore檔案，還充當最後一個安全網，過濾掉不必要的資料夾和潛在的機密檔案。這樣做還可以加快構建速度 - 通過排除在生產環境並不會用到的通用開發資料夾（例如.git，測試結果，IDE配置），整個構建過程可以更好的使用快取，並取得一個更佳的效能。

<br/><br/>

### 程式碼示例 – 對於Node.js，一個好的預設.dockerignore示例

<details>
<summary><strong>.dockerignore</strong></summary>

```
**/node_modules/
**/.git
**/README.md
**/LICENSE
**/.vscode
**/npm-debug.log
**/coverage
**/.env
**/.editorconfig
**/.aws
**/dist
```

</details>

<br/><br/>

### 程式碼示例 反模式 - 遍歷拷貝所有檔案

<details>
<summary><strong>Dockerfile</strong></summary>

```dockerfile
FROM node:12-slim AS build

WORKDIR /usr/src/app
# 下一行拷貝所有檔案
COPY . .

# 剩餘部分

```

</details>
