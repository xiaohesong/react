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

这个`render`主要是调用`legacyRenderSubtreeIntoContainer`函数，然后这个函数又在内部调用了`legacyCreateRootFromDOMContainer`函数，这个`legacyCreateRootFromDOMContainer`去实例化了`ReactRoot`函数,(然后`ReactRoot`函数内部有个`createContainer`被签名，这里就是重点了，`createContainer`实际上是`ReactFiberReconciler`中的`createContainer`在作用，然而这个`createContainer`签名函数调用的却是`ReactFiberRoot`内的`createFiberRoot`, `createFiberRoot`内有一个`createHostRootFiber`操作，这个`createHostRootFiber`实际上是`ReactFiber`里面的`createHostRootFiber`在处理，他去`createFiber`，这个`createFiber`就是去实例化一个`FiberNode`，然后返回这个实例化之后的对象，实例化`FiberNode`之后，`ReactFiberRoot`中的`createFiberRoot`会生成一个`root`对象，这个对象有在`current`属性上赋值前面生成的`FiberNode`对象 ), 所以在`ReactRoot`内有一个`root`变量去接收返回的对象(接收的这个对象的`current`属性被赋予了`FiberNode`的对象), 然后赋值给内部字段`_internalRoot`。所以这个时候`legacyCreateRootFromDOMContainer`函数内实例化的`ReactRoot`是一个包含`FiberNode`对象的对象。
上面是创建容器的过程，也可以看出，创建容器其实就是以`FiberNode`为主。然后接下来就是以`ReactDOM.js`中的`legacyRenderSubtreeIntoContainer`签名继续向下。
因为初始化的时候不存在`parentComponent`，所以直接`root.render(children, callback);`。
