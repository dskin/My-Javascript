# 类型

数据类型是一门编程语言所必须具备的要素，用何种方式来存储数据以期望获得计算机与我们的正确交互是及其重要的，我们这样定义类型这个概念：类型是值的内部特征，它定义了值的行为，以使其区别于其他值。

## JavaScript的类型分类
> JavaScript目前一共有七种内置类型，包括了新增的symbol类型

* 字符串（string）
* 布尔值（boolean）
* 空值（null）
* 未定义（undefined）
* 数字（number）
* 对象（object）
* 符号（symbol）

其中，除对象外，其它六种被称为**基本数据类型**，对象也被称为**复杂数据类型**或**引用数据类型**。

数据类型总类这么多，为了判断某个变量是属于何种数据类型，于是我们引入了`typeof`检测符。

但是由于一些原因，我们会发现使用typeof检测符的时候会出现一些“小问题”：
1. typeof null会返回object而不是null，这是由于历史原因造成的错误，实际上是应该返回null的。
2. typeof somefunction会返回function而不是object,这是由于JavaScript中函数和对象特殊关系造成的，函数是对象，对象也是函数，函数在JavaScript中是一等公民。
3. typeof的安全防范机制。typeof 会对已声明未赋值或未声明的变量都显示undefined，某些情况下会造成困扰，可以利用这个小特性实现全局命名变量的检测判定。


### String类型
String类型也就是我们常说的字符串类型，它是由零或多个16位Unicode字符组成的字符序列，字符串可以用单引号或者双引号表示。

