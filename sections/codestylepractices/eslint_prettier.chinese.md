# 使用 ESLint 和 Prettier


### 比較 ESLint 和 Prettier

如果你使用ESLint格式化程式碼，它只是給你一個警告，比如這一行太寬（取決於你的`最大長度`設定）。Prettier會自動為你格式化。

```javascript
foo(reallyLongArg(), omgSoManyParameters(), IShouldRefactorThis(), isThereSeriouslyAnotherOne(), noWayYouGottaBeKiddingMe());
```

```javascript
foo(
  reallyLongArg(),
  omgSoManyParameters(),
  IShouldRefactorThis(),
  isThereSeriouslyAnotherOne(),
  noWayYouGottaBeKiddingMe()
);
```

Source: [https://github.com/prettier/prettier-eslint/issues/101](https://github.com/prettier/prettier-eslint/issues/101)

### 整合 ESLint 和 Prettier

ESLint和Prettier在程式碼格式化功能上有重疊, 但它可以很容易通過其他的包來解決，比如 [prettier-eslint](https://github.com/prettier/prettier-eslint), [eslint-plugin-prettier](https://github.com/prettier/eslint-plugin-prettier), 和 [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)。有關他們的差異的更多資訊，您可以檢視連結 [這裡](https://stackoverflow.com/questions/44690308/whats-the-difference-between-prettier-eslint-eslint-plugin-prettier-and-eslint).
