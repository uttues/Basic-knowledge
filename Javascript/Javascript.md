## truthy & true

在JavaScript中，**truthy（真值）**指的是在布尔值上下文中，**转换后的值为真的值**。

* **除 `false`、`0`、`""`、`null`、`undefined` 和 `NaN` 以外皆为真值**
* eg：{}、[]、Infinity、-Infinity、new Date()等





## 防抖、节流

**防抖**(debounce)：**把多个顺序的调用合并到一起（只执行一次）**，可能是在最后一次后面执行，也可能是在第一次调用时执行。防抖在某些情况下对性能会有极大的优化

* **非立即执行版**：触发事件后函数不会立即执行，而是在 n 秒后执行，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。
* **立即执行版**：触发事件后函数会立即执行，然后 n 秒内不触发事件才能继续执行函数的效果。

**节流**(throttle)：**只允许一个函数在 X 毫秒内执行一次**，与debounce相比的区别是不会重新延时。



**防抖应用**

* **搜索框自动补全性能优化**
  * 自动补全：通过发出异步请求将当前内容作为参数传给服务器，然后服务器回传备选项。

**节流应用**

* **监听滚动事件进行图片懒加载**
* **按钮点击（点赞、取消）**



## 函数柯里化

柯里化：把**接受多个参数的函数**变换成**接受一个单一参数（最初函数的第一个参数）的函数**，并且**返回接受余下的参数而且返回结果的新函数**

函数柯里化的主要作用和特点就是**参数复用**、**提前返回**和**延迟执行**。

**Currying** **延迟求值**的特性需要用到 JavaScript 中的**作用域**来保存上一次传进来的参数。





## toPrecision() vs toFixed()

**共同点**：把**数字转成字符串**供展示使用（可以处理小数位数），且可以对多余数字做**凑整处理**

**不同点：**

- toPrecision 是**处理精度**，精度是**从左至右第一个不为0的数**开始数起。
- toFixed 是小**数点后指定位数取整**，从小数点开始数起。

> 凑整处理有Bug：1.005.toFixed(2) 返回的是 1.00 而不是 1.01
>
> 原因： 1.005 实际对应的数字是 1.00499999999999989，可用更专业的四舍五入函数 **`Math.round()`** 



## __ proto __ & getPrototypeOf()

**共同点**：都可以用于访问对象原型

**不同点**：__ proto __已经**过时**，不推荐使用；推荐通过 Object.getPrototypeOf() 来获取对象原型

> Object.setPrototypeOf(obj, proto) 
>
> 修改对象原型是**非常缓慢的操作**，破坏了对象属性访问操作的内部优化，故**尽量不要修改对象原型**



## definePropety()

Object.defineProperty()，可以在一个对象上**定义一个新属性**，或者**修改**一个对象的现有属性，并**返回这个对象。**

**语法**：

> Object.defineProperty(obj, prop, descriptor)
>
> - obj: 要在其上定义属性的**对象**
> - prop:  要定义或修改的**属性的名称**
> - descriptor: 将被定义或修改的**属性的描述符**（不可缺省，可用{} ）

**属性描述符有两种形式：数据描述符、存取描述符**，分别有四种键值

* **数据描述符**：
  * configurable：布尔值，表示是否可修改该属性的属性描述符
  * enumerable：布尔值，表示该属性是否可枚举
  * value：任意Javascript类型，默认undefined
  * writable：布尔值，表示是否可以用赋值运算符改变属性值
* **存取描述符**【定义的属性叫做**存取器属性**】
  * configurable
  * enumerable
  * get：一个getter方法，该方法的返回值作为属性值
  * set：一个setter方法，接受唯一一个参数，并将该参数的新值分配给该属性



## definePropety()

在**ES5**中只分为**全局作用域和函数作用域**，也就是说**for,if,while**等语句是不会创建作用域的。

而在**ES6**中使用let,const时有了一个**块级作用域**的概念



## for...of

* 接受迭代器，包括数组，字符串，`Set`，`Map`，DOM集合
* 接受类数组对象
* 迭代的项目可以就地解构



## for... in & for ... of【ES6】

**共同点：**遍历

**不同点：**

* **遍历内容：for...in 循环出的是**key**，for...of 循环出的是**value**
* **遍历数组时**：不建议用 **for...in** 来遍历数组，推荐使用 **for...of**
* **遍历对象自身属性时：**
  * for ... in 中会遍历**本身及其原型链上**的属性，可结合hasOwnProperty() 进行**过滤**
  * for...of 需要通过和 **Object.keys()** 搭配使用来遍历对象





## 字符串：substr、substring和slice

**共同点：**返回一个新的字符串，原字符串不变

**String.substr(N1,N2)** ：从指定的位置(N1)截取**指定长度(N2)**

* **快被废弃了，别用！**

**substring(N1,N2)**：从**两个参数中找出较小值**作为新字符串开始位置，截取字符串【长度为：较大值 - 较小值】

* 不接受负值参数

**slice(start,end)**：第一个参数为起始位置，截取到第二个参数的前一位



## hasOwnProperty() 

会返回一个布尔值，指示对象自身属性中是否具有指定的属性



## 函数声明方式

* **函数声明** 

  * ```
    function functionName(arg){}
    ```

  * **存在函数声明提升**，解析器会率先读取函数声明，使其在执行任何代码之前就可以访问

* **函数表达式**  

  * ```
    var variableName = function functionName(arg){}
    ```

  * 如果不提供functionName则表示**匿名函数**，可以用variableName来调用；如果提供了functionName，则可以**用于在函数内部使用来代指其本身（递归）**，或者**在调试器堆栈跟踪中鉴别该函数**

  * **变量提示**，但只有执行了才会读取函数内容

* **new Function**：性能不好不推荐使用



## 严格模式

类和模块的内部，默认就是严格模式，所以不需要使用`use strict`指定运行模式。只要你的代码写在类或模块之中，就只有严格模式可用。考虑到未来所有的代码，其实都是运行在模块之中，所以 ES6 实际上把整个语言升级到了严格模式



## Javascript语言特点

**解释型**的**脚本语言**：不用预编译，在程序执行的过程中**逐行进行解释执行**，运行效率偏低，开发简便

* 传统语言的编写执行顺序为"编写"→"编译"→"链接"→"执行"【C、C++】

**跨平台**：**不依赖于操作系统**，仅需要客户端浏览器的支持（现在也可以用于编写服务端程序Node.）

**动态类型：**声明一个变量之后，这个变量可以存储（指向）不同类型的变量

**弱类型：**隐式类型转换【缺陷】

**基于原型**：原型链的概念，会从原型链上继承属性



## 深拷贝、浅拷贝

> 深拷贝、浅拷贝是针对于Object、Array的拷贝而言。

**浅拷贝：**创建**新对象**：对于**基本类型**拷贝的是值；对于**引用类型**拷贝的是地址（引用类型变化会影响另一个对象）

**深拷贝**：对于引用类型，会**在堆内存中开辟一个新的空间**来存放新的对象，并完整复制**（新旧对象变化不影响）**



### 浅拷贝的实现

* **数组方法**：slice、concat
* **对象方法**：Object.assign(target, source) 拷贝的是属性值（属性值为地址则复制地址）
  * Object.assign() 拷贝**源对象自身的并且可枚举的属性**到目标对象
* **手动遍历实现**：类型判断 + 创建新对象 + 遍历源对象，给新对象添加属性



### 深拷贝的实现

* **JSON.parse( JSON.stringify() );**
  * **缺陷：**
    * 如果对象中有一个属性值为undefined，拷贝之后该属性值丢失；如果是数组，则该元素值变为null
    * 不能处理循环引用、拷贝函数等

https://juejin.im/post/5d6aa4f96fb9a06b112ad5b1#heading-1







# this指针相关

## this指针

**非箭头函数：this指针与函数的调用方式有关**

* **undefined** ：（strict：this为undefined，非strict：this为globalobject）
  - **函数调用functionName();**
  - **IEEF立即调用函数表达式(function(){})(),(function functionName(){})();**
* **对象：**
  - **方法调用**：如someObj.method() : **带someObj进去**
  - **new** functionName() 方式的调用：**创建一个新对象 newObject，带进去**
  - **functionName.call和functionName.apply：把call和apply指定thisArg带进去**

**箭头函数：this指针与函数创建时所在的词法环境有关，会继承环境上的这个this**



## call、apply、bind方法的区别

**相同点**：

* 都是**函数对象**的方法，**作用是改变函数的调用对象**，也就是函数的上下文（通过this指针指向修改实现）
* 如果不传入**第一个参数**时，默认值为undefined / 传入null。
  * 在严格环境下为undefined，在非严格环境下会用全局对象作为thisObj

**不同点**

* call和apply调用时会**直接执行**函数，而**bind调用后返回一个新的函数**，如需执行应**再一次调用**。
* **apply**以**参数数组**的形式传参；call**逐个传参**；bind也是**逐个传参**，并且可以**分两次**（调用bind+调用函数）
* **call的性能比apply好！**且可以**用ES6的拓展运算符实现apply：  `fn.call(obj, ...args)`**
* **bind函数绑定后this指针固定化（this与调用方式无关）**，永久生效，不能再用call/apply来改变this方向



## 怎么获取最大值、最小值？

* Math.max() / Math.min() 可以接受多个参数，返回最大值、最小值

* 如果是求数组中的最大值
  * Math.max.apply(Math, nums);
  * Math.max(...nums)



## call、apply、bind方法的应用场景

* 实现继承
* 获取数组中的最大值、最小值
* 将类数组转化为数组，Array.prototype.**slice.call(arrLike);** |  Array.prototype.**concat.apply([], arrLike);** 
* 数组拼接  Array.prototype.concat.call(arr, arr2) |  push 返回长度
* 判断变量类型 Object.prototype.toString.call()





