# 鎖定依賴版本

<br/><br/>


### 一段解釋



您的程式碼依賴於許多外部包，假設它“需要”和使用momentjs-2.1.4，預設情況下，當佈署到生產中時，npm可能會獲得momentjs 2.1.5，但不幸的是，這將帶來一些新的bug。使用npm配置檔案和設定 ```–save-exact=true``` 指示npm去完成安裝，以便下次執行 ```npm install```（在生產或在Docker容器中，您計劃將其用於測試）時，將獲取相同的依賴版本。另一種可選擇受歡迎的方法是使用一個shrinkwrap檔案（很容易使用npm生成）指出應該安裝哪些包和版本，這樣就不需要環境來獲取新版本了。

* **更新:** 在npm5中，使用.shrinkwrap依賴項會被自動鎖定。Yarn，一個新興的包管理器，預設情況下也會鎖定依賴項。

<br/><br/>


### 程式碼示例: .npmrc檔案指示npm使用精確的版本

```
// 在項目目錄上儲存這個為.npmrc 檔案
save-exact:true
```

<br/><br/>

### 程式碼示例: shirnkwrap.json檔案獲取準確的依賴關係樹

```javascript
{
    "name": "A",
    "dependencies": {
        "B": {
            "version": "0.0.1",
            "dependencies": {
                "C": { 
                    "version": "0.1.0"
                }
            }
        }
    }
}
```

<br/><br/>

### 程式碼示例: npm5依賴鎖檔案 - package.json

```javascript
{
    "name": "package-name",
    "version": "1.0.0",
    "lockfileVersion": 1,
    "dependencies": {
        "cacache": {
            "version": "9.2.6",
            "resolved": "https://registry.npmjs.org/cacache/-/cacache-9.2.6.tgz",
            "integrity": "sha512-YK0Z5Np5t755edPL6gfdCeGxtU0rcW/DBhYhYVDckT+7AFkCCtedf2zru5NRbBLFk6e7Agi/RaqTOAfiaipUfg=="
        },
        "duplexify": {
            "version": "3.5.0",
            "resolved": "https://registry.npmjs.org/duplexify/-/duplexify-3.5.0.tgz",
            "integrity": "sha1-GqdzAC4VeEV+nZ1KULDMquvL1gQ=",
            "dependencies": {
                "end-of-stream": {
                    "version": "1.0.0",
                    "resolved": "https://registry.npmjs.org/end-of-stream/-/end-of-stream-1.0.0.tgz",
                    "integrity": "sha1-1FlucCc0qT5A6a+GQxnqvZn/Lw4="
                }
            }
        }
    }
}
```
