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

其中，除对象外，其它六种被称为**基本数据类型**，对象也被称为**复杂数据类型**。

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

Object类在JavaScript中是所有它的实例的基础，它本身以及它的实例都拥有以下属性方法。
* constructor 保存用于创建当前对象的函数
* hasOwnProperty()
* isPrototypeOf()
* propertyIsEnumerable()
* toLocalString()
* toString()
* valueOf()