# SetTimeout()、SetInterval() 相关

## SetTimeout()、SetInterval() 的区别

**共同点**

* **第一个参数**：回调函数，**第二个参数**：delay（以毫秒ms为单位，默认0）；**后面的参数用于向回调函数传参**
* 返回值：**定时器对象标识符**，可以用于**传入clearInterval 或 clearTimeout**来清除定时器

**不同点**

* setTimeout，到达一定时间就触发；setInterval，每隔一定时间就触发一次，持续触发
* **很少使用setInterval**，原因是**setInterval不一定能准时执行，有可能前一个回调函数没完成，后一个就到了**



## 为什么SetTimeout不能准时执行？

- 因为 JavaScript 是**单线程**，一定时间内只能执行一段代码；需要一个**任务队列来维护执行顺序**（同步+异步）
- delay 参数指的是，再过多长时间，就**将当前任务添加到队列**中。
- 如果**队列是空的**，那么添加的代码会**立即执行**；否则就需要等待



## SetInterval存在的问题

SetInterval 规定的是：每隔一段时间就**将回调函数的代码插入到任务队列中**（如果任务队列中已经有了就不放）

**出现的问题**：回调函数之间的时间间隙比预期要短，甚至可能会直接跳过间隔

* **原因：任务队列 && 函数运行需要时间**
* 建议用 setTimeout 代替 setInterval



## 回调函数的this指针

SetTimeout()、SetInterval()中，回调函数是**在全局作用域中**执行的，根据回调函数的类型不同，分情况考虑：

* **非箭头函数**：非严格模式下指向window对象，严格模式下是undefined
* **箭头函数**：this的值由定义时所在的词法环境决定

> 其他解决方法：变量that暂存this，call、apply、bind改变this指向



## 时间间隔为0ms的表现

实际上实际延时比设定值0ms更久，**原因：函数嵌套**，或者是已经执行的setInterval回调函数阻塞

HTML5中已经将 SetTimeout()、SetInterval() **最小执行时间**统一为4ms



## 用setTimeout模拟实现setInterval

```js
let timerController = {
  timeMap: {},
  id: 0,
  mySetInterval(cb, delay, ...args) {
    let timeId = this.id++;
    let func = () => {
      cb.call(null, ...args);
      this.timeMap[timeId] = setTimeout(func, delay);
    }
    this.timeMap[timeId] = setTimeout(func, delay);
    return timeId;
  },
  myClearInterval(id) {
    clearTimeout(this.timeMap[id]);
    delete this.timeMap[id];
  }
}

// let timer1 = timerController.mySetInterval((a, b) => {
//   console.log(a, b);
// }, 1000, 1, 2);
// let timer2 = timerController.mySetInterval((a, b) => {
//   console.log(a, b);
// }, 1000, 3, 4);
// setTimeout(() => {
//   timerController.myClearInterval(timer1)
// },4000)
```



## clearTimeout(timer) & timer = null

* clearTimeout(timer)  清除后不执行计时器回调，但是timer!=null
* timer = null 仍然会进行定时器回调



# 对象相关

## 获取对象键的相关API

* **for ... in**  遍历**自身和原型链上所有**的**可枚举**的属性
* **Object.keys()**  返回**对象自身**的所有**可枚举**属性，数组
* **Object.getOwnPropertyNames()**  返回**对象自身**的**所有属性**（可枚举+不可枚举，**无Symbol**），数组
* **Object.getOwnPropertySymbols()**  返回**对象自身**的**Symbol属性**
* **Reflect.ownKeys()**  返回**对象自身**的**所有属性**（可枚举+不可枚举，**包括Symbol**），数组
* **Object.getOwnPropertyDescriptors()** 返回**对象自身所有属性**的描述对象（**无Symbol**）



## 遍历对象自身的属性值

* **ES6：for ... of  + Object.keys()**

```js
for(let index of Object.keys(myObject)){
  console.log(index+':'+myObject[index]);
}
```

* **ES5：for ... in  + hasOwnProperty()过滤**

```js
for (let index in myObject) {
  if (myObject.hasOwnProperty(index)) {
    console.log(index+':'+myObject[index]);
  }
}
```



## 创建空对象的方法

* **Object.create(null)** 创建的对象除了自身设置的属性之外，**原型链上没有任何属性**，不继承Object的任何东西
* **var o = {a:1};** 创建的对象继承`Object`的方法，如`hasOwnProperty`、`toString`等，可以直接使用。
* **Object.create({})**  比起 {} 创建对象多了一层proto嵌套
* **Object.create(Object.prototype)** 效果等同于 {} 创建对象
* **new Object()** 效果等同于 Object.create(Object.prototype) 



## new操作符做了什么

**new操作符**可以用来创建对象实例，指的是用户定义的对象，或者是有构造函数的内置对象

```
new constructor[([arguments])]
```

1. 创建了一个**空对象** obj
2. 连接对象obj到另一个对象（**设置原型** `__proto__`），另一个对象就是构造函数的原型（prototype），建立新对象的原型链。
3. 在obj对象的执行环境下调用构造函数，此时将**this指针**指向新创建出来的对象obj，同时**执行了构造函数**。
4. 分析构造函数中的内容，如果**无返回值/返回一个非对象值**，则将**obj作为新对象**返回，否则将返回构造函数返回的对象作为new操作的返回对象



## 实现继承的方法及优缺点（ES5）

* **原型链继承**：让子类构造函数的prototype指向**父类的实例**，即可继承父类的属性（也包括函数表达式方法）
  * **缺点**
    * **父类实例中的属性会被所有实例共享**，修改父类实例引用类型属性会影响全部子类实例
      * 除非是创建新的引用类型来完整覆盖
    * 在创建子类实例的时候，**不能向父类传参**
* **借用构造函数(经典继承)**：在子类的构造函数中用call调用父类的构造函数，传入this指针作为上下文 
  * **优点**
    * 避免了父类引用类型的属性被所有实例共享
    * 在创建子类实例的时候，**可以向父类传参**
  * **缺点：** 父类方法在构造函数中定义，**每次创建实例都会创建一遍方法**（内存消耗）。
* **组合继承**：父类方法在构造函数原型上声明，父类属性定义在构造函数内，子类构造函数中调用父类构造函数
  * **优点**：融合**原型链继承和构造函数**的优点
    * 可以向父类传参、父类属性不被共享、父类方法只创建一次
  * **缺点**：**调用两次父构造函数**，结局是父类的属性出现了两次（一次是子类实例里，一次是子类实例原型）
    * 一次是创建子类对象时，一次是设置子类构造函数原型【为了可访问父类原型上的方法】
* **原型式继承**：类似Object.create的模拟实现，在函数内定义构造函数，将传入该函数的对象作为该构造函数的原型，最终返回new操作符对构造函数的作用结果。
  * **缺点**：同原型链继承的缺点相同
* **寄生式继承**：本质上是**封装了继承过程**的函数
  * 在函数内部调用Object.create()，并传入参数对象，在返回的新对象上创建新的属性、方法，最终返回它
  * **缺点**：每次创建子类实例都会创建一次子类方法【实际上也有原型链继承的缺点】
* **寄生组合式继承**：组合式继承 + 在子类实例与父类原型对象中间多加了一层【借助其它构造函数实现寄生】
  * **优点**
    * 组合继承优点
    * 避免多次调用父构造函数，父类属性在原型链上只出现一次
    * 保留正常的原型链，能够正常使用instanceof和isPrototypeOf
  * **ES6的extends**被编译后的JavaScript代码（ES6->ES5）核心是寄生组合式继承，同时添加了Object.setPrototypeOf(subClass, superClass)来继承父类的静态方法



## 从设计思想上谈谈继承本身的问题

**继承的最大问题**在于：**无法决定继承哪些属性**，所有属性都得继承。

> 为新的子类创建父类，这也是有问题的
>
> * 父类是无法描述所有子类的细节情况的，为了不同的子类特性去增加不同的父类，**代码势必会大量重复**
> * 一旦子类有所变动，父类也要进行相应的更新，**代码的耦合性太高**，维护性不好。

golang完全采用的是**面向组合**的设计方式：设计一系列零件，将这些零件进行拼装，来形成不同的实例或者类。



# 数组相关

## Javascript数组方法

* **创建数组**
  * **字面量方法** [1, 2]
  * **构造器**
  * **ES6 Array.of()**   返回由所有参数值组成的数组 【弥补构造器创建的缺陷】
  * **ES6 Arrary.from()**   将(两类对象)转为数组
* **改变原数组的方法**
  * **splice()**    添加/删除/替换数组元素
  * **pop() **  删除一个数组中的最后的一个元素
  * **push() **   向数组的末尾添加元素
  * **shift() **  删除数组的第一个元素
  * **unshift() **  向数组的头部添加元素
  * **sort() **  数组排序
  * **reserve()**   数组逆序
  * **ES6 copyWithin() **  指定位置的成员复制到其他位置
  * **ES6: fill()**   填充数组
* **不改变原数组的方法**
  * **slice()**   浅拷贝数组的元素
  * **join()**    数组转字符串，并加入分隔符（默认逗号）
  * **toLocaleString()**   数组转字符串
  * **toString()** 数组转字符串【不推荐】
  * **concat()**   合并两个或多个数组
  * **indexOf()**   查找数组是否存在某个元素，返回第一次出现的下标
  * **lastIndexOf()**   查找数组是否存在某个元素，返回最后一次出现的下标
  * **ES7 includes()**    查找数组是否包含某个元素，返回布尔值
