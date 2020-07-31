## Set & Map

> Set 和 Map 主要的应用场景在于 **数据重组**（去重） 和 **数据储存**，底层原理是**哈希算法**
>
> **哈希算法：**根据设定的**哈希函数H(key)**和**处理冲突方法**将一组关键字映射到一个**有限的地址区间**上，根据key值访问数据**（快速存取，空间换时间）**
>
> * 可以理解为一个线性表，但是其中的元素不是紧密排列的，而是可能存在空隙
> * 解决冲突的方法：线性探查法（向前找一个空），双散列函数法
>
> ****
>
> ---

- **Set 集合**：[value, value]，键值与键名是一致的（或者说**只有键值，没有键名**）
  - **成员唯一、无序且不重复**
  - **可以遍历**，相关方法有：add、delete、has
- **Map** **字典**：[key, value]，**key值唯一**
  - **可以遍历**，方法很多，可以跟各种**数据格式转换**；相关方法有 set、get、has、delete
- **WeakSet**
  - **成员都是对象**，并且是**弱引用**，可以被垃圾回收机制回收，**可以用来保存DOM节点，不容易造成内存泄漏**
  - **不能遍历**，方法有add、delete、has
- **WeakMap**
  - **只接受对象作为键名**，并且**键名是弱引用**，**键名所指向的对象可以被垃圾回收**，回收后键名无效
  - **不能遍历**，方法有get、set、has、delete



## 箭头函数和普通函数的区别

* **this相关**
  * **箭头函数没有自己的this**，其内部的`this`就是箭头函数在**定义时**所处**外层执行环境的this**（从自己的**作用域链的上一层继承**）
  * **普通函数**的this指针则与他的**调用方式有关**
  * 箭头函数中**`this`指向固定化**，**不能用`call()`、`apply()`、`bind()`这些方法去改变`this`**
* **箭头函数不可以当构造函数**，即**不可以使用`new`命令**【因为new中需要在新创建的对象上执行该函数，涉及this指针的改变】
* 箭头函数**没有原型prototype**，除了`this`，**变量`arguments`、`super`、`new.target`**在箭头函数之中也是不存在的，**指向外层函数的对应变量**。如果要用arguments，**可以用Rest参数（一个数组）代替。**   （...rest)=>{console.log(rest)}
* 箭头函数**不可以使用`yield`命令**，因此箭头函数**不能用作Generator函数**（生成器）
* 箭头函数相较于普通函数，**语法简洁、清晰**，常用于简化回调函数



## let、const、var的区别

* **作用域**：`let`、`const`所声明的变量，只在**`let`、`const`命令所在的代码块{}内有效**，而var不是
* **变量提升**：`var`命令会发生**“变量提升”现象**：**变量可以在声明之前使用，值为`undefined`**，而`let`、`const`命令规定**声明的变量一定要在声明后使用，否则报错**【减少运行时错误】
  * 本质：编译时在词法环境中var会提前登记
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



## Object.assign(target, ...sources)

**作用**：将源对象中所有**可枚举**属性的**值复制**到目标对象，**返回目标对象**（**浅拷贝**）

**语法**：Object.assign(target, ...sources)

**注意事项**：如果目标对象中的属性具有相同的键，则属性将被源对象中的属性**覆盖**





# ES6 的继承

## ES6继承简述

* ES6的继承是通过**class**丶**extends** 关键字来实现的

* constructor 方法是**类的构造函数**，在ES6里面，通过 **new 命令创建对象实例时，会自动调用该方法**。在实现继承时，需要在子类的 constructor 里通过 **super()** 去调用父类的constructor。可以理解为是**在子类（传入子类实例this）上借助父类的构造函数来添加父类上的属性**实现父类**实例属性的继承**，执行完super()之后，可以在constructor()里对this进行进一步加工，添加子类的实例属性。

* 在constructor中super指的是**父类的构造函数**；而如果把super**当作对象使用**时：

  * **在子类的普通方法**中，super指向父类的**原型对象**；因为父类的实例方法保存在父类构造函数的原型上，所以可以在子类的普通方法中通过super去调用父类的**实例方法**

  * **在子类的静态方法（static）**中，super指向**父类**；因为在ES6中类的静态属性、方法是保存在类上的；所以可以在子类的静态方法中通过super去调用父类的**静态属性、方法**

    * 加上static，表示该方法不会被实例继承，而可以直接通过类来调用

  

