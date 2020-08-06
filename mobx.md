#### Mobx解决的问题

传统React使用的数据管理库为Redux。Redux要解决的问题是**统一数据流**，数据流**完全可控并可追踪**。要实现该目标，便需要进行相关的**约束**。Redux由此引出了dispatch action reducer等概念，对state的概念进行**强约束**。然而对于一些项目来说，太过强，便**失去了灵活性**。Mobx便是来填补此空缺的。

#### Redux和Mobx进行简单的对比：

1. Redux的编程范式是**函数式**的而Mobx是**面向对象**的；
2. 因此数据上来说Redux理想的是**immutable**的，**每次都返回一个新的数据**，而Mobx**从始至终都是一份引用**。因此Redux是支持**数据回溯**的；
3. 然而和Redux相比，**使用Mobx的组件可以做到精确更新**，这一点得益于Mobx的**observable**；对应的，Redux是**用dispath进行广播**，**通过Provider和connect来比对前后差别控制更新粒度**，有时需要自己写**SCU**；Mobx更加精细一点。

#### Mobx核心概念

Mobx的核心原理是**通过action触发state的变化**，进而**触发state的衍生对象**（**computed** value & **Reactions**）。

![img](https://img-blog.csdnimg.cn/20190531115303571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDM2OTU2OA==,size_16,color_FFFFFF,t_70)

State
在Mobx中，State就对应业务的最原始状态，通过observable方法，可以使这些状态变得可观察。
通常支持被observable的类型有三个，分别是Object, Array, Map；对于原始类型，可以使用Obserable.box。
值得注意的一点是，当某一数据被observable包装后，他返回的其实是被observable包装后的类型

const Mobx = require("mobx");
const { observable, autorun } = Mobx;
const obArray = observable([1, 2, 3]);
console.log("ob is Array:", Array.isArray(obArray));
console.log("ob:", obArray);
1
2
3
4
5
控制台输出为：

ob is Array: false
ob: ObservableArray {}
1
2
对于该问题，解决方法也很简单，可以通过Mobx原始提供的observable.toJS()转换成JS再判断，或者直接使用Mobx原生提供的APIisObservableArray进行判断。

computed
Mobx中state的设计原则和redux有一点是相同的，那就是尽可能保证state足够小，足够原子。这样设计的原则不言而喻，无论是维护性还是性能。那么对于依赖state的数据而衍生出的数据，可以使用computed。
简而言之，你有一个值，该值的结果依赖于state，并且该值也需要被obserable，那么就使用computed。
通常应该尽可能的使用计算属性，并且由于其函数式的特点，可以最大化优化性能。如果计算属性依赖的state没改变，或者该计算值没有被其他计算值或响应（reaction）使用，computed便不会运行。在这种情况下，computed处于暂停状态，此时若该计算属性不再被observable。那么其便会被Mobx垃圾回收。
简单介绍computed的一个使用场景
假如你观察了一个数组，你想根据数组的长度变化作出反应，在不使用computed时代码是这样的
————————————————
版权声明：本文为CSDN博主「若～～～」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_44369568/java/article/details/90713881