# antd 오류 : Failed to parse source map: 'webpack://antd/./components/config-provider/style/index.less' URL is not supported

- antd에서 css를 가져오려고 `App.tsx` 와 `index.tsx` 에 직접 `import 'antd/dist/antd.css';` 를 했더니 아래와 같은 오류가 발생하였다.

```shell
WARNING in ./node_modules/antd/dist/antd.css (./node_modules/css-loader/dist/cjs.js??ruleSet[1].rules[1].oneOf[5].use[1]!./node_modules/postcss-loader/dist/cjs.js??ruleSet[1].rules[1].oneOf[5].use[2]!./node_modules/source-map-loader/dist/cjs.js!./node_modules/antd/dist/antd.css)
Module Warning (from ./node_modules/source-map-loader/dist/cjs.js):
Failed to parse source map: 'webpack://antd/./components/config-provider/style/index.less' URL is not supported

WARNING in ./node_modules/antd/dist/antd.css (./node_modules/css-loader/dist/cjs.js??ruleSet[1].rules[1].oneOf[5].use[1]!./node_modules/postcss-loader/dist/cjs.js??ruleSet[1].rules[1].oneOf[5].use[2]!./node_modules/source-map-loader/dist/cjs.js!./node_modules/antd/dist/antd.css)
Module Warning (from ./node_modules/source-map-loader/dist/cjs.js):
Failed to parse source map: 'webpack://antd/./components/icon/style/index.less' URL is not supported
```

### 해결법

`index.css` 에 `@import '~antd/dist/antd.css';` 하면 된다.

```css
@import '~antd/dist/antd.css';

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
    monospace;
}
```