## ES6继承的本质：两条继承链

作为一个**构造函数**，子类（`B`）的原型对象（`prototype`属性）是**父类的原型对象（`prototype`属性）的实例**。

作为一个**对象**，子类（`B`）的原型（`__proto__`属性）是父类（`A`）；

```js
// B 的原型继承 A 的原型
Object.setPrototypeOf(B.prototype, A.prototype);

// B 继承 A 的静态属性
Object.setPrototypeOf(B, A);
```



## Class类

* 如果某个方法之前加上星号（`*`），就表示该方法是一个 Generator 函数，也就是该类的默认遍历器 for of



#### constructor 构造函数

使用 new 操作符时会自动执行constructor，返回一个对象实例

#### 静态方法

在一个方法前加上`static`关键字，就表示该方法**不会被实例继承**，而是**直接通过类来调用**，这就称为“静态方法”

* 只能**通过类名去调用**，如 Foo.bar()；如果在**实例上调用静态方法**，会抛出一个**错误**
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



## 为什么ES6中引入了Class

因为ES5中的继承方法**与其他传统面向对象语言（C++、Java）差异较大**。故提供了更接近传统语言的写法，引入了 Class的概念，作为对象的模板。

本质上只是一个**语法糖**，使得对象原型的写法更加清晰、更像面向对象编程的语法

typeof + 类 === ‘function’，也就是说类的数据类型就是函数，类本身就指向构造函数。





## ES6中与ES5继承的不同点

* **调用方法**：
  * ES6 的类 类似于构造函数，**本质上就是一个函数**，但**ES6中类必须使用`new`调用**，否则会**报错**。
  * ES5 中 而普通的构造函数**不用`new`也可以执行**。
* **实例方法是否可枚举**：**ES6**中Class的**实例方法**和**ES5**中的寄生组合式继承**都定义在构造函数的Prototype上**
  * ES6中Class中定义的实例方法是**不可枚举**的
  * ES5中在构造函数原型上定义的实例方法是**可枚举**的
* **严格模式**：ES6中类的内部默认就是**严格模式**，不需要使用 use strict【也只能用严格模式】
* **是否存在变量提升**
  * ES6中：类**不存在变量提升**，不会把类的声明提升到代码头部，原因是**保证子类在父类之后定义**
  * ES5中具体分析

----

* **ES5** 的继承，实质是**先创造子类的实例对象`this`**，然后再将父类的方法添加到`this`上

* **ES6** 的继承，实质上是**先将父类实例对象的属性和方法，加到`this`上面**，然后再用子类的构造函数修改`this`。【子类中只有调用`super`之后，才可以使用`this`关键字】

  

## Object.getPrototypeOf()

用来从子类上获取父类。可以使用这个方法判断，一个类是否继承了另一个类。



# ES6 的proxy

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





# ES6 的模块化

## ES6模块

> 模块化可以**将一个大程序拆分成互相依赖的小文件**，再用简单的方法拼装，有利于开发大型、复杂项目
>
> 在 ES6 之前，社区模块加载方案（最主要）： **CommonJS 和 AMD** 。前者用于**服务器**，后者用于**浏览器。**

**ES6 模块化**：ES6 模块**不是对象**， 它的对外接口是一种**静态定义**，**输出一个只读引用**（地址），在代码**静态解析阶段**就可以生成；所以我们称之为：**编译时输出接口**。

* **ES6 模块**的设计思想是**尽可能的静态化**，使得**编译时**就能确定**模块的依赖关系**、输入和输出的变量。

* Javascript 引擎对脚本进行**静态分析**，遇到 import 就会**生成一个只读引用（地址）**。等到**脚本真正执行时**，再**根据这个只读引用到被加载的那个模块里去取值**（接口名与模块内部变量建立了一一对应的关系）
  * 只读引用：不能修改引入变量的值（引用类型值，即地址）
* ES6 模块是**动态引用**，并**不会缓存值**，根据只读引用取值，一旦原始值变了，import加载取到的值也会改变。

