# 使用node命令而不是npm啟動容器

## 一段解釋

我們經常看到開發者使用`CMD 'npm start'`啟動app的程式碼示例。這是一個不好的做法。因為`npm`不會向您的app轉發訊號（signals），這將阻止應用優雅關閉（graceful shutdown），（見[/sections/docker/graceful-shutdown.md]）。如果您使用了子程序（child-processes），在意外關閉時則無法正確清理它們，將殭屍程序留在主機上。同時，`npm start`也導致無意義的增加一個額外程序。使用`CMD ['node','server.js']`啟動您的應用吧。假如您的應用使用了子程序（child-processes），也可以使用`TINI`作為入口（entrypoint）。

### 程式碼示例 - 啟動Node

```dockerfile
FROM node:12-slim AS build

WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm ci --production && npm clean cache --force

CMD ["node", "server.js"]
```


### 程式碼示例 - 使用Tiny作為入口（ENTRYPOINT）

```dockerfile
FROM node:12-slim AS build

# 使用子程序（child-processes）的情況下，新增Tini
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm ci --production && npm clean cache --force

ENTRYPOINT ["/tini", "--"]

CMD ["node", "server.js"]
```

### 反模式

使用npm start
```dockerfile
FROM node:12-slim AS build

WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm ci --production && npm clean cache --force

# 不要這麼做!
CMD "npm start"
```

在同一字元串命令裡面使用node，將啟動一個bash/ash指令碼程序去執行您的命令。它和使用`npm`的效果類似。

```dockerfile
FROM node:12-slim AS build

WORKDIR /usr/src/app
COPY package.json package-lock.json ./
RUN npm ci --production && npm clean cache --force

# 不要這麼做，它將啟動bash
CMD "node server.js"
```

使用npm啟動，這裡是程序樹：
```console
$ ps falx
  UID   PID  PPID   COMMAND
    0     1     0   npm
    0    16     1   sh -c node server.js
    0    17    16    \_ node server.js
```
額外的兩個程序沒有任何好處。

來源:


https://maximorlov.com/process-signals-inside-docker-containers/


https://github.com/nodejs/docker-node/blob/master/docs/BestPractices.md#handling-kernel-signals