String类型有一些特殊的[转义序列](https://msdn.microsoft.com/zh-cn/library/2yfce773(v=vs.94).aspx)，可以表示一些特殊的字符形式。

JavaScript中字符串的值是不可变的，要改变某个字符串的值，底层实现是先销毁再创建。

将任何一个值转为String类型的方法，一是使用toString（）方法，二是使用String（）函数。二者的区别是null和undefined类型无法通过第一种方式进行转换。

### Boolean类型
Boolean类型比较凄惨，只用两个字面值，true和false，二者是区分大小写的。要是想将其他类型值转为Boolean类型，则除了以下五种情况外，都将转为true值。

* false
* 空字符串
* null
* undefined
* 0和NaN

### Null类型
Null值更凄惨，只有一个特殊的值，这就是null，它表示为空。这里需要注意的情况就是typeof null将返回object的小问题。此外，undefined和null做相等性测试的时候是相等的，因为undefined值派生于null。

### Undefined类型
undefined的疑惑就在于它既会对未声明的变量和已声明未赋值的值都返回undefined，而不是undeclared，这里值得关注。

### Number类型
JavaScript使用的是IEEE754格式来表示整数和浮点数值，因此也一直被浮点精度缺失的问题搞得头疼。Number类型也是JavaScript中最重要的数据类型之一。Number类型数值字面量可以使用十进制、八进制、十六进制来表示。

对于极大极小的数值，可以使用科学计数法表示的浮点数值进行表示。

Number.MIN_VALUE和Number.MAX_VALUE分别表示JavaScript所能表示的最小和最大数值，超过这个范围则用Infinity和-Infinity来表示。同时可以用`isFinite()`函数进行判定一个数是不是有穷数。

NaN表示非数值，它是一个特殊的数值，不与任何值相等。NaN这个数值出现的意义就在于当出现异常运算的过程中不报错从而终止运算，而是返回NaN，不影响其它代码的执行。可以用`isNaN()`来判定某变量是否是数值，isNaN可用于对象。

要想将其他类型的值转为Number类型，可通过`Number()`、`parseInt()`、`parseFloat()`这三个函数进行转换，第一个可用于任何数据类型，后两个则专门用于将字符串转为数值。

Number()函数转换规则：
* 布尔值，true和false转为1和0
* 数字，简单传入传出
* null，返回0
* undefined，返回NaN
* 字符串，进行特殊转换规则，较复杂不再论述P30
* 对象，则调用valueOf或toString

parseInt()函数转换规则则是遇到非数字会停止并返回之前已转换的数值，若开头就为非数值，则返回NaN。parseInt()接受参数作为转换进制基数。

### Object类型
> js高程定义对象概念说，对象其实就是一组数据和功能的集合。很简单精炼的概括。

Object类在JavaScript中是所有它的实例的基础，在这里先做一个简单的介绍，它本身以及它的实例都拥有以下属性方法。
* constructor 保存用于创建当前对象的函数
* hasOwnProperty()
* isPrototypeOf()
* propertyIsEnumerable()
* toLocalString()
* toString()
* valueOf()

--------------------------------------------------------------

## 引用类型的值（对象）的实例
> 这个标题有点难以理解，简单说，JavaScript中变量是可以接受两种类型值的，一种是基本数据类型的值，它是简单的数据段，包括string、boolean、undefined、null、number和symbol六种；另一种是引用数据类型的值，它是那些由多个类型的值构成的对象，只有object一种，也可以说还有函数（对象和函数有数不清的暧昧关系）。而object对象有多种实例对象，包括array和封装对象等。

### Object类型
Object类型是JavaScript中所有引用类型值的基类，大多数引用类型值都是它的实例。

一般来说，创建一个Object实例的方式有两种，一是new操作符跟Object构造函数；二是对象字面量表示法。如无意外，常使用第二种为佳。

同时，访问对象属性的方法也有两种，一是点表示法；二是方括号表示法。

#### instanceof操作符
我们前面使用typeof判断变量的数据类型，而这里我们用instanceof判断变量的引用类型，因为typeof判断null和object都返回object，而我们有许多种object引用类型，这就容易造成误判。于是，我们引入instanceof，通过`变量 instanceof 引用类型`是否返回true来判断该变量是否是我们需要判定的引用类型。

### Array类型
JavaScript的数组非常强大，可以用来存储任何数据结构，可以数值、字符串甚至对象混搭存储，这都不是问题。

创建一个Array类型的实例即数组的方式也有两种，一种是Array构造函数法；二是对象字面量法。

1. 构造函数法使用时，既可以添加new操作符，也可以不添加；同时，可以在构造函数的添加多个参数表示数组中应该包含的项；当然，也可以传递数值，表示直接创建数值个新对象。
2. 对象字面量法使用时，注意不要传递多个逗号表示的连续空项，可能会造成错误。

访问数组的方式是通过方括号中输入要访问的数值索引来完成。

判断变量是不是数组既可以通过`instanceof`来判定，也可以通过`Array.isArray()`方法来判定。

#### 数组常用方法
1. 栈方法
	* push() 数组后添加
	* pop() 数组后移除
2. 队列方法
	* shift() 数组前添加
	* unshift() 数组前移除
3. 重排序方法
	* reverse() 反转
	* sort() 排序
4. 操作方法
	* concat() 复制当前数组，连接指定参数创建新数组
	* slice() 返回指定参数位置的数组，可接受负数，不影响原数组
	* splice() 最强大的数组方法，返回原数组中删除的数
		+ 删除 接受两参数，删除的起点位置，删除的数目
		+ 插入 接受三参数，插入的起始位置，0（要删除的项数），要插入的项
		+ 替换 接受三参数，起始位置，要删除的项数和要插入的项
5. 位置方法
	* indexOf() 查找
	* lastIndexOf() 倒序查找
6. 迭代方法
	> 以下方法都有其具体的应用场景，但是这五个方法都接受两个参数，一个是在迭代的每一项元素要运行的函数，以及在运行该函数时的作用域对象，影响this的值。在第一个参数即需要运行的函数中，该函数接受三个参数，分别表示该数组项的值，该项在数组中的位置以及数组对象本身。这五个方法由于使用较少，对它的掌握程度还不够，需要多加使用。
	* every() 对数组中的每一项运行时都给定一个函数，若该函数对每一项都返回true，则返回true
	* filter() 返回该函数对每一项返回true的项组成的数组，相当于一个过滤器
	* forEach() 无返回值，相当于for循环
	* map() 返回每次函数调用的结果组成的数组
	* some() 该函数对任一项都返回true，则返回true
7. 归并方法
	* reduce()
	* reduceRight()

### Date类型
> javascript中的Date类型使用来自UTC1970年1月1日零时开始经过的毫秒数来保存日期，它所能保存的时间是在这个标准时间的前后一亿年，目前完全不用担心有存储不了时间的情况发生。

创建一个日期对象，使用`new Date()`构造函数方式进行创建。不传入参数则表示默认取得当前日期时间，要想创建一个自定义的日期时间对象，则要传入的参数是从1970年至所自定义时间经过的毫秒数，这种创建自定义对象的方式实在繁琐，因此，JavaScript提供了两个方法`Date.parse()`和`Date.UTC()`。

parse()和UTC()二者的区别只在于接收的表日期的字符串参数不同，同样返回相应日期和1970年相距的毫秒值。此外，若直接将表日期的字符串参数传入`new Date()`中，那么将会在底层自动调用parse()和UTC()。

`Date.now()`返回调用该方法时的日期时间毫秒数

同时，Date类型也像其它引用类型一样，重写了object类型的`toLocalString()`,`toString()`,`valueOf()`方法，不同浏览器会有不同的返回结果。

#### 日期格式化
* toDateStirng()
* toTimeString()
* toLocaleDateString()
* toLocaleTimeString()
* toUTCString()

#### 日期/时间组件方法
[相关属性方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date)

### RegExp类型
待完成补充

### Function类型