
<!--introduced_in=v0.10.0-->
<!-- type=misc -->

Node.js 插件是用 C++ 编写的动态链接共享对象，可以使用 [`require()`][require] 函数加载到 Node.js 中，且像普通的 Node.js 模块一样被使用。
它们主要用于为运行在 Node.js 中的 JavaScript 与 C/C++ 库之间提供接口。

目前用于实现插件的方法相当复杂，涉及多个组件和 API 的知识：

 - V8：Node.js 目前用于提供 JavaScript 实现的 C++ 库。
   V8 提供了用于创建对象、调用函数等的机制。
   V8 的 API 文档主要在 `v8.h` 头文件中（Node.js 源代码中的 `deps/v8/include/v8.h`），也可以在查看 [V8 在线文档][v8-docs]。

 - [libuv]：实现了 Node.js 的事件循环、工作线程、以及平台所有的的异步操作的 C 库。
   它也是一个跨平台的抽象库，使所有主流操作系统中可以像 POSIX 一样访问常用的系统任务，比如与文件系统、socket、定时器、以及系统事件的交互。
   libuv 还提供了一个类似 POSIX 多线程的线程抽象，可被用于强化更复杂的需要超越标准事件循环的异步插件。
   建议插件开发者多思考如何通过在 libuv 的非阻塞系统操作、工作线程、或自定义的 libuv 线程中降低工作负载来避免在 I/O 或其他时间密集型任务中阻塞事件循环。

 - 内置的 Node.js 库。Node.js 自身开放了一些插件可以使用的 C++ API。
   其中最重要的是 `node::ObjectWrap` 类。

 - Node.js 包含一些其他的静态链接库，如 OpenSSL。
   这些库位于 Node.js 源代码中的 `deps/` 目录。
   只有 V8 和 OpenSSL 符号是被 Node.js 开放的，并且通过插件被用于不同的场景。
   更多信息可查看[链接到 Node.js 自身的依赖][Linking to Node.js' own dependencies]。

以下例子可从 [Node 插件示例][download]下载，作为学习插件开发的起点。

