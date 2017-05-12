---
title: NAN_node-addon-examples笔记
---

github链接: [node-addon-examples](https://github.com/nodejs/node-addon-examples/blob/master/1_hello_world/README.md)

#### 工作步骤
1 `npm init` 在项目根目录初始化 
2 `npm install nan@latest --save` ,`npm install node-gyp -g`
3 添加`"gypfile": true`至`package.json`，在项目根目录新建`binding.gyp` （YAML格式）
4 编写`helloworld.cc`
5 `node-gyp configure` 然后 `node-gyp build`，完成后生成` ./build/Release/hello.node.`
6 编写`helloworld.js`


#### NAN_METHOD
```
NAN_METHOD(Method) {
  NanScope();
  NanReturnValue(String::New("world"));
}
```
* NAN的作用：
变化的V8 API使得难以使用相同的C ++代码定位不同版本的Node，所以NAN有助于提供**简单的映射**，因此我们可以定义FunctionTemplate将接受的V8兼容功能。
* NanScope（）的作用：
用于设置一个V8“句柄范围”，它与JavaScript中的函数范围非常相似。
* NanReturnValue（）的作用：
设置函数的返回值。在这种情况下，我们正在创建一个简单的V8 String对象，其内容为“world”，这将作为标准的JavaScript字符串显示，值为“world”。