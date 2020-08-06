## Node的特点

**单线程**：Node中Javascript执行是单线程的（但Node有底层线程池配合完成I/O任务）

**异步I/O**：提供了类似Web Worker的子进程，**弥补了单线程无法利用多核CPU的缺点**（单线程上存在CPU与I/O阻塞）

**事件驱动（事件与回调函数）**：**事件循环**机制，引入**工作线程池**处理复杂任务，将结果以**回调**的方式返回给主线程处理

* 在Javascript中，**函数式编程**，函数是一等公民，**将函数作为对象传参调用**是一大特点

* **优点**：基于事件驱动的服务器模型，可以构建**高性能服务器**，有效解决**高并发**问题

**跨平台**：让 JavaScript 可以实现跨平台运行







## Node 的运行原理（架构）

![image](https://user-gold-cdn.xitu.io/2019/8/2/16c502486f4a3dc0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

从左到右，从上到下，Node 被分为了四层：

- **应用层**：处理 JavaScript 交互，常见的就是 **Node 的模块**，如 http，fs
- **V8 引擎层**：利用 V8 来**解析 JavaScript** 语法，进而和下层 API 交互
- **Node API 层**：为上层模块提供系统调用，和操作系统进行交互
- **libuv 层**：**跨平台的底层封装**，实现了事件循环、文件操作等，是 Node 实现**异步的核心**。



### Node 异步实现原理（libuv）

* 调用一些特殊的 API 时，会进入对应 API 与 C++ 桥接通讯的 **Node C++ binding** ，向**工作线程池提交一个任务**
* libuv层中会将不同的任务**分配给不同的线程（工作线程池）**，形成并维护**事件循环**，以**异步回调**的方式将任务的执行结果返回给 V8 引擎（主线程）





## Node 中的线程

Node 通过**事件循环**（libuv层提供）的方式**运行 JavaScript 代码**（初始化和回调），并提供了一个**工作线程池**用于处理诸如文件 I/O 等高成本任务

* 主线程 + 线程池（多个工作线程），前面的负责编排任务，后面的执行
* Node 通过**底层的 libuv 及其提供的事件机制**来协调两种类型线程之间的工作，从而实现了让**单线程语言Javascript在Node中实现异步操作**
  * libuv层中会将不同的任务分配给不同的线程，形成一个事件循环（event loop），以异步的方式将任务的执行结果返回给 V8 引擎（主线程）

![img](https://user-gold-cdn.xitu.io/2019/8/2/16c502486f11ef9d?imageslim)

### Node是单线程的

**Node 的单线程**：指的是**主线程上 JavaScript 以单线程的方式运行**，并没有给 JavaScript 创建新线程的能力（本身就是单线程语言）

**优点**：避免了**多线程程序设计的复杂性**，消除了多线程中的**状态同步、死锁问题**，以及由于**线程上下文交换**所带来的**性能消耗**

**缺点**：

* Javascript的单线程无法利用**多核CPU**
* 一些错误可能会引起**整个应用退出**
* **大量计算占用CPU**导致无法继续调用异步I/O（浏览器中JS长时间执行会阻塞 UI 的渲染、响应）



### Node 中的工作线程池

**任务**：工作线程池被用于处理一些**高成本任务**，包括 I/O 操作、CPU 计算等，如：

1. I/O 密集型任务
   - **DNS**：用于 **DNS 解析**的模块，`dns.lookup()`, `dns.lookupService()`
   - **文件系统**：所有**文件系统 API**，除了 `fs.FSWatcher()` 和显式调用 API 如 `fs.readFileSync()` 之外
2. CPU 密集型任务：
   - Crypto：用于加密的模块
   - Zlib：用于压缩的模块，除了那些显式同步调用的 API 之外

**原理**：Node 的工作线程池是在 **libuv** 中实现的，libuv对外提供了通用的任务处理 API — `uv_queue_work`

* 调用上述 API，会进入对应 API 与 C++ 桥接通讯的 **Node C++ binding** ，向**工作线程池提交一个任务**
* 详见下一模块

---

> 浏览器中Web Worker：创建**工作线程**来进行**计算**，用**消息传递**来传递运行结果，**解决Javascript大计算**阻塞UI渲染的问题
>
> Node中的子进程 child_process：将**计算分发**到子进程，通过**进程间通信IPC**来传递结果（Master-Worker 管理各个工作进程）







## 事件循环

Node 通过**事件循环**（libuv层提供）的方式**运行 JavaScript 代码**（初始化和回调），并提供了一个**工作线程池**用于处理诸如文件 I/O 等高成本任务，**以异步的方式将任务的执行结果**返回给主线程

* 事件循环是 Node **实现异步 I/O 和事件驱动程序设计的基础**
* **优点**：非阻塞，解决高并发问题**【事件循环 => 异步I/O => 事件驱动 => 构建高性能的服务器】**
* **缺点**：**不符合常规的线性思路**，需要把一个完整的逻辑拆分为一个个事件，增加了开发和调试的难度

```js
// 1.执行到db.query时，不会等待结果返回，而是直接继续执行后面的语句，直到进入事件循环
// 2.当结果返回时，会将事件发送到事件队列
// 3.等到线程进入事件循环后，才会调用之前的回调函数继续执行后面的逻辑

db.query('SELECT * from some_table', function(res) {
  res.output();
});
```

![image](https://user-gold-cdn.xitu.io/2019/8/2/16c5024875ef6f03?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 描述Node的事件循环

1. node 初始化

   - 初始化 node 环境
   - 执行输入代码
   - **执行 process.nextTick 回调**
   - 执行 microtasks

2. **进入 Event Loop**

   1. timers 阶段：检查 timer 队列是否有到期的 timer回调（setTimeout/setInterval），有的话按 timerId 升序执行。
   2. IO callback 阶段：检查是否有等待（pending）的 IO 回调，有的话执行
   3. idle，prepare 阶段：完成nodejs 内部调用
   4. poll 阶段：检查是否存在尚未完成的回调，如果存在，分两种情况
      - 如果队列不为空（包含到期的定时器和 IO 事件），执行可用回调（未完成的回调可完成）
      - 如果队列为空，检查是否有 setImmediate 回调（队列空：未完成的回调还不可以触发）
        - 如果有，退出 poll 阶段，进入 check 阶段。
        - 如果没有，超时之前 node 阻塞在这里，等待新的事件通知。
      - 如果**不存在尚未完成的回调**，直接退出 poll 阶段
   5. check 阶段：如果有 setImmediate 回调，执行回调
   6. closing callback：如果套接字或处理函数突然关闭（例如 socket.destroy()），则'close' 事件将在这个阶段发出。

   ---

   > **在事件循环的每一个子阶段退出之前，都会执行：**
   >
   > 1. **检查是否有 process.nextTick，有的话执行**
   > 2. **执行 microtasks**
   > 3. **退出当前阶段**
   >
   > ---

3. 检查是否有活跃的 handlers（定时器，IO 事件句柄）

   - 如果有，继续下一轮循环
   - 没有，就退出事件循环，退出程序

![nodejs-event-loop-order](https://user-gold-cdn.xitu.io/2020/7/8/1732f172f4017f1d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





### 事件循环中的异步任务调度

事件循环的**使用场景**（异步任务调度）：异步 I/O 和非 I/O 的异步操作

- **异步 I/O** ：目的是**使 I/O 操作与 CPU 操作分离**，从而**非阻塞的调用底层接口**，如网络通信、磁盘 I/O、数据库访问等
  - Node 也提供了部分同步 I/O 方式，如`fs.readFileSync`，但 Node 并不推荐用户使用
- **非 I/O 的异步操作**：定时器，如 `setTimeout`、`setInterval`，以及 `process.nextTick`、`promise`、`setImmediate`



### process.nextTick

**作用**：为事件循环设置一项任务， Node 会**在下一轮事件循环时调用** callback【可以用于将复杂工作拆散，避免阻塞】

**潜在危害**：**递归调用 process.nextTick()** 会造成了其他队列的**饥饿（starve）现象**

---

**1、为什么 process.nextTick 不是在当前循环执行？**

1. 因为一个 Node 进程只有一个主线程，任何时刻都只有一个事件在执行。如果这个事件占用大量 CPU 时间，事件循环中的下一个事件就要等待很久
2. 使用 process.nextTick() 可以把复杂的工作**拆散**，**变成一个个较小的事件**，**避免大计算量的任务在事件循环同步执行，阻塞其他事件**（分两次）

```js
function doSomething(args, callback) {
  somethingComplicated(args);	// 立即执行
  process.nextTick(callback);	// 不立即执行callback，放入下一个事件循环处理，避免长时间占用CPU事件，阻塞其他事件
}
doSomething(function onEnd() {
  compute();
});

function test() { 
  process.nextTick(() => test());	// 阻塞整个 Event Loop
}
```

**2、定时器 setTimeout|setInterval 也能将任务拆分至下一次事件循环处理，为什么不用定时器来实现任务拆分？**

- 定时器的处理**涉及到最小堆操作**（超时），时间复杂度为 `O(lg(n))`
- `process.nextTick` 只是把回调函数放入**队列**之中，时间复杂度为 `O(1)`，更加高效

**3、setImmediate() & process.nextTick()**

`setImmediate()` 和 `process.nextTick()` 类似，也是**将回调函数延迟执行**。

* process.nextTick 并**不属于 Event Loop 中的某一阶段**，而在 Event loop 的**每一个阶段结束之前**，**直接执行** nextTickQueue 中插入的 tick，并且直到整个 Queue 处理完
* `setImmediate` 在**事件循环的 check 阶段**才会执行

**4、process.nextTick() **会优于**promise** 执行



### `setTimeout` vs `setImmediate`

`setTimeout` `setImmediate`的回调顺序无法保证

```js
setTimeout(function() {
  console.log('setTimeout')
}, 0);
setImmediate(function() {
  console.log('setImmediate')
});
```

**表现**：多次执行上述代码，会得到两种不同的输出结果。

**原因**：`setTimeou` 和 `setImmediate` **不一定处于同一个循环内**，所以它们的执行顺序无法确定（看例子2）

* 当事件循环启动时，**定时任务的回调可能尚未进入队列**（超时就放入，但无法保证），此时`setTimeout` 被跳过，转而执行了 **check 阶段**的任务。

---

```js
const fs = require('fs');

fs.readFile(__filename, () => {
    setTimeout(() => {
        console.log('timeout')
    }, 0);
    setImmediate(() => {
        console.log('immediate')
    })
});
```

对于这种情况，**immediate 将会永远先于 timeout 输出。**

1. 执行 `fs.readFile`，开始文件 I/O，**事件循环启动**
2. 文件读取完毕，相应的回调会被加入事件循环中的 **I/O 队列**
3. 事件循环执行到 **pending 阶段**，**执行 I/O 队列中的任务**
4. 回调函数执行过程中，定时器被加入 **timers 最小堆**中，`setImmediate` 的回调被加入 **immediates 队列**中
5. 当前事件循环处于 pending 阶段，继续执行到 **check 阶段**，此时发现 immediates 队列中存在任务，从而执行 `setImmediate` 注册的**回调函数**
6. **本轮事件循环执行完毕，进入下一轮**，在 **timers 阶段**执行 `setTimeout` 注册的回调函数



### 浏览器事件循环 & Node 事件循环

* 浏览器中的事件循环是 **HTML 规范**中制定的，由不同**浏览器厂商**自行实现：有**宏任务、微任务**之分
* Node 中的事件循环是由 **libuv 库**实现；**无宏任务**的概念，**调用了V8中实现的微任务**（但Node中的微任务与浏览器的微任务实际上不是同一个东西）

#### 浏览器环境

在浏览器中，JavaScript 执行为**单线程**（不考虑 web worker），所有代码均在**主线程调用栈**执行，当**主线程任务**清空后会去轮循**任务队列**中的任务

异步任务分为两种：**宏任务和微任务**，当满足执行条件时会被放入各自的**队列**中**，等待进入主线程执行**

- 宏任务：script 中的代码、`setTimeout`、`setInterval`、`I/O`、UI render
- 微任务：`promise`、`Object.observe`、`MutationObserver`

> task queue 实际上并非队列，而是**集合**（sets），因为事件循环的执行规则是**执行第一个可执行的任务**，而不是将第一个任务出队并执行。

#### Node 环境



![image](https://user-gold-cdn.xitu.io/2019/8/2/16c50252258f44a5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





## 为什么Node可以构建高性能服务器

因为Node是基于事件驱动的**（事件循环=>支持异步=>事件驱动=>高性能服务器）**

**经典的服务器模型**有以下几种：

- **同步式**：一次只能处理一个请求，其他请求都处于等待状态（阻塞）
- **每进程/每请求**：为每个请求启动一个进程，能够实现同时处理多个请求，但由于系统资源有限，不具备扩展性
- **每线程/每请求**：为每个请求启动一个线程，虽然线程比进程轻量，但是对于大型站点而言，依然不够（扛不住高并发请求，内存耗光）
- **事件驱动**：通过**事件驱动**的方式，（Node中配合**线程池**处理请求），**无需为请求创建额外的线程**（线程复用）
  - **优点**：可以**省去创建、销毁线程的开销**，同时因为线程较少，操作系统在调度任务时上下文切换的代价较低
  - **应用**：这种模式被很多平台所采用，如 Nginx(C)、Event Machine(Ruby)、 Node等。





# 基础

#### node 如何获取命令行传来的参数

`process.argv.splice(2)`

> **process** 是一个全局变量，它提供**当前 Node.js 进程**的有关信息
>
> **process.argv** 属性则返回一个**数组**，数组中的信息包括启动Node.js进程时的命令行参数
>
> - process.argv[0] : 返回**启动Node.js进程的可执行文件**所在的绝对路径
> - process.argv[1] : 为**当前执行的**JavaScript文件路径
> - process.argv.splice(2) : 移除前两者后，剩余的元素为其他命令行参数(也就是我们自定义部分)

---

举个场景：我们需要在script定义一个node的命令，然后执行改文件后获取不同的参数

![img](https://user-gold-cdn.xitu.io/2020/6/21/172d4fd57874aaf3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

获取命令行传来的参数还可以结合[commander](https://github.com/tj/commander.js)的 `commander.parse(process.argv);`



#### node有哪些相关的文件路径

Node 中的文件路径有 `__dirname`, `__filename`, `process.cwd()`, `./ 或者 ../`

- `__dirname`: 总是返回**被执行的 js 所在文件夹**的绝对路径
- `__filename`: 总是返回**被执行的 js** 的绝对路径
- `process.cwd()`: 总是返回**运行 node 命令时所在的文件夹**的绝对路径



#### node相关path API

- `path.dirname()`： 返回 **path 的目录名**
- `path.join()`：所有给定的 path 片段**连接**到一起，然后规范化生成的路径
- `path.resolve()`：将路径或路径片段的序列**解析为绝对路径**，解析为**相对于当前目录的绝对路径**，相当于cd命令

![img](https://user-gold-cdn.xitu.io/2020/6/21/172d54d34923f276?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### path.join() & path.resolve()

- join是把各个path片段连接在一起， resolve把`／`当成根目录
- join是直接拼接字段，resolve是解析路径并返回

```js
path.join('/a', '/b') // '/a/b'
path.resolve('/a', '/b') //'/b'

path.join("a","b")  // "a/b"
path.resolve("a", "b") // "/Users/tree/Documents/infrastructure/KSDK/src/a/b"
```





#### node如何实现文件读取

使用r**equire('fs')**载入fs模块，之后通过**fs文件系统模块**提供的API实现文件读取，

**fs模块**是node中重要的模块之一，主要用于文件的读写、移动、复制、删除、重命名等操作。

---

fs模块中所有方法都有**同步和异步**两种形式，eg：fs.rename对应的同步方法就是`fs.renameSync`

* 在繁忙的进程中，应使用异步方法， 同步的版本会阻塞整个进程（停止所有的连接）
* 无论同步异步尽量对抛出的异常做相应的处理

![img](https://user-gold-cdn.xitu.io/2020/6/21/172d57f461ff8901?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### node的url模块是用来干嘛的？

node的url模块用来对url的字符串解析、url组成等功能

- `url.parse`：可以将一个url的字符串解析并返回一个url的对象
- `url.format`:将传入的url对象编程一个url字符串并返回

![img](https://user-gold-cdn.xitu.io/2020/6/21/172d5da27c46a7d3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```js
Url {
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'baidu.com:8080',
  port: '8080',
  hostname: 'baidu.com',
  hash: '#node',
  search: '?query=js',
  query: 'query=js',
  pathname: '/test/h',
  path: '/test/h?query=js',
  href: 'http://baidu.com:8080/test/h?query=js#node'
}
```



#### node的http模块创建服务与Express或Koa框架有何不同?

express是一个**服务端框架**，支持node原生的写法，框架中**封装了node的http模块**，还封装了中间件、路由等特征，方便开发web服务器，换句话说express = http模块 + 中间件 + 路由

先看看`http`模块是如何实现一个简单的服务器

![img](https://user-gold-cdn.xitu.io/2020/6/21/172d595f0e3123f5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```
运行3000端口` 即可访问到浏览器打印出`hello node.js
```

接下来看看express如何实现

![img](https://user-gold-cdn.xitu.io/2020/6/21/172d5bd5a1df942b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

以上就实现一个简单的服务端逻辑，包含中间件、路由设置

####  Express和Koa框架中间件有什么不同？

> 答案：中间件： `app.use`方法就是**往中间件队列中塞入新的中间件**，express中间件处理方式是线性的，next过后继续寻找下一个中间件，当然如果没有调用next()的话，就不会调用下一个函数了，调用就会被终止

- express 中间件：是通过 next 的机制，即上一个中间件会通过 next 触发下一个中间件
- koa2 中间件：是通过 async await 实现的，中间件执行顺序是“洋葱圈”模型（推荐）

我们看下koa2下面这个简单的例子，你可以对比下Express的实现

![img](https://user-gold-cdn.xitu.io/2020/6/21/172d5cb771fd9d6f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1) 看看输出的日志

![img](https://user-gold-cdn.xitu.io/2020/6/21/172d5be181acd5d9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

#### 什么是模版引擎？

模板引擎是一个通过**结合页面模板、要展示的数据生成HTML页面**的工具，本质上是**后端渲染（SSR）**的需求，加上Node渲染页面本身是纯静态的，当我们需要页面多样化、更灵活，我们就需要使用模板引擎来强化页面，更好的凸显服务端渲染的优势

常见主流模版引擎有：

- art-template [官方文档](http://aui.github.io/art-template/) ：号称效率最高的，模版引擎
- ejs  [官方文档](http://ejs.co/)：是一个JavaScript模板库，用来从JSON数据中生成HTML字符串。
- pug [官方文档](https://pugjs.org/api/getting-started.html)：是一款健壮、灵活、功能丰富的模板引擎,专门为 Node.js 平台开发



#### 注册路由时 app.get、app.use、app.all 的区别是什么

app.use(path,callback) ：**只匹配前缀**，是express用来**调用中间件**的方法。中间件**通常不处理请求和响应**，一般**只处理输入数据**，并将其**交给队列中的下一个处理程序**

app.all(path,callback)：用作**路由处理**，**匹配完整路径（具体路由）**，可以指代所有的请求方式，在app.use之后 可以理解为包含了app.get、app.post等的定义

* 比如`app.all('/user/tree')`,能同时覆盖：`get('/user/tree') 、 post('/user/tree')、 put('/user/tree')`
* 可以匹配 * 路径来处理跨域请求

app.use(path,callback)



#### express response有哪些常用方法

express response对象是对**Node.js原生对象ServerResponse的扩展**，express response常见的有：`res.end()`、`res.send()`、`res.render()`、`res.redirect()`

- res.end()：**结束响应过程** - 如果服务端没有数据回传给客户端则可以直接用res.end返回

- res.send(body)：如果**服务端有数据**可以使用res.send，忽略res.end
  - body参数可以是一个Buffer对象，一个String对象或一个Array

* res.render：用来**渲染模板文件**，也可以结合模版引擎来使用
  *  [Express：模板引擎深入研究](https://zhuanlan.zhihu.com/p/67695057)
* res.redirect：**重定向**到path所指定的URL，同时也可以重定向时定义好HTTP状态码（默认为302）

```
res.redirect('http://baidu.com');
res.redirect(301, 'http://baidu.com');
```

![image-20200806111632700](C:\Users\GZS15720\AppData\Roaming\Typora\typora-user-images\image-20200806111632700.png)



#### node是怎样支持https?

通过openssl生成公钥私钥（不做详细介绍），然后基于 express的 https模块 实现，设置options配置, options有两个选项，一个是证书本体，一个是密码

![img](https://user-gold-cdn.xitu.io/2020/6/30/173034668387178e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



####  node和客户端怎么解决跨域的问题

在路由设置里面加了header的设置（用到了app.all()全匹配）

![img](https://user-gold-cdn.xitu.io/2020/6/30/1730393023ba8fc4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### node应用内存泄漏

nodejs是执行javascript的V8引擎，也就是说nodejs的GC就是说V8引擎的GC

> 内存泄漏（Memory Leak）指由于错误造成程序未能释放已经不再使用的内存的情况。如果内存持续占用过高，会影响服务器响应，情况严重直接能让程序奔溃，

##### 如何检测内存泄漏

- 通过内存快照，可以使用node-heapdump [官方文档](https://github.com/bnoordhuis/node-heapdump)获得内存快照进行对比，查找内存溢出
- 可视化内存泄漏检查工具 Easy-Monitor [官方文档](https://github.com/hyj1991/easy-monitor#readme)



#### 两个node程序之间怎样交互

通过child_process模块中fork的方式

**原理**：

- **子程序**用**process.on**来监听父程序的消息，用 **process.send**给父程序发消息
- **父程序**里用**child.on,child.send**进行交互，来实现父进程和子进程互相发送消息

![img](https://user-gold-cdn.xitu.io/2020/6/30/17303cba93ce609d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





# 1

https://juejin.im/post/6847902222504869901#nodejs

## V8 内存管理模型

node 程序运行中，此进程占用的所有内存称为**常驻内存**：

1. **代码区**：存放即将执行的代码片段
2. **栈**：存放局部变量
3. **堆**：存放对象、闭包上下文
4. **堆外内存**：由 c++分配，不受 V8 管理，也不会被 V8 回收。（Buffer 数据）







## 进程 Process

1. 查看进程：`ps -ef`

2. 当前进程的启动目录：`process.cwd()`

3. 改变工作目录：`process.chdir()`

4. 标准流

   - `process.stdin`
   - `process.stdout`
   - `process.stderr`

5. child_process.fork 与 POSIX 的 fork 有什么区别? Nodejs 的 child_process.fork()在 UNIX 上的实现最终调用了 POSIX 的 fork，但是 POSIX 需要手动管理子进程的资源释放，child_process.fork 不用担心这个问题，nodejs 自动释放，并且可以在 option 中选择父进程死后是否允许子进程存活

   - `spawn`启动一个子进程来执行命令
   - `exec`启动一个子进程带回调参数获知子进程的情况，可指定进程运行的超时时间。
   - `fork`加强版的 spawn，返回值是 child_process 对象，可以与子进程交互。

6. child.kill 与 child.send的区别：一个是基于信号系统, 一个是基于 IPC

7. 父进程或子进程的死亡是否会影响对方? 什么是孤儿进程?

   - 子进程死亡不会影响父进程, 不过子进程死亡时（线程组的最后一个线程，通常是“领头”线程死亡时），会向它的父进程发送死亡信号.

     - 子进程死亡时（处于“终止状态”），若父进程没有及时调用 wait() 或 waitpid() 来返回死亡进程的相关信息，此时子进程还有一个 PCB 残留在进程表中，被称作**僵尸进程**【PCB：进程控制块】

   - 反之父进程死亡, 一般情况下子进程也会随之死亡,

     - 但如果此时**子进程处于可运行态、僵死状态等等**的话, 子进程将被**进程 1（init 进程）**收养，从而成为**孤儿进程**.

       

## Koa

**compose：**

```js
function compose(middlewares){
   return ctx => {
      const dispatch = i => {
         const middleware = middlewares[i]
         if(!middleware) return
         return middleware(ctx, () => dispatch(i+1))
      }
      return dispatch(0)
   }
}
复制代码
```

**Context: ctx**

```js
class Context{
   constructor(req, res) {
      this.req=req
      this.res=res
   }
}
```

**Koa-mini**

```js
class Application() {
   constructor() {
      this.middlewares = []
   }
   listen(...args) {
      const server = http.createServer(async (req,res) => {
         const ctx = new Context(req,res)
         const fn = compose(this.middlewares)
         await fn(ctx)

         ctx.res.end(ctx.body)
      })
      server.listen(...args)
   }
   use(middleware){
      this.middlewares.push(middleware)
   }
}
```







## Node中的进程

https://zhuanlan.zhihu.com/p/27069865

### 1. Process

#### 1）操作系统的的进程

进程是一个应用程序的实例，同一个应用程序可以起多个实例（进程）。并且进程是一个**系统资源的集**合，这些资源包括内存、CPU等。同时进程也是**系统各项资源使用的标识**，像有了身份证才能办银行卡一样，各项如 fd、端口等资源都是通过进程为标识使用的。

> PS: 操作系统给每个进程都划分了单独的虚拟内存空间，以避免跨进程的内存注入问题。

```js
~ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Apr22 ?        00:00:14 init
root         2     1  0 May07 ?        00:00:00 [kthreadd/4230]
root         3     2  0 May07 ?        00:00:00 [khelper/4230]
root       127     1  0 Apr22 ?        00:00:00 upstart-udev-bridge --daemon
root       145     1  0 Apr22 ?        00:00:01 /lib/systemd/systemd-udevd --daemon
syslog     302     1  0 Apr22 ?        00:01:00 rsyslogd
...
```

其中的一些参数意义：

![img](https://picb.zhimg.com/80/v2-78b459d39c39863a74d4be0c43f0a081_720w.png)

 [ps](https://link.zhihu.com/?target=http%3A//man7.org/linux/man-pages/man1/ps.1.html) 命令中有一些参数是 Node.js 从进程内部拿不到的比如当前进程的父进程 ID，当前进程的 CPU 利用率等。



#### 2）process 对象

Node.js 的 process 对象是**一堆信息与操作的集合**。可能是由于 process 挺多功能是在 C++ 中 binding 的原因（为了开发方便）导致 process 上混合了很多功能，包括但不限于：

- 进程基础信息
- 进程 Usage
- 进程级事件
- 系统账户信息
- 环境变量：可以通过 **process.env** 来获取其值（配置方式：环境变量/配置文件）
- 信号收发
- 三个标准流
- Node.js 依赖模块/版本信息
- ......



### 2. Child Process

关于子进程 （child_process）模块这里简化一下内容介绍，主要分为 3 个部分：

- **exec**：**启动一个子进程**来执行命令，调用 bash 来解释命令，所以如果有命令有外部参数，则需要注意被注入的情况。
- **spawn**：更**安全的**启动一个子进程来执行命令，使用 option 传入各种参数来设置子进程的 stdin、stdout 等。通过内置的管道来与子进程建立 IPC 通信。
- **fork**：spawn 的特殊情况，专门用来**产生 worker 或者 worker 池**。
  -  返回值是 [ChildProcess ](https://zhuanlan.zhihu.com/p/goog_1177605021)[对象](https://link.zhihu.com/?target=https%3A//nodejs.org/dist/latest-v7.x/docs/api/child_process.html%23child_process_class_childprocess)，并且可以方便的与子进程交互。
  - fork之后在父进程和子进程之间开放一个IPC通道，使得不同的node进程间可以进行消息通信。



#### 进程间通信 IPC（有坑点）

![img](https://pic2.zhimg.com/80/v2-ed9ff8de5115954fc02981654f04da18_720w.png)

在 Node.js 中 IPC 的实现，在 Windows 上通过**命名管道**

#### Node 父子进程通信

Node.js 在启动子进程的时候，**主进程先建立 IPC 频道**，然后**将 IPC 频道的 fd (文件描述符)** 通过 **process.env** **环境变量**（NODE_CHANNEL_FD）的方式传递给子进程，然后**子进程通过 fd 连上 IPC 与父进程建立连接**。



### 3.Cluster

Node可以通过`cluster`模块来**利用多核CPU以及搭建一个用于负载均衡的node服务集群**

cluster 是**基于 child_process.fork 实现**的

* 可以实现**多个 worker 监听同一端口**，处理同一端口的请求
  * 正常情况一个端口被多个进程监听是会报**端口冲突**的（本质父子进程）
* **没有拷贝父进程的空间**，而是通过**加入 cluster.isMaster 来区分父、子进程**。
* cluster 产生的进程之间通过 **IPC** 来通信的：

主进程自己不是一个服务器，它只是负责**创建和维护子进程**，因为如果主进程也对外服务，一旦其失败，所有子进程都会失败。

> #### Node 父子进程通信
>
> Node.js 在启动子进程的时候，**主进程先建立 IPC 频道**，然后**将 IPC 频道的 fd (文件描述符)** 通过 **process.env** **环境变量**（NODE_CHANNEL_FD）的方式传递给子进程，然后**子进程通过 fd 连上 IPC 与父进程建立连接**。

```js
const cluster = require('cluster')
const http = require('http')
const numCPUs = require('os').cpus().length

if (cluster.isMaster) {
   // 仅父进程执行：父进程根据CPU数量创建一个/多个子进程
   for(i = 0; i<numCPUs;i++) {
      cluster.fork()	//本质上使用的还是child_process.fork()
   }
   cluster.on('exit', (worker) => {
      console.log(`worker ${worker.process.pid} died`)
      cluster.fork();	// 如果一个子进程失败了，父进程能够重启启动它
   })
} else {
   // 仅子进程执行
   // workers can share any TCP connection
   http.createServer((req, res) => {
      res.writeHead(200)
      res.end('hello world')
   }).listen(3000)
}
```



#### cluster 的负载均衡（不推荐）

Node.js 中 LB （load balance，**负载均衡**）是通过两个方式实现的 ①**句柄共享**（win）和 ② **round-robin**（*nix）

##### 句柄共享

描述：由**主进程创建 socket 监听端口**后，将 **socket 句柄**直接**分发**给感兴趣的 worker，然后当连接进来时，让 worker 直接自行 accept 然后处理。理论上这个方法应该是性能最好的, 但实际使用中**存在比较大的分配不均**的问题（常见情况是 8 个 worker，70%的连接跟其中的 2 个建立）。

##### round-robin，时间片轮转法

描述：round-robin，时间片轮转法是所有平台的默认方案（除了 windows）。主进程监听端口，接收到新连接之后，通过**时间片轮转法来决定将接收到的客户端的 socket 句柄传递给指定的 worker 处理**。至于每个连接由哪个 worker 来处理，完全由内置的**循环算法**决定。

 round-robin 的均衡程度比句柄共享要好一点



负载均衡的更优方案：nginx