* **遍历数组的方法**
  * **forEach**  按升序为数组中含有效值的每一项执行一次回调函数，返回值为undefined
  * **some**    数组中的是否有满足判断条件的元素
  * **every **  检测数组所有元素是否都符合判断条件
  * **filter**   过滤原始数组，返回新数组
  * **map**   对数组中的每个元素进行处理，返回新的数组
  * **reduce **  为数组提供累加器，合并为一个值
  * **reduceRight**   从右至左累加
  * **ES6 find & findIndex **  根据条件找到数组成员
  * **ES6 key & value & entries **  遍历键名、遍历键值、遍历键名+键值



## Javascript数组中的高阶函数

**高阶函数：**一个可以 **接受另一个函数作为参数** 或者 **返回值为一个函数** 的函数【其实很多遍历方法也是】

* **filter** 过滤原始数组，返回新的数组

* **map** 对数组中的每个元素进行处理，返回新的数组

* **reduce** 为数组提供累加器，对数组总每个元素做累积操作，合并为一个值



## map和forEach的区别

**相同点：**数组中的每个元素都会执行一次传入的参数函数

**不同点：**

* **参数函数返回值类型**
  * forEach传入的函数参数调用时**总是返回 undefined 值**，即使设置return了一个值。
  * Map传入的函数参数调用时**返回一个值作为新数组中对应的元素**

* **整体返回值类型**
  * forEach返回undefined
  * Map会返回一个新的数组
* **修改数组**
  * forEach中**修改原数组**需执行 arr[index] = newValue
  * Map中调用参数函数时return的新值会作为**新数组**的元素，但**原数组不改变**，除非同上执行

* **使用场景**
  * forEach适合于你**并不打算改变**数据的时候，而只是想用数据做一些事情 – 比如**存入**数据库或则**打印**出来
  * Map适用于要**改变数据值**的时候
* **执行速度**
  * Map执行速度更快



## V8引擎中Sort函数的内部实现

> 过去V8引擎sort源码：数组长度小的用插入排序，否则用快速排序
>
> 被舍弃原因：快速排序不是稳定的排序算法，在最坏情况下，时间复杂度会降级到O(n^2)。

**TimSort 稳定的 归并排序**

* 在数据量小的子数组中使用**插入排序**
* 然后再使用**类似归并排序**将有序的子数组进行合并排序
  * 归并算法是以**递归**的方式，但TimSort是以**迭代的方式**进行的
* **算法效率**
  * 最好：对于已排序好的数组，会以O（n）的时间内完成排序【只产生单个run，不需要合并】
  * 最坏：O（n log n）
  * 平均：O（n log n）



## 判断数组中是否包含某个值

* **indexOf()**  如果存在，则返回数组元素**第一次**出现时的下标，否则返回-1
* **lastIndexOf()**    如果存在，则返回数组元素**最后一次**出现时的下标，否则返回-1
* **includes(searcElement[,fromIndex])**    如果存在，返回**true**，否则返回false
* **ES6：find(callback[,thisArg])**   返回数组中满足条件的**第一个元素的值**，否则返回**undefined**
* **ES6：findIndex(callback[,thisArg])** 返回数组中满足条件的**第一个元素的下标**，否则返回-1





## 如何中断forEach循环？

> 在forEach中用**return**不会返回，函数会**继续执行**，不管怎么设置return的值，实际上都是return undefined

**中断方法：**

1. 使用**try监视**代码块，在需要中断的地方**抛出异常**。
2. **官方推荐方法**（替换方法）：**用every和some替代forEach函数**。
   - every在碰到return false的时候，中止循环。
   - some在碰到return ture的时候，中止循环



## foreach()第二个参数

foreach()第二个参数**用来绑定回调函数的this指针**



## includes & indexOf

**includes方法是为了弥补indexOf方法的缺陷而出现的:**

- indexOf方法**不能识别`NaN`**

  ```js
  let a = [NaN]
  console.log(a.indexOf(NaN)); // -1
  ```

- indexOf方法**检查是否包含某个值不够语义化，需要判断是否不等于`-1`，表达不够直观**



## 判断一个对象是否为数组的方法

* ES5 API： **Array.isArray()**
* 每个Object对象都有一个**toString**方法返回对象类型class【被重写】，故用call、apply调用
* 比较**原型链** Object.getPrototypeOf()  === Array.prototype
* obj **instanceof** Array
* 比较对象**构造函数** obj.constructor===Array

```js
1、Array.isArray(arr)
2、Object.prototype.toString.apply(b) === '[object Array]'
3、Object.getPrototypeOf(b) === Array.prototype
4、obj.constructor===Array
5、obj instanceof Array
```



## 为什么不建议用for...in来遍历数组

* for ... in 会将数组**原型链上的属性**以及**在数组上添加的属性**（arr.name='arr'）都遍历出来【可枚举】
  * 实际上for ... of也会遍历出在数组上添加的属性
* for(let index in arr) 中index索引为**字符串型数字**，不能直接进行数值运算
* 以**任意顺序迭代**对象的可枚举属性，可能不是按照实际数组的内部顺序



## 数组拍平方法

* **思路一**：**递归+concat**，每一层递归中所作的事：创建一个空数组，遍历原数组：如果该元素是数组，进入递归（会返回一个扁平化数组）；如果该元素是数组，通过concat()拼接
  * **reduce + concat + 递归**
  * **普通遍历 + concat + 递归**
* **思路二**：将数组转化为字符串，调用**split()**，用**map()** 对每个元素都做一个数值转换**parseInt()**
  * **将数组转化为字符串：**join(',')  |  toString()
* **思路三**：展开运算符可以对数组**降一维**，[].concat(...arr)，可以在循环中判断，如果数组中有数组元素，则执行一次展开运算符降维；直至数组扁平化
* **思路四**：**arr.flat(Infinite)**



## **数组去重方法**

* **思路一：**将数组作为**Set构造函数**的参数，返回一个去重的Set对象，Array.from()
* **思路二：**利用**map键名不重复**的特点去重
* **思路三：创建一个新数组**，遍历原数组，每一次都看看该元素是否在新数组中，如果不是就加进去
  * **判断元素**是否在数组中出现：indexOf() | includes() | 双重循环，内层循环遍历新数组

* **思路四：**利用**reduce**的特点，设置previousValue为空数组，之后每一次对元素执行参数函数时，判断该元素是否出现在previousValue数组中，如果不是就加进去；最终执行完reduce会返回该数组。
* **思路五：利用**filter**的特点，在每一次对元素执行参数函数时，**用indexOf判断该元素是否是第一次出现在该数组中**，如果是，就返回true；如果多次出现，则过滤该元素，最终返回的数组已去重
* **思路六**【直接操作原数组】：双重循环，内层循环用于向当前元素下标往后数，剔除多次出现。
  * **删除元素**：splice()



## 类数组

**数据结构特点**：有一个属性名为length，属性值为数字n（最终可转换出长度为n的数组）

```js
var a = {'1':'gg','2':'love','4':'meimei',length:5};
//类数组中有些属性名为数字字符串，在类数组转换时会将属性值赋给对应下标的元素，若无则undefined
```

**javascript中常见的类数组：**

* `arguments`对象
* 调用 getElementByTagName/ClassName/Name()返回的结果
* 用querySlector获得的nodeList

**类数组的特点**：类数组**不能直接使用**一些数组函数，比如说 `join()、map()、slice()`，但可以通过call、apply、bind调用来使用类类数组。

```js
var a = {'0':'a', '1':'b', '2':'c', length:3};  // An array-like object
Array.prototype.join.call(a, '+');  // => 'a+b+c'
Array.prototype.slice.call(a, 0);   // => ['a','b','c']: true array copy
Array.prototype.map.call(a, function(x) { 
    return x.toUpperCase();
});       
```

**类数组对象转换为数组对象：**

*  **Array.prototype.slice.call(arrLike)**   -----    slice浅拷贝
*  **Array.from()**
*  **ES6展开运算符**  let args = [...arguments]
*  **Array.prototype.concat.apply([], arrLike)**  // 用apply



# 数据类型相关

## Javascript数据类型有哪些？

JavaScript中的数据类型可分为以下两类：

* **原始数据类型**（7种）：**Null、Undefined、Boolean、Number、String、BigInt （ES6：Symbol）**
* **引用数据类型**：Object
  * 细分下来：**普通对象Object、Array、Function 、Date、RegExp、Math（ES6：Set、Map）**



## 基本数据类型和引用数据类型的区别

* **访问方式**

  * 基本数据类型**按值访问**，引用数据类型**按引用访问**

    > JavaScript规定**放在堆内存中的对象无法直接访问**，必须要访问这个对象在堆内存中的地址**（存放在栈内存中）**，然后再按照这个地址去获得这个对象中的值，所以引用类型的值是按引用访问

* **比较方式**

  * 基本数据类型的比较是**值的比较**，“==” 中不同数据类型比较会先进行隐式转换，再进行值的比较
  * 引用类型的比较是**引用的比较**，也就是**引用地址的比较**，即使在 “==” 中也要求两边的对象引用地址相同，即指向同一个对象

* **内存存放方式**

  * 基本类型的变量是存放在**栈内存（Stack）**里的**（栈内存中包括了变量的标识符和变量的值）**
  * 引用类型则是同时保存在**栈内存和堆内存**中
    * 堆内存中存放了对象的内容
    * 栈内存存放了变量的标识符和指向对象的指针，也就是堆内存地址

* **是否可变**

  * 基本数据类型的值是不可变的。对基本类型变量赋新值时，原值不会发生改变，但是会被其它值替换。
  * 引用数据类型的值是可变的。比如可以向对象添加新的属性/方法，或者是修改属性值
    * 存放在栈内存里面的引用地址不可变