- **编译时加载**：
  - 优点：编译时加载**效率高**；能在编译阶段做**静态优化、静态分析**（静态分析 => 可以引入宏，类型检验）
  - 缺点：ES6模块不是对象，所以**无法引用ES6模块本身**；无法实现条件加载（import()弥补）

---

ES6 模块化的特点：**编译时加载、严格模式、动态引用无缓存**

- ES6 模块是**动态引用**，输出的接口与对应模块值是**动态绑定关系**（通过接口可以获取模块内部实时的值）。
- ES6 的模块**自动采用严格模式**，不管是否加上`"use strict";`（顶层的this关键字返回undefined）



## ES6 模块与 CommonJS 模块的差异 

- CommonJS 模块是**一个对象**，该对象**只有运行时才能获取**【运行时加载】
- ES6 模块是在**编译时输出接口（只读引用）**，通过`export`**指定输出的模块代码**，`import`**引入其他模块**【编译时加载】

* CommonJS 模块输出的是一个**值的拷贝（缓存）**，**原始值的改变**不会影响到输出的值（不存在动态更新）

- ES6 模块输出的是**值的引用（动态引用）**，根据**只读引用**去原先的模块中取值，ES6 输出的接口与对应模块值是**动态绑定关系**（模块变，export变）

```js
// CommonJS模块
let { stat, exists, readFile } = require('fs');  //对象的结构，本质上是获取了一个对象

// ES6模块（从`fs`模块加载 3 个方法，其他方法不加载。）
import { stat, exists, readFile } from 'fs';
```

> **CommonJS 模块化**：
>
> * CommonJS中的模块是一个对象（也就是module.exports属性），输入时**查找对象属性**，也就是说只有在**脚本运行时才会生成**，所以称 CommonJS 模块是**运行时加载**的。
> * 在CommonJS 引入模块时该对象**会被缓存**，可以理解为就是**拷贝**了一份新的对象引进来，所以**原先模块内部内的变化不会影响到输出的值**。
>   * 涉及到对象的拷贝，基本数据类型复制，**复杂数据类型**则是**浅拷贝**（两个模块引用的对象指向同一个内存空间）
>   * 但是在CommonJS中可以输出一个**取值器函数**，输出后仍然可以读取模块内部的变化

---

共同点：

* CommonJS用require命令加载某个模块，会去运行模块代码（多次加载只在第一次加载时运行）
* import 加载某个脚本，也会去执行对应的模块代码（多次加载只在第一次加载时运行）



## 严格模式（ES5引入）

- 变量必须声明后再使用
- 函数的参数不能有同名属性，否则报错
- 不能使用`with`语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀 0 表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量`delete prop`，会报错，只能删除属性`delete global[prop]`
- `eval`不会在它的外层作用域引入变量
- `eval`和`arguments`不能被重新赋值
- `arguments`不会自动反映函数参数的变化
- 不能使用`arguments.callee`
- **禁止`this`指向全局对象**（ES6 模块之中，顶层的`this`指向`undefined`，即不应该在顶层代码使用`this`）
- 不能使用`fn.caller`和`fn.arguments`获取函数调用的堆栈
- 增加了保留字（比如`protected`、`static`和`interface`）



## 如何在浏览器中加载 ES6 模块

**传统脚本的加载方式：**script标签  |  defer  |  async  |   type="application/javascript"属性（可省略）

---

**浏览器加载 ES6 模块**：script标签  |  type="module"属性

* **表现特点**【效果等同于打开了defer属性】
  * **异步下载，延迟执行**，不会阻塞 DOM 渲染，即等到整个页面**渲染完，再执行**模块脚本
  * 多个< script type="module">会**按序依次执行**

* < script>标签的**async属性**也可以打开，**加载完就执行**，执行后恢复渲染（不按序，加载完就执行）



## import() 

### import() 的特点

* import() 支持**动态加载模块**，**运行时加载**，可以实现**条件加载**（非模块的脚本也可以使用）

  * `import`会被 JavaScript 引擎静态分析，**先于模块内的其他语句执行**，有利于编译器提高效率，但也导致**无法在运行时加载模块**
  * node的require()也是一种运行时加载，import() 可取代【区别：require() 异步加载，import() 同步加载】
  * import() **与所加载的模块没有静态连接关系**

  ```js
  const path = './' + fileName;
  const myModual = require(path);	// 运行时才知道加载哪个模块
  ```

