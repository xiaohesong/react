- 安装依赖
clone下来之后执行`npm i`,会有报错：
```shell
npm ERR! code EUNSUPPORTEDPROTOCOL
npm ERR! Unsupported URL Type "link:": link:./scripts/eslint-rules/

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/username/.npm/_logs/2019-03-23T06_40_48_484Z-debug.log
```
解决办法就是把`package.json`中的`"file:./scripts/eslint-rules/"`这个`link`改成`file`。