## 基本包装类型

除了null和 undefined，其它**基本类型（String、Number、Boolean、BigInt、Symbol）**都有对应的**包装对象**

**基本包装类型**：

* **与引用类型相似，但同时也具有对应基本类型的特殊行为**
* 除了null和 undefined，其它**五种基本类型**都有对应的**包装对象**
* ES6开始，**不支持为原始数据类型显式创建包装对象**（调用new操作符），**用 new Object() 创建**
  * 如果是 new Symbol、new BigInt会报错
  * 如果是调用 new String/Number/Boolean由于遗留原因仍可被创建

**基本包装类型的表现**

* 读取基本类型值时，会**自动创建**对应的基本包装类型对象。此对象只存在代码的执行瞬间，然后立即**被销毁**
* 调用包装类型的**valueOf()**返回基本类型值
* 对基本包装类型的实例调用**typeof**会**返回“object”**
* 使用构造函数创建布尔的对象进行布尔运算时，对象都会被转换为true



## 基本包装类型与引用类型的区别

对象的**生命期**不同：

- 自动创建出来的基本包装对象的只存在于执行代码的那一行，**即执行结束后离开销毁**；
- new操作符创建的引用类型，在**执行流离开当前作用域之前一直被保存在内存中**。



## JavaScript类型检查

* 用**Object.prototype.toString** 方法检测类型，这是**唯一一个可依赖的**方式！

  * 几乎所有内置对象的原型对象都重写了toString()，所以不能够直接调用toString，应该**结合call | apply**

* **typeof** ：返回类型

  * 对于**基本数据类型**，**除了 null** 之外，都可以调用typeof显示正确的类型
  * 对于**引用数据类型**，**除了函数**之外，都会显示"object"
  * **用途**：**检测是否定义/ 赋值**（let有坑）

* **instanceof** ：用于判断左边的对象是不是右边的类实例【可用于判断引用数据类型】

  * 原理：基于原型链的查询，检测**构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上**。
  * 其语法是`object instanceof constructor` （右边需要为构造函数）



### typeof 是否能正确判断类型

* 对于**基本数据类型**，**除了 null** 之外，都可以调用typeof显示正确的类型
* 对于**引用数据类型**，**除了函数**之外，都会显示"object"



### instanceof能否判断基本数据类型

能，需要结合Symbol.hasInstance使用，重定义原有的instanceof方法

> Symbol.hasInstance用于判断某对象是否为某构造器的实例，能用来自定义 instanceof在某个类上的行为。

```js
class PrimitiveNumber {
  static [Symbol.hasInstance](x) {
    return typeof x === 'number'
  }
}
console.log(111 instanceof PrimitiveNumber) // true
```



## null是对象嘛？

`null` 是表示缺少的标识，指示**变量未指向任何对象**，也就是对象的值未设置。

```js
typeof null  //object
Object.prototype.toString.apply(null)   // [object Null]	
```

* typeof null 会输出 object
* **原因：**这是 JS 存在的一个悠久 Bug。在 JS 的最初版本中使用的是 32 位系统，为了性能考虑使用**低位存储变量的类型信息**，**000 开头代表是对象**然而 **null 表示为全零**，所以将它错误的判断为 object



## null和undefined的差异

相同点:

- 在 if判断语句中,值都默认为 false

差异:

- null转为数字类型值为0，而undefined转为数字类型为 NaN(Not a Number)
- undefined是代表调用一个值而该值却没有赋值，这时候默认则为undefined
- null是一个很特殊的对象，最为常见的一个用法就是作为参数传入(说明该参数不是对象)
- 设置为null的变量或者对象会被**内存收集器**回收



## BigInt相关

### 什么是BigInt

BigInt 可以**用任意精度表示整数**，安全地**存储和操作大整数**，即使这个数已经超出了 Number 的安全整数范围。

作用：可以用于表示**高分辨率的时间戳**，使用**大整数id**，等等，而不需要使用库。【兼容性一般】



### 为什么需要BigInt

在过去的JS中，所有的数字都是Number类型，遵循 IEEE 754标准，以双精度64位浮点格式表示。

这导致JS中的Number**无法精确表示非常大的整数**，JS中的Number类型只能安全地表示 (-(2^53-1))和（(2^53-1)），任何超出此范围的整数值都可能**失去精度**。

如果遇见了非常大的整数，会对它做**四舍五入**的处理，就会产生一些奇怪的问题：

```
9007199254740992 === 9007199254740993;    // → true 居然是true!
```



### 如何创建并使用BigInt

* **在数字末尾追加n**             9007199254740992n !== 9007199254740993n
* **BigInt()中传入字符串**      BigInt("9007199254740992") !== BigInt("9007199254740993")



### BigInt注意事项

- BigInt不支持一元加号运算符
- 不允许在 BigInt 和 Number 之间进行混合操作，因为隐式类型转换可能丢失信息
- 不能将BigInt传递给Web api和内置的 JS 函数，这些函数需要一个 Number 类型的数字
- 当 Boolean 类型与 BigInt 类型相遇时，只要不是0n，就是true
- 元素都为BigInt的数组可以进行sort
- BigInt可以正常地进行位运算，如|、&、<<、>>和^



##  JS中类型转换有哪几种

- 转换成数字
- 转换成布尔值
- 转换成字符串

> 注意"Boolean 转字符串"这行结果指的是 true 转字符串的例子

