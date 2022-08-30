# 仔細挑選您的 CI 平臺

<br/><br/>


### 一段解釋

曾經，CI世界就是易於擴充套件的[Jenkins](https://jenkins.io/) vs 簡單方便的SaaS方案。遊戲正在改變，比如SaaS提供者[CircleCI](https://circleci.com/)和[Travis](https://travis-ci.org/)提供了強大的解決方案，包含最小化設定時間的Docker容器，而Jenkins也嘗試在簡單易用性上做文章而提高競爭性。雖然您可以在雲上設定豐富的CI解決方案, 如果它需要控制更多的細節Jenkins仍然是選擇的平臺。最終的選擇歸結為CI過程自定義的範圍: 免安裝，方便設定的雲供應商允許執行自定義shell命令、自定義的docker image、調整工作流、執行matrix build和其他豐富的功能。但是, 如果使用像Java這樣的正式程式語言來控制基礎結構或程式設計CI邏輯 - Jenkins可能仍然是首選。否則, 考慮選擇簡單方便和設定自由的雲選項。

<br/><br/>


### 程式碼示例 – 典型的雲CI配置，一個yml檔案就夠了
```javascript
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:4.8.2
      - image: mongo:3.4.4
    steps:
      - checkout
      - run:
          name: Install npm wee
          command: npm install
  test:
    docker:
      - image: circleci/node:4.8.2
      - image: mongo:3.4.4
    steps:
      - checkout
      - run:
          name: Test
          command: npm test
      - run:
          name: Generate code coverage
          command: './node_modules/.bin/nyc report --reporter=text-lcov'      
      - store_artifacts:
          path: coverage
          prefix: coverage

```


 
 ### Circle CI - 幾乎零設定的雲CI
![alt text](../../assets/images/circleci.png "API error handling")

### Jenkins - 完善和強大的CI
![alt text](../../assets/images/jenkins_dashboard.png "API error handling")

 
<br/><br/>
