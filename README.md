- 安装依赖
clone下来之后执行`npm i`,会有报错：
```shell
npm ERR! code EUNSUPPORTEDPROTOCOL
npm ERR! Unsupported URL Type "link:": link:./scripts/eslint-rules/

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/username/.npm/_logs/2019-03-23T06_40_48_484Z-debug.log
```
解决办法就是把`package.json`中的`"file:./scripts/eslint-rules/"`这个`link`改成`file`。

# 源码阅读

**注意：** 看代码之前，请先确定你对源码库有个[大概的了解](https://github.com/xiaohesong/TIL/blob/master/front-end/react/codebase-overview.md)

> `warning`和`invariant`都是在条件`false`时触发，`warning`会继续下去，`invariant`会终止。

# ReactDOM
先看ReactDOM的render。从react-dom文件夹下client下的ReactDOM开始。点击进去看对外暴露的`ReactDOM`对象内的`render`函数。