![img](https://user-gold-cdn.xitu.io/2019/10/20/16de9512eaf1158a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 如何看待 JavaScript是一门弱类型语言

JavaScript是一门**弱类型语言**：**表达式运算赋值等操作都会导致隐式的类型转换**

强类型语言和弱类型语言的区别，就在于**计算时不同类型之间是否可以对使用者透明地隐式转换**。

* 从**使用者的角度**来看，如果一个语言可以**隐式转换**它的所有类型，那么它的变量、表达式等在参与运算时，**即使类型不正确，也能通过隐式转换来得到正确地类型**，这对使用者而言，就好像所有类型都能进行所有运算一样，所以这样的语言被称作弱类型。
* 与此相对，强类型语言的类型之间不一定有隐式转换。



## “==” 和 “===”相关

### “==” 和 “===” 的区别

> JavaScript是一门**弱类型语言**：**表达式运算赋值等操作都会导致隐式的类型转换**

**"==" 相等运算符 ：** **值相等**则为真。如果两边类型不同，会进行**隐式类型转换**之后再作比较

**"===" 严格运算符** ：要求**值相等**，而且也要求**类型相同**。 不做类型转换，类型不同的结果一定不等

**编程建议**：**尽量使用严格运算符 ===**。

**原因：**因为"=="不严谨，在比较时会进行**隐式类型转换**，可能带来一些违反直觉的后果。



### 严格运算符 === 的运算规则

* **不同类型值**：直接返回false

* **原始数据类型之间**（number、string、boolean）进行**值比较**

* **引用数据类型之间**（Object、Array、Function）进行**地址比较**
* **undefined 和 null** 与自身严格相等，null === null  undefined === undefined  //true
* **NaN** 与自身不严格相等      NaN=== NaN  //false
* ！！ +0 严格等于 -0



### 相等运算符 == 的运算规则

* **两边类型相同**则进行值比较：基本数据类型按值比较，引用数据按址比较
* **原始数据类型之间**（number、string、boolean）隐式类型转换为**数值类型**，再做比较
* **对象**与**数字或字符串**相比较，会尝试通过方法**valueOf和toString**将对象转换为其**原始值**（字符串或数字类型的值）。如果尝试转换失败，会产生一个运行时错误。 
* **对象**与**布尔型**比较时，**布尔型先转化为数值**。
* 当**两个操作数均为对象**时，按地址比较，引用相同对象则为true
* **undefined和null** 与其他类型的值比较时，结果都为**false**，相互比较为**true**
* **NaN** 与自身不相等      NaN == NaN  //false



##  对象转原始类型是根据什么流程运行的

对象转原始类型，会调用内置的[ToPrimitive]函数，对于该函数而言，其逻辑如下：

1. 如果**Symbol.toPrimitive()**方法，优先调用再返回
2. 调用**valueOf()**，如果转换为原始类型，则返回
3. 调用**toString()**，如果转换为原始类型，则返回
4. 如果都没有返回原始类型，会**报错**

> Symbol.toPrimitive 是一个内置的 Symbol 值，它是**作为对象的函数值属性存在**的





# 概念解析相关

## 闭包相关

### 闭包是什么？

闭包：指有权**访问另外一个函数作用域中的变量**的函数。

在**一个函数里面定义函数**，这个**内部函数**就是一个闭包（从内部函数访问到外部函数的作用域）

> 在函数里创建函数，内部函数就会将它**创建时所在的词法环境**保留下来，存在自己的[[scope]]里，然后我们**将闭包作为外部函数的返回值返回出去**，用一个全局变量接受，因为存在**引用关系**，就可以使得**原本的外部函数在执行后不被销毁，可以访问其中的变量**，这就是**闭包的作用**



### 为什么会产生闭包/作用域链是什么？

ES5中只存在**两种作用域**————**全局作用域和函数作用域**

当访问一个变量时，解释器会首先在当前**作用域**查找标示符，如果没有找到，就去**父作用域**找，**直到找到该变量的标示符或者不在父作用域中**，这就是**作用域链**。

**每一个子函数，也就是嵌套函数，都会拷贝上级的作用域**，形成一个作用域的链条。

> * 执行栈
> * 运行上下文：两个词法环境（一个解析引用，一个变量值），一个thisBinding
> * 词法环境（登记本）：环境记录，outer指针
>   * outer指针会指向函数创建时的运行上下文词法环境
> * [[scope]]：在函数里创建函数，内部函数就会将它创建时所在的词法环境保留下来，存在自己的[[scope]]里





### 为什么需要闭包/闭包的作用

* **JavaScript的链式作用域**：通过outer链接的词法作用域，所以它的一个特点就是，可以**沿着作用域链往上找**。即函数内部可以直接访问外部变量，但在外部无法访问函数内部变量，闭包解决了这个问题
* **解决循环时变量泄露问题 var**
* Javascript中没有**私有变量，私有方法**的说法，可以用闭包来模拟，把想要让外界访问的东西开放出去
* 避免**全局变量污染**



### 闭包的缺点

* **常驻内存，增加内存使用量**；----> 在退出函数之前，将不使用的局部变量全部删除

* 使用不当造成**内存泄漏**（循环引用）



### 闭包有哪些表现形式

* 在函数A内创建一个函数B，将函数B **return**出去，用一个变量接收

* 在函数A内创建一个函数B，在函数A内调用外面的函数C，并将函数B作为**参数传递**过去

  

## 原型&原型链

**原型：**每一个构造函数都有一个prototype属性，这个prototype属性会指向一个对象，这个对象指的就是构造函数的原型。

**原型链：**每个对象都有一个proto属性，这个属性指向该对象的构造函数prototype属性对应的原型。**实例对象与原型**之间的连接就是原型链。

* 作为一个对象，当你**访问其中的一个属性或方法**的时候，如果这个对象中没有这个方法或属性，就会沿着原型链向上找，直到找不到为止
* `Object.prototype.__proto__ === null` 构造函数Object()对应原型的原型为空null，而null没有原型，也就是**原型链的最后一个环节**



# 事件相关

## 事件绑定的方式

- **内联属性**，onclick="alert(1); " 
- 使用 **元素.on事件**
- 使用**事件监听addEventListener** 【可以给同一个事件绑定多个回调函数，按绑定顺序执行】

* **IE6、7、8中** ：**attachEvent()和detachEvent()**





## mouseover和mouseenter有什么区别？

二者的区别是**mouseenter不会冒泡**。

* 当二者绑定的元素**都没有子元素**时，二者的行为是一致的。
* 当二者内部**都包含子元素**时，行为就不一样了。在mouseover绑定的元素中，鼠标每次进入一个子元素就会触发一次mouseover事件，而mouseenter只会触发一次。





## target和currentTarget的区别？

* currentTarget指向事件绑定的元素【在该元素上进行事件监听】

* target 则是触发事件的元素。【从target处开始冒泡】

> **事件委托**：通过事件冒泡（或捕获）给父元素添加事件监听，达到给每一个子元素添加监听事件的效果
>
> 由e.target指向引发触发事件的元素，而e.currentTarget则为挂载监听器的父元素



## 事件处理阶段

事件处理其实有3个阶段：**事件捕获**阶段、**事件目标**阶段**事件冒泡**阶段

**事件捕获**：先从根节点向下执行捕获回调函数，一直到实际触发事件的元素的父元素位置，**捕获过程**结束；

* eg：如果父元素通过事件捕获方式注册对应事件的话，会先触发

**事件目标**：然后事件传播到实际触发事件的DOM元素上，为**事件目标**阶段

* 事件目标阶段，无论**addEventListener函数第三个参数是true还是false**，绑定的回调函数都会被执行

**事件冒泡**：之后**开始冒泡**，从**触发事件的DOM节点**开始，向上冒泡一直到根节点，触发对应的冒泡注册事件

> 事件的执行顺序按事件回调函数的添加顺序执行

![img](https://user-gold-cdn.xitu.io/2019/4/16/16a2654b0dd928ef?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 如何阻止事件冒泡?

* **原生：**

  * **非IE浏览器**：调用当前事件对象的stopPropagation()方法【**阻止捕获和冒泡**中事件的进一步传播】
  * **IE浏览器**：

* Vue：事件修饰符stop  eg：@click.stop

  

## 如何阻止默认事件?

* **原生**
  * **非IE浏览器**：调用当前事件对象的preventDefault()方法   
  * **IE浏览器**：window.event.returnValue = false; 
* **Vue** ：事件修饰符prevent  eg：@click.prevent



## 事件委托

事件委托是指**利用“事件冒泡”**，只通过指定一个事件处理程序，来管理某一类型的所有事件。

也就是说，当此事件处理程序被触发时，**通过当前事件对象中的target来确认究竟是在哪个元素触发的事件**，从而达到**一次注册，处理多个元素触发事件**的目的。

**优点**

* **减少事件注册，减少内存消耗**：table可以代理所有td的click事件
* **动态绑定事件**：如果元素是动态变化的，那么传统事件监听方式在**增删**元素时都**需要手动进行**绑定、解绑。

**缺点**：

* **基于冒泡**，不支持不能冒泡的事件，eg：blur、focus

* **层级过多，冒泡过程中，可能会被某层阻止掉（建议就近委托）**

  

## 不支持冒泡的事件

- UI事件：load、unload、scroll、resize
- 焦点事件：blur、focus
  - 当元素失去焦点时发生 blur 事件
- 鼠标事件：mouseleave、mouseenter



## 如何让点击事件只触发一次

* 在设置事件监听器时设置第三个**选项对象的once属性**

  ```js
  document.getElementById('btn0')
          .addEventListener("click", function(event) {
              doSomething('hello world');
          }, {once: true});
  ```

* 在执行完事件处理函数之后调用**removeEventListener**移除事件处理函数

* Vue中的通过**修饰符`once`**实现只触发一次事件处理程序的方式

  ```vue
  <button v-on:click.once="doThis"></button>
  ```




## addEventListener第三个参数

```
domElement.addEventListener("click", function(){}, useCapture);
```

如果是boolean值：**指定事件处理函数的时期或阶段(boolean)**：true：捕获阶段执行，false**：**冒泡（默认值）

如果是对象值：表示事件相关的选项对象，比如说：once属性，事件只触发一次



# ES6相关

## ES6 模块与 CommonJS 模块的差异

- CommonJS 模块输出的是一个**值的拷贝**，ES6 模块输出的是**值的引用**。
- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。



## Set & Map

> Set 和 Map 主要的应用场景在于 **数据重组**（去重） 和 **数据储存**，底层hash

- **Set 集合**
  - **成员唯一、无序且不重复**
  - [value, value]，键值与键名是一致的（或者说**只有键值，没有键名**）
  - **可以遍历**，方法有：add、delete、has
- **Map** **字典**
  - 本质上是**键值对**的集合，类似集合
  - **可以遍历**，方法很多，可以跟各种**数据格式转换**
- WeakSet
  - **成员都是对象**
  - 成员都是**弱引用**，可以被垃圾回收机制回收，**可以用来保存DOM节点，不容易造成内存泄漏**
  - **不能遍历**，方法有add、delete、has
- WeakMap
  - **只接受对象作为键名**（null除外），不接受其他类型的值作为键名
  - **键名是弱引用**，键值可以是任意的，**键名所指向的对象可以被垃圾回收**，此时键名是无效的
  - **不能遍历**，方法有get、set、has、delete



## 箭头函数和普通函数的区别

* **this相关**
  * **箭头函数没有自己的this**，其内部的`this`就是箭头函数在**定义时**所处**外层执行环境的this**（从自己的**作用域链的上一层继承**）
  * **普通函数**的this指针则与他的**调用方式有关**
  * 箭头函数中**`this`指向固定化**，**不能用`call()`、`apply()`、`bind()`这些方法去改变`this`**
* **箭头函数不可以当构造函数**，即**不可以使用`new`命令**
* 箭头函数**没有原型prototype**除了`this`，**变量`arguments`、`super`、`new.target`**在箭头函数之中也是不存在的，**指向外层函数的对应变量**。如果要用arguments，**可以用Rest参数（一个数组）代替。**   （...rest)=>{console.log(rest)}
* 箭头函数不可以使用`yield`命令，因此箭头函数**不能用作Generator函数**（生成器）
* 箭头函数相较于普通函数，**语法简洁、清晰**，常用于简化回调函数



## let、const、var的区别

* **作用域**：`let`、`const`所声明的变量，只在**`let`、`const`命令所在的代码块{}内有效**，而var不是
* **变量提升**：`var`命令会发生**“变量提升”现象**：**变量可以在声明之前使用，值为`undefined`**，而`let`、`const`命令规定**声明的变量一定要在声明后使用，否则报错**。
  * 本质：在词法环境中var会提前登记
* **暂时性死区**：**在代码块内**，使用`let`、`const`命令声明变量之前，该变量都是**不可用**的。
  * **本质**上，只要一进入当前作用域，所要使用的变量就**已经存在**了，但是**不可获取**，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。
  * 意味着`typeof`不再是一个百分之百安全的操作，可能会引起报错（在let之前用typeof）
* **不允许重复声明**：`let、const`不允许在相同作用域内，重复声明同一个变量，而var会覆盖掉之前存在的同名属性，出现全局变量污染
* **全局对象的属性**：在浏览器中，使用`var`声明的全局函数和变量会成为**全局对象的属性**（可以用window.foo访问），而 `let、const` 不会。
  * var的这种行为是出于兼容性而存在的，不能去依赖它。（模块化避免了这种情况）



## Object.is和 === 的区别

**Object.is()** ：用来**比较两个值是否严格相等**【ES6新增】，与`===`的行为基本一致，修复了一些**特殊情况下的失误**

* **+0**不等于**-0**
* **NaN**等于自身。



## proxy

可以拦截13种操作，包括：对象属性的读取、设置；propKey in proxy；delete proxy[propKey]；读取对象属性集合；读取属性的描述对象getOwnPropertyDescriptor；获取对象的原型getPrototypeOf；拦截 `Proxy` 实例作为函数调用的操作apply:

- **get(target, propKey, receiver)**：拦截**对象属性的读取**，       通过. / [] 都行
- **set(target, propKey, value, receiver)**：拦截**对象属性的设置** ，返回一个布尔值。
- **has(target, propKey)**：拦截 propKey in proxy 的操作，返回一个布尔值。
- **deleteProperty(target, propKey)**：拦截 delete proxy[propKey] 的操作，返回一个布尔值。
- **ownKeys(target)**：拦截读取对象属性集合的操作，返回目标对象所有自身的属性的属性名【数组】
  * Object.getOwnPropertyNames(proxy) 、 Object.getOwnPropertySymbols(proxy) 、Object.keys(proxy) 、for...in 循环
  * 而 Object.keys() 的返回结果仅包括目标对象自身的可遍历属性。
- **getOwnPropertyDescriptor(target, propKey)**：拦截 Object.getOwnPropertyDescriptor(proxy, propKey) ，返回属性的描述对象。
- **defineProperty(target, propKey, propDesc)**：拦截 `Object.defineProperty(proxy, propKey, propDesc）` 、`Object.defineProperties(proxy, propDescs)` ，返回一个布尔值。
- **preventExtensions(target)**：拦截 `Object.preventExtensions(proxy)` ，返回一个布尔值。
- **getPrototypeOf(target)**：拦截 `Object.getPrototypeOf(proxy)` ，返回一个对象。
- **isExtensible(target)**：拦截 `Object.isExtensible(proxy)` ，返回一个布尔值。
- **setPrototypeOf(target, proto)**：拦截 `Object.setPrototypeOf(proxy, proto)` ，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
- **apply(target, object, args)**：拦截 `Proxy` 实例作为函数调用的操作，比如 `proxy(...args)`、`proxy.call(object, ...args)` 、`proxy.apply(...)` 。
- **construct(target, args)**：拦截 `Proxy` 实例作为构造函数调用的操作，比如 `new proxy(...args)` 。



## Proxy & defineProperty()

* **作用范围**

  * Object.defineProperty 只能通过**属性标识符中的set、get**键来**重定义**属性的**读取和设置**行为
  * Proxy，可以**重定义更多的行为**（13种），由Proxy来“**代理**”某些操作，比如 in、delete、函数调用等

* **返回值**

  * 执行Object.defineProperty 后会返回**新增/修改属性后的对象**
  * 执行new Proxy()后会返回一个**Proxy实例**，通过**类型检测**可以发现也是一个**object对象**，并且这个对象上的操作会**被handler拦截处理**。

* **数据监控谁**

  * **（监听属性）**执行Object.defineProperty后，修改原对象**对应属性**，就会被监控到
  * **（监听对象）**Proxy中，操作的是new Proxy返回的对象，只有在**返回值对象上**的操作才会被拦截

* **是否需要遍历**

  * 如果要监控某一对象的所有属性，则**需要进行遍历**，对每个属性都执行Object.defineProperty
  * Proxy是**直接挟持整个对象**，只要对象某一个属性修改了，就可以都监听到

* **新属性**

  * 所以新增属性时，需要**重新遍历对象**，对其新增属性再使用 **Object.defineProperty** 进行劫持

  * Proxy 通过 set 拦截对象属性的设置，是**可以拦截到对象的新增属性**的。



## Object.assign

**作用**：将源对象中所有**可枚举**属性的**值复制**到目标对象，**返回目标对象**

**语法**：Object.assign(target, ...sources)

**注意事项**

* 如果目标对象中的属性具有相同的键，则属性将被源对象中的属性**覆盖**
* `Object.assign()`拷贝的是属性值，是一种**浅拷贝**。



## Class类

* 如果某个方法之前加上星号（`*`），就表示该方法是一个 Generator 函数，也就是该类的默认遍历器 for of



#### constructor 构造函数

使用 new 操作符时会自动执行constructor，返回一个对象实例

#### 静态方法

在一个方法前加上`static`关键字，就表示该方法**不会被实例继承**，而是**直接通过类来调用**，这就称为“静态方法”

* 如果在**实例上调用静态方法**，会抛出一个**错误**，表示不存在该方法。
* 只能**通过类名去调用**，如 Foo.bar()
* 父类的静态方法，可以被子类继承，可直接在子类中调用，也可以从`super`对象上调用。

#### 静态属性

静态属性指的是 Class 本身的属性，即`Class.propName`，

#### 实例方法

* 定义时前面不需要加上`function`这个关键字，直接把函数定义放进去了就可以了。
* 另外，方法之间不需要逗号分隔，加了会报错。
* **类的所有方法都定义在类的`prototype`属性上面**。

#### 实例属性

实例属性可以定义在**`constructor()`方法里面的`this`上面**，也可以定义在**类的最顶层（不需要加this）**。

* 定义在this上的是实例上的属性
* 没有用this定义的属性在原型上

#### 私有方法

* 将私有方法/私有属性写到类外面，在类里面用一个公有方法通过call去调用私有方法
* 利用`Symbol`值的唯一性，将私有方法的名字命名为一个`Symbol`值，在公有方法里去调用它【属性表达式】
  * 也不是完全找不到，比如说 **Reflect.ownKeys()**

#### 私有属性

* 在属性名之前，使用`#`表示，eg：#count = 0 【也可以定义私有方法】
* 可以设置getter、setter方法  get #x()

#### new.target 属性

函数内部用，如果构造函数不是通**过new命令或Reflect.construct()**调用的，new.target会**返回undefined**

* 可以写出不能独立使用、必须继承后才能使用的类（不能被实例化，只能用于继承）
* 函数外部会报错



#### 为什么ES6中引入了Class

因为ES5中的继承方法**与其他传统面向对象语言（C++、Java）差异较大**。故提供了更接近传统语言的写法，引入了 Class的概念，作为对象的模板。

本质上只是一个**语法糖**，使得对象原型的写法更加清晰、更像面向对象编程的语法

typeof + 类 === ‘function’，也就是说类的数据类型就是函数，类本身就指向构造函数。



## 继承

### ES6继承简述

* ES6的继承是通过**class**丶**extends** 关键字来实现的

* constructor 方法是**类的构造函数**，在ES6里面，通过 **new 命令创建对象实例时，会自动调用该方法**。在实现继承时，需要在子类的 constructor 里通过 **super()** 去调用父类的constructor。可以理解为是**在子类（传入子类实例this）上借助父类的构造函数来添加父类上的属性**实现父类**实例属性的继承**，执行完super()之后，可以在constructor()里对this进行进一步加工，添加子类的实例属性。

* 在constructor中super指的是**父类的构造函数**；而如果把super**当作对象使用**时：

  * **在子类的普通方法**中，super指向父类的**原型对象**；因为父类的实例方法保存在父类构造函数的原型上，所以可以在子类的普通方法中通过super去调用父类的**实例方法**

  * **在子类的静态方法（static）**中，super指向**父类**；因为在ES6中类的静态属性、方法是保存在类上的；所以可以在子类的静态方法中通过super去调用父类的**静态属性、方法**

  * > 加上static，表示该方法不会被实例继承，而可以直接通过类来调用

  

### ES6继承的本质：两条继承链

作为一个**构造函数**，子类（`B`）的原型对象（`prototype`属性）是**父类的原型对象（`prototype`属性）的实例**。

作为一个**对象**，子类（`B`）的原型（`__proto__`属性）是父类（`A`）；

```js
// B 的原型继承 A 的原型
Object.setPrototypeOf(B.prototype, A.prototype);

// B 继承 A 的静态属性
Object.setPrototypeOf(B, A);
```



### ES6中与ES5继承的不同点

* **调用方法**：
  * ES6 的类 类似于构造函数，但**ES6中类必须使用`new`调用**，否则会**报错**。
  * ES5 中 而普通的构造函数**不用`new`也可以执行**。
* **实例方法是否可枚举**：**ES6**中Class的**实例方法**和**ES5**中的寄生组合式继承**都定义在构造函数的Prototype上**
  * ES6中Class中定义的实例方法是**不可枚举**的
  * ES5中在构造函数原型上定义的实例方法是**可枚举**的
* **严格模式**：ES6中类的内部默认就是**严格模式**，不需要使用 use strict【也只能用严格模式】
* **是否存在变量提升**
  * ES6中：类**不存在变量提升**，不会把类的声明提升到代码头部，原因是**保证子类在父类之后定义**
  * ES5中具体分析



* **ES5** 的继承，实质是**先创造子类的实例对象`this`**，然后再将父类的方法添加到`this`上

* **ES6** 的继承，实质上是**先将父类实例对象的属性和方法，加到`this`上面**，然后再用子类的构造函数修改`this`。【子类中只有调用`super`之后，才可以使用`this`关键字】

  

#### Object.getPrototypeOf()

用来从子类上获取父类。可以使用这个方法判断，一个类是否继承了另一个类。



# ES6 系列之 Promise

## Promise简介

Promise 是一个对象，**代表一个异步操作**，有三种状态：**pending、fulfilled、rejected**

Promise构造函数**接受一个函数作为参数**，该函数的两个参数分别是resolve和reject。在函数内部可以放入一些**异步操作的代码**，当异步操作结束时，**调用resolve | reject**，并且传入相关数据，此时就会自动去回调对应的 **then代码块 | catch代码块**。



## Promise的优缺点

* **优点** | **作用**
  * **异步封装：**Promise对象提供**统一的接口**，使得**控制异步操作更加容易**。可以用于**封装异步请求**，return一个Promise，这样子就可以在**调用**时设置对应的回调函数 then | catch
  * **链式调用**：Promise的then、catch代码块返回一个Promise对象，也就是说我们可以在Promise上连接多个then，并且**前一个then完成后会将结果作为参数传入下一个then**，这使得我们**可以将异步操作以同步操作的流程表达出来，**避免了**回调地狱**

* **缺点**：
  * 对比起async/await，**仍然需要传递一个回调函数给 then**，有时候不太方便

  * 一旦**创建**Promise就会**立即执行**，**无法中途取消**。

  * **必须通过catch来捕捉错误**，否则Promise内部抛出的**错误不会反应到外部。**

  * 当处于**pending状态**时，无法得知目前进展到哪一个阶段



## Promise相关方法

* **Promise.prototype.then()**：返回Promise，支持链式调用，返回值传递
* **Promise.prototype.catch()**：返回Promise，支持链式调用，返回值传递；错误会冒泡**直到被catch捕获**
* **Promise.prototype.finally()**  不论状态如何都会执行的操作，支持隔代传递返回值

* **Promise.all()**  返回一个Promise，等 **所有成功 / 一个失败** （every）
* **Promise.race()**   返回一个Promise，等 **最先改变**
* **Promise.allSettled()**   返回一个Promise，等 **所有完成**，执行**then方法**，传入状态数组
* **Promise.any()**   返回一个Promise，等 **一个成功，否则失败**

- **Promise.resolve()**：将现有对象**转为 Promise 对象**，传入的参数则为该Promise实例 **then** 时传入的参数
  - 参数类型：Promise实例(直接返回) | thenable对象(执行then方法) |  无参数  |  其他原始值
- **Promise.reject()**：将现有对象**转为 Promise 对象**，传入的参数则为该Promise实例 **catch** 时传入的参数

* **Promise.try()**：**不论函数`f`是同步函数还是异步操作**，都想**用 Promise 来处理它**；并且实现**同步函数同步执行，异步函数异步执行**，让他们具有统一的API，并且可以更好的管理异常【好像还不能用】



## catch的本质

```js
// then的语法糖
Promise.prototype.catch = function(fn){
    return this.then(null,fn);
}
```



## Promise中的then第二个参数和catch有什么区别

* **在捕获错误时有就近原则**。如果promise内部报错，then的第二个参数存在则先捕获，不存在则由catch捕获

* 如果在**then的第一个函数里抛出了异常**，后面的catch能捕获到，而then的第二个函数捕获不到

* catch**更接近同步的写法**

  

## 为什么不建议在then中传第二个参数，而更建议使用catch？

* 如果在**then的第一个函数里抛出了异常**，后面的catch能捕获到，而then的第二个函数捕获不到
* catch**更接近同步的写法**



## Promise可以进行链式写法的原因

Promise.prototype.then**()** 的返回值是一个新的Promise实例，前一个then方法的返回值作为下一个then的参数



## Promise抛出错误与传统try/catch代码块的差别

* Promise 对象抛出的**错误不会传递到外层代码**，**只能被catch方法捕获**。如果没有使用catch方法，就相当于Promise会将错误吃掉【也就是说同步try...catch在异步代码上不起作用】

  * Promise报错**不会退出进程，终止脚本运行**（！）
  * 如果该脚本**放在服务器执行**，退出码就是`0`（即表示执行成功）。

* Promise**调用rejected()**，后面的代码**还会继续执行**



# ES6 系列之 Generator 

## Generator函数简介

Generator函数内封装了**多种内部状态**，执行Generator函数会**返回一个遍历器对象**，可用于**遍历内部状态**。

Generator最大的特点就是可以**暂停执行、恢复执行**：**yield表达式**是暂停执行标记；**next方法**可以恢复执行。

* 根据这一特点，可以借助 Generator函数来**处理异步操作**：
  * 把异步操作写在**`yield`表达式中**，将异步操作的后续操作可以放在**`yield`表达式下面**；这样子执行到异步操作时Generator函数暂停，在**异步操作完成**时**调用`next`方法，让Generator函数重新获得执行权，再往后执行**，就可以**用看上去是同步的代码来进行异步编程**
  * **前提：**需要**编写代码手动执行**或者**使用自动执行器**（Thunk函数 | co模块）



## Generator函数的应用

* 处理异步操作，**同步化表达，用统一的try...catch**处理错误****

* **在对象上部署 Iterator 接口**

  

## Generator与普通函数的区别 | 特点

* **表现上**：**星号**、用**yield表达式**定义**内部状态** function* g(){}
* Generator 可以**暂停执行**：**yield表达式**暂停执行、**next方法**恢复执行
* 执行Generator函数**返回一个遍历器对象**，故也**不能够用作构造函数**
* **错误处理机制**：可以在函数内捕获函数体外用throw方法抛出的错误



## Generator函数**在异步编程上的优缺点**

* **优点**
  - 异步操作相关逻辑都被**封装在一个函数**，看上去就像同步一样，按部就班非常清晰
  - 在**解决回调地狱的问题上**，语义比Promise清晰很多（Promise会也很多语法，荣誉）
  - 也可以**处理并发的异步操作**，将需要并发的操作都放在数组或对象里面，跟在`yield`语句后面。
  - **多个`yield`表达式**，可以**只用一个`try...catch`代码块来捕获错误**
* **缺点**：需要**手动编写执行器**或者是**使用自动执行器**【自动执行器的两种实现：**Thunk函数 / co函数**】



## yield表达式 & return语句

**相同点：**都能返回语句后面的表达式的值

**不同点**

* yield表达式有**位置记忆功能**：遇到 yield 函数暂停执行，下一次再从该位置向后执行，而return语句无
* Generator函数内可以**执行多个yield表达式**，但只能**执行一次return**
* 如果**用for...of遍历的话不会遍历出return语句后面的值**（因为此时done为true）



## next()、throw()、return()

**next()：**除了第一次调用之外，携带的参数会被当作**上一个`yield`表达式的返回值**

**throw()**：抛出一个错误（**函数体外抛出，函数体内可捕获**），并且捕获后回顺带自动执行一次next()

**return()：**返回给定的值，并且**终结遍历**，**返回值的`done`属性为`true`**

* **共同点**：**让 Generator 函数恢复执行**



## yield* 表达式的作用

**yield*表达式**：后面加**一个遍历器对象**，用来**在Generator 函数里面执行另一个 Generator 函数**

* 在yield表达式，效果等同于**将该遍历器对象中的多个yield表达式for...of插入**
* **递归**：数组拍平



## 为什么Generator 函数能封装异步任务

- **暂停执行和恢复执行**：能等待异步操作完成之后再继续执行，不阻塞**【根本原因】**
- **函数体内外的数据交换：**next返回值的value属性（向外输出）；next方法还可以接受参数（向内输入）
- **错误处理机制：**可以在**函数内部**部署错误处理代码，**捕获函数体外throw()方法抛出的错误**
- 当**异步操作的结果**是一个Promise时，就可以**在next()方法返回值的value属性上设置对应的then代码块**，并且用then继续推进Generator函数的执行



## Generator的自动执行策略

### Thunk 函数

JavaScript 语言中的Thunk是指，将**多参数函数**替换成一个**只接受回调函数作为参数**的**单参数函数**（这个单参数版本，用于接收回调参数，就叫做 Thunk 函数）

---

**Thunk 函数**能作为 Generator 函数的**异步操作**执行器

**思想：**Generator函数要想执行异步操作，关键是需要**在异步操作完成时，继续调用next方法**。那么我们就可以将异步任务转化为Thunk函数（接收一个回调函数参数），并**将该Thunk函数作为yield表达式的返回值**，这样子就能**调用next()返回值的value**并传入对应的回调函数形式，在回调函数中继续调用next方法。

【Thunk函数所起到的作用：**在回调函数中将执行权交还**】

即，在yield中返回Thunk函数，**将回调函数反复传入`next`方法的`value`属性**，异步任务完成时就会回调它，就可以**递归的自动完成流程管理**

```js
//基于Thunk函数的Generator执行器
function run(func) {
  var gen = fn();

  function next(err, data) {
    var result = gen.next(data);
    if (result.done) return;
    result.value(next);
  }

  next();  // Thunk 的回调函数
}

function* g() { }

run(g);
```

> Thunk函数原本指的是**传名调用**，在函数传递参数时，**将参数包裹在Thunk函数中**，用到时再求值



### co模块

co模块用于 Generator 函数的自动执行。

**使用方法：**

* 引入co模块，调用co函数并传入Generator函数即可。
* `co`函数返回一个`Promise`对象，因此可以用`then`方法添加回调函数，在 Generator 函数执行结束时执行
* co **支持并发的异步操作**，将需要并发的操作都放在数组或对象里面，跟在`yield`语句后面。
  * 也就是说**允许某些操作同时进行，等到它们全部完成，才进行下一步**。

---

使用 co 的**前提条件**：Generator 函数的**`yield`命令后面，只能是 Thunk 函数或 Promise 对象**。

* 如果数组或对象的成员，全部都是 Promise 对象，也可以使用 co



#### 为什么 co 可以自动执行 Generator 函数？（原理）

**Generator 自动执行的条件就是，当异步操作有结果时，能够获取执行权，自动继续执行**

**两种实现方法：**

- Thunk 函数。将异步操作包装成 Thunk 函数，**在回调函数里**面交回执行权。
- Promise 对象。将异步操作包装成 Promise 对象，**用`then`方法**交回执行权。

co 模块的本质就是**将这两种自动执行器（Thunk 函数和 Promise 对象），包装成一个模块**。



## Generator实现状态机及其优点

**优点**：无需使用外部遍历保存状态，**更简洁，更安全**（状态不会被非法篡改）、**更符合函数式编程**思想

> **本身就包含了一个状态信息**，即目前是否处于暂停态。

```js
// Generator函数的状态机实现
var clock = function* () {
  while (true) {
    console.log('Tick!');
    yield;
    console.log('Tock!');
    yield;
  }
};
```



## Generator 与 协程

协程：指的是**多个线程互相协作**（维护**多个执行栈**），完成异步任务，在同一时间**只有一个执行栈获得执行权**

**多线程**下，**多个线程并行执行**，只有**一个线程正在运行** | **单线程**下，**多个函数并行执行**，只有**一个函数正在运行**

> 引入协程以后**每个任务可以保持自己的调用栈**。**最大的好处**就是**抛出错误时，可以找到原始的调用栈**。
>
> 不至于像异步操作的**回调函数**那样，**一旦出错，原始的调用栈可能早就结束。**

### 协程与子例程的差异

* **程序运行表现**
  * 传统的“子例程”：**堆栈式“后进先出”**，只有**当调用的子函数完全执行完毕，才会结束执行父函数。**
  * 协程：**并行执行，只有一个在运行，暂停执行时交换执行权**
* **实现方式（内存）**
  * 子例程：只使用一个栈
  * 协程：同时存在**多个栈**，但只有一个栈是在运行状态【**以多占用内存为代价**，实现多任务的并行】

### 协程与普通线程的差异

- **共同点：**都有自己的**执行上下文**、可以**分享全局变量**。
- **不同点**
  - 同一时间可以有**多个协程处于运行状态**，但是**运行的协程只能有一个**，其他协程都处于暂停状态。
  - **普通线程**之间是**抢先式**的，**由运行环境决定**哪个线程得到资源；协程是**合作式的**，**执行权由协程自己分配**



## Generator 与 上下文

- **普通情况下的上下文：堆栈，后进先出**。最后产生的上下文环境首先执行完成，退出堆栈，继续执行其它
- **Generator 函数的上下文环境**：一旦遇到`yield`命令，Generator 函数的上下文环境就会**暂时退出堆栈**，上下文环境冻结，执行next()时上下文环境重新加入调用栈。



# ES6 系列之 async/await

## async/await简介

async是Generator函数的**语法糖**。

当async函数执行的时候，一旦**遇到`await`就会先返回**，等到**异步操作完成，再接着执行函数体内后面的语句**。

`async`函数**返回一个 Promise 对象**：

* 可以使用**`then`方法添加回调函数**，当内部**所有`await`命令**异步执行完成/return，会执行then并**传入返回值**
* `async`函数**内部抛出错误|某个await命令后面的Promise对象变为reject状态**，会导致**返回的 Promise 对象变为`reject`状态**，可以用**catch方法捕获**。



## async & Generator函数

async 函数是 **Generator 函数的语法糖**，改进如下：

* **内置执行器**：**Generator 函数的执行依赖于执行器**【手动 | **co模块** | thunk函数】，async函数**自带执行器**

* **更好的语义**：async 和 await**语义更清楚**，async表示函数里有异步操作，await表示表达式需要等待结果。

* **更广的适用性**：**`co`模块**约定，**`yield`命令后面只能是 Thunk 函数或 Promise 对象**，await还能是原始类型

* **返回值是 Promise**，可以**直接用`then`方法**指定下一步的操作**、catch捕获错误**



### async 函数的实现原理

async 函数的实现原理，就是**将 Generator 函数和自动执行器，包装在一个函数里**

```js
async function fn(args) {
  // ...
}

// 等同于

function fn(args) {
  return spawn(function* () {
    //...
  })
}
function spawn(genF) {
  return new Promise(function (resolve, reject) {
    const gen = genF();
    function step(nextF) {
      let next;

      try {
        next = nextF();
      } catch (e) {
        return reject(e);
      }

      if (next.done) {
        return resolve(next.value);
      }
      
      Promise.resolve(next.value).then(function (v) {
        step(function () { return gen.next(v); });  
      }, function (e) {
          step(function () { return gen.throw(e);})
      })
    }
    step(function() { return gen.next(undefined); });
  })
}
```



### async的并发应用

`map`方法的参数是`async`函数，但它是**并发执行**的，因为**只有`async`函数内部是继发执行，外部不受影响。**

```js
async function logInOrder(urls) {
  // 并发读取远程URL
  const textPromises = urls.map(async url => {
    const response = await fetch(url);
    return response.text();
  });

  // 按次序输出
  for (const textPromise of textPromises) {
    console.log(await textPromise);
  }
}
```



### 顶层 await

**在模块的顶层独立使用`await`命令**，目的是为了解决**模块异步加载**的问题，异步操作完成时，模块才会输出值

* 注意，如果加**载多个包含顶层`await`命令的模块，加载命令是同步执行的。**

> 之前需要让原始模块输出一个Promise对象，由Promise对象判断异步操作有没有结束
>
> * 要求模块的使用者按照特殊的方法使用这个模块，否则出错
> * 如果usage又需要输出，则等于这个依赖链的所有模块都要使用 Promise 加载。



# ES6 系列之 模块化

## ES6 模块与 CommonJS 模块的差异 

- CommonJS 模块输出的是一个**值的拷贝**，ES6 模块输出的是**值的引用**。
  - CommonJS 一旦输出一个值，模块内部的变化就影响不到这个值
- CommonJS 模块是**运行时加载**，ES6 模块是**编译时输出接口**
  - CommonJS 加载的是一个对象（即`module.exports`属性），该对象只有在脚本运行完才会生成。
  - ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。
  - JS 引擎对**脚本静态分析**的时候，遇到模块加载命令`import`，就会生成一个**只读引用**。等到脚本**真正执行时**，再**根据这个只读引用，到被加载的那个模块里面去取值**。
  - ES6 模块是**动态引用**，并且不会缓存值，模块里面的变量绑定其所在的模块。





# 原理场景类型题

### 数据类型系列 - 输出了什么？

```js
function test(person) {
  person.age = 26
  person = {
    name: 'hzj',
    age: 18
  }
  return person
}
const p1 = {
  name: 'fyq',
  age: 19
}
const p2 = test(p1)
console.log(p1) // -> ?
console.log(p2) // -> ?
```

结果

```js
p1：{name: “fyq”, age: 26}
p2：{name: “hzj”, age: 18}
```

> 原因：
>
> * 对象是**引用数据类型**，**按址访问**
> * 在**函数传参**的时候传递的是**对象在堆中的内存地址**，test函数中的实参person是p1对象的内存地址，通过调用person.age = 26改变的是p1变量标识符所指向堆内存中的那一块对象内容
> * 随后person变成了另一块内存空间的地址，并且在最后将这另外一份内存空间的地址返回，赋给了p2
> * 而p1在堆内存中的对象内容已经发生改变，age = 26







### '1'.toString()为什么可以调用？

其实在这个语句运行的过程中做了这样几件事情：

```js
var s = new Object('1');
s.toString();
s = null;
```

第一步: **创建Object类实例**。

> 注意为什么不是String ？
>
> 目前ES6规范也**不建议用new来创建基本类型的包装类**。【对Symbol和BigInt调用new会报错TypeError】

第二步: **调用实例方法**。

第三步: 执行完方法**立即销毁**这个实例。



### 0.1+0.2为什么不等于0.3？

* Javascript如何存储小数？JavaScript 中所有数字包括**整数和小数**都只有一种类型 — **Number**
* Number的实现遵循 **IEEE 754 标准**，使用 **64 位固定长度**来表示，也就是标准的 **double 双精度浮点数**
  * 优点：**归一化处理整数和小数**，节省存储空间
* 将 0.1和0.2在转换成二进制后会**无限循环**，由于**标准位数的限制**后面多余的位数会被**截掉**，此时就已经出现了**精度损失**，**相加后**因浮点数小数位的**限制而截断**的二进制数字转换为十进制就会变成0.30000000000000004



> #### 扩展：如何解决浮点误差？
>
> 理论上用**有限的空间**来存储**无限的小数**是**不可能保证精确**的，但可以处理一下得到期望的结果。
>
> * **数据展示**：使用 toPrecision(12) 凑整并 parseFloat 转成数字后再显示
>
>   ```js
>   parseFloat(1.4000000000000001.toPrecision(12)) === 1.4  // True
>   ```
>
> * **数据运算**：把小数通过pow()转成整数后再运算，再将结果缩小同样的倍数换成小数
>
>   ```js
>   function add(num1, num2) {
>     const num1Digits = (num1.toString().split('.')[1] || '').length;
>     const num2Digits = (num2.toString().split('.')[1] || '').length;
>     const baseNum = Math.pow(10, Math.max(num1Digits, num2Digits));
>     return (num1 * baseNum + num2 * baseNum) / baseNum;
>   }
>   ```



### [] == ![]结果是什么？

**结果**：true

**原因**：

* 先执行右边的 ![]，[]是引用类型转换为布尔值是true，取反得到false
* [] == false 的比较变成**引用类型与基本数据类型之间的比较**，做法是将左右两边都转化为数值类型，两边都为0
* 0==0 结果为true





### 输出

```
function ALIBABA() { 
	return 1 ; 
}
var ALIBABA; 
console.log( typeof ALIBABA );
```

**原因：函数提升要比变量提升的优先级要高一些**，且不会被变量声明覆盖，但是会被变量赋值之后覆盖。



### 理解Promise.resolve()

```js
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

Promise.resolve(console.log('zero'))

console.log('one');

// zero one two three 
```

- setTimeout(fn, 0)在**下一轮“事件循环”开始时**执行（宏任务）
- Promise.resolve().then() 在本轮“**事件循环”结束时**执行（微任务）
- console.log('one')则是**立即执行**，因此最先输出。
- **Promise.resolve() 参数中传入的可执行代码即为同步代码，立即执行**dui