* import() **返回一个 Promise 对象**，并且这个模块会作为then方法的参数传入（可以用**对象解构**获取接口）
  
  * default 输出的接口可以直接用参数获得【Promise这里很灵活！】

### import() 的适用场景

（1）**按需加载**：用到时再加载

（2）**条件加载**：运行时根据不同状态加载对应模块

（3）**动态的模块路径**：允许动态生成模块路径



## ES6模块的语法

#### import

`import`**输入的变量都是只读**的，因为它的**本质是输入接口**，不能在加载模块的脚本里改写接口

`import`后面的`from`指定模块文件的位置，可以是相对路径，也可以是绝对路径，`.js`后缀可以省略。

`import`命令具有**提升效果**，**会提升到整个模块的头部，首先执行。**【编译阶段就提升】

由于`import`是静态执行，所以**不能使用表达式和变量**，`import { 'f' + 'oo' } from 'my_module'`

**`import`语句会执行所加载的模块**【import多次也只执行一次】

最好不要将CommonJS 模块的`require`命令和 ES6 模块的`import`命令可以写在同一个模块里面

* 因为`import`在静态解析阶段执行，所以它是一个模块之中最早执行的。



```js
import {a} from './xxx.js'
a = {}; // Syntax Error : 'a' is read-only;

import {a} from './xxx.js'
a.foo = 'hello'; // 合法操作，但不建议这样修改，难以调试（动态引用，其他模块也会读取到改后的值）

import 'lodash'; //上面代码仅仅执行lodash模块，但是不输入任何值。

foo();
import { foo } from 'my_module';
//不会报错，因为import的执行早于foo的调用：编译阶段执行import命令提升代码，运行时调用
```



#### export

一个模块就是一个独立的文件。该文件内部的所有变量，除非是**用export导出，否则外部无法获取**

export **输出的变量得放在{}里**，才会被当成接口，否则只是相当于输出了某个值，不可取（除非是默认导出）

`export`、import 命令可以出现在模块的任何位置，只要处于**模块顶层**就可以，但**不能放在块级作用域中**

* 块级作用域内会报错，因为处于条件代码块之中，**无法做静态优化**，违背了 ES6 模块的设计初衷。

```js
//写法一
export var firstName = 'Michael';

//写法二（应该优先考虑使用这种写法。因为这样就可以在脚本尾部，一眼看清楚输出了哪些变量。）
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;
export { firstName, lastName, year }; 

//export时可以使用as关键字重命名
export { v1 as streamV1 };
```



#### 模块的整体加载

```javascript
import * as circle from './circle';
```



#### 模块的整体输出

```js
export * from 'my_module';
```



#### export default 命令

- 本质上，`export default` 就是**输出一个叫做`default`的变量或方法**，并且**可以重新命名**（导入时不加 {} ）
- 一个模块只能有一个默认输出

```javascript
export default a; // 将变量a的值赋给变量default（输出变量）
import newNameA from 'xxx.js'	// import时可以重新命名
```



#### export 与 import 的复合写法

在一个模块之中，先输入后输出同一个模块，import 语句可以与export语句写在一起。

```javascript
export { foo, bar } from 'my_module'; // 对外转发的接口在当前模块不能直接使用（实际上无导入）

// 可以简单理解为（但不能使用！）
import { foo, bar } from 'my_module';
export { foo, bar };
```

```javascript
// 接口改名
export { foo as myFoo } from 'my_module';

// 默认接口
export { default } from 'foo';
```

具名接口改为默认接口的写法如下。

```javascript
export { es6 as default } from './someModule';

// 等同于
import { es6 } from './someModule';
export default es6;
```

同样地，默认接口也可以改名为具名接口。

```javascript
export { default as es6 } from './someModule';

// 等同于
import * as ns from "mod";
export {ns};
```









# ES6 的 Promise

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



# ES6 的 Generator 

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



# ES6 的 async/await

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



