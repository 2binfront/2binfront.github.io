---
title: JavaScript
date: 2022-10-30 15:29:37
tags: JavaScript
---

# JS

## 基础

```
JS和python都是解释型语言，仅需解释器在每次执行时编译并解释执行，编译型语言如c++，Java编译一次后，产生可执行文件可多次执行，效率高

<!--本地路径/和\是等效的-->
<img src=".\Image\20161025\guo.jpg" />
<img src="./Image/20161025/guo.jpg" />
<img src=".\Image/20161025/guo.jpg" />
<img src="./Image\20161025\guo.jpg" />
<!--网络文件路径一定要使用正斜杠/-->
```

诞生于1995，出现时用于处理网页中的**前端验证**，发展中遵循ECMAScript标准，完整的JavaScript由三部分组成：**ECMAScript标准，DOM文档对象模型，BOM浏览器对象模型**

解释型语言，无需编译直接运行，动态语言，基于原型的面向对象，写在script标签中，从上到下逐条执行；标识符命名仅可以含有字母、数字、_、$

## 变量性质、关键字和特性

区分大小写，允许Unicode字母、数字和表意文字(意味着可以用汉字定义变量但不推荐)

当浏览器开辟出供代码执行的栈[内存](https://so.csdn.net/so/search?q=内存&spm=1001.2101.3001.7020)后，代码并没有自上而下立即执行，而是继续做了一些事情：**把当前作用域中所有带var/function关键字的进行提前的声明和定义 => 变量提升机制**

typeof 操作符

```js
typeof 'aaa';//"string"
```



### `let`和`var`的区别

 `let`和`const`不存在变量提升机制；`var`允许重复声明，而`let`不允许重复声明； `let`能解决`typeof`检测时出现的暂时性死区问题（`let`比`var`更严谨）；let创建的全局变量没有给window设置对应的属性；let会产生块级作用域

- `var`声明是全局作用域或函数作用域，而`let`和`const`是块作用域。
- `var`变量可以在其范围内更新和重新声明； `let`变量可以被更新但不能重新声明； `const`变量既不能更新也不能重新声明。
- 它们都被提升到其作用域的顶端。但是，虽然使用变量`undefined`初始化了`var`变量，但未初始化`let`和`const`变量。
- 尽管可以在不初始化的情况下声明`var`和`let`，但是在声明期间必须初始化`const`。

暂时性死区：在块级顶部到变量正式申明这块区域去访问这个变量的话，直接报错

```js
var x = 1;
if(true) {
  console.log(x);//报错而不是到外部引用x
  
  let x = 2;
}
```



### 六种数据类型

`String` `Number` `Boolean`  `Undefined` `Symbol` `Object` 

`Object`为引用数据类型  

- Number

  均采用IEEE 754存储数字，会把能转换为整数的小数转换为整数。科学计数法：3.123e7等效于3.123*10^7

  八进制值用0o做前缀，十六进制0x前缀

  浮点值最高精度为1e-17，不宜用js做科学计算

  `NaN`表示`Not a Number`，数据类型也是`Number`，有`Number.MIN_VALUE`=5E-324，`Number.MAX_VALUE`=1.797e308，还有正无穷 Infinity 和负无穷 -Infinity。函数 isFinite()可以判断数值是不是有限

  isNaN()函数可以判断所给参数是否能转换为数值，不能则返回 true

  ```js
  isNan('10');//返回false，因为能转化为数字
  
  num.toFixed(digits)//浮点数规整化，digits为小数点后保留位数
  
  Math.trunc() //return the integer portion of a number
  ```

  即将支持的 BigInt 大数，在number字面量后加上 n 即可。可以表示任意大整数。

- `null`值用来表示一个为空的对象，`typeof null`时返回`object`类型。原则上 null 表示一个空指针

- string：将其他数据类型转换为String类型：`toString`方法和`String`函数，前者不能转换null和undefined类型，后者可以，凡遇字母转换为`NaN`。  转换为Number类型：Number函数，针对字符串有`parseInt`和`parseFloat`，这两个函数遇到其他类型时会先将对象转换为字符串再进行转换。

  toString()方法在用于数值对象时可以接受参数，表示转换为不同进制的数字字符。

  ```js
  let num = 10; 
  console.log(num.toString()); // "10" 
  console.log(num.toString(2)); // "1010" 
  console.log(num.toString(8)); // "12" 
  console.log(num.toString(10)); // "10" 
  console.log(num.toString(16)); // "a" 
  ```

  *用加号操作符给一个值加上一个空字符串""也可以将其转换为字符串*

  - ``反引号，不同于单引号''和双引号""，可以包裹模板字符串，能够允许嵌入表达式的字符串字面量，嵌入${expression}即可

  - 标签函数标签函数 会接收被插值记号分隔后的模板和对每个表达式求值的结果。 标签函数本身是一个常规函数，通过前缀到模板字面量来应用自定义行为。

    ```js
    let a = 6; 
    let b = 9; 
    function simpleTag(strings, ...expressions) { 
        console.log(strings); 
        for(const expression of expressions) { 
            console.log(expression); 
        } 
        return 'foobar'; 
    } 
    let taggedResult = simpleTag`${ a } + ${ b } = ${ a + b }`; 
    // ["", " + ", " = ", ""] 字符串数组
    // 6 
    // 9 
    // 15 
    console.log(taggedResult); // "foobar" 
    
    ```

  - 用模板字面量也可以直接获取原始的模板字面量内容。在标签函数的第一个参数中，存在一个特殊的属性`raw`，其他地方无法对字符串数组使用。

- Symbol类型：唯一标识符，具有唯一性、隐藏性（用作对象属性名时无法通过object.key访问，而需要定制的object.getOwnPropertySymbols()方法得到对象中所有用作属性名的symbol）

  ```js
  let id1 = Symbol('id');
  let id2 = Symbol('id');
  console.log(id1===id2);//false
  
  let id3 = Symbol.for('id');
  console.log(id1===id3);//false
  
  let id4  =Symbol.for('id');
  console.log(id3===id4)//true
  ```

  全局注册并登记，使得相同参数注册的值symbol相等。前提是都通过for注册登记。

  

数字0，空字符串，NaN，空指针null，undefined都可以自动转化为false

null 其实属于自己的类型 Null，而不属于Object类型，typeof 之所以会判定为 Object 类型，是因为JavaScript 数据类型在底层都是以二进制的形式表示的，二进制的前三位为 0 会被 typeof 判断为对象类型，而 null 的二进制位恰好都是 0 ，因此，null 被误判断为 Object 类型。 **对象被赋值了null 以后，对象对应的堆内存中的值就是游离状态了，GC 会择机回收该值并释放内存。**因此，**需要释放某个对象，就将变量设置为 null，即表示该对象已经被清空，目前无效状态。**

## 基础语句

\=\=运算符会做强制类型转换	\=\=\=不会，单引号双引号和python一样，都可以用来包裹字符串，无区别。

typeof 操作符，返回其后变量或字面量的类型

\+ 能做数字间的加减运算，当任一对象为字符串时会把另一参加运算对象转化为字符串再进行字符串拼接，参与运算对象为 object 或其他类型时会先转换为字符串再按如上规则运算

- try{}catch{}finally{}

  1.try中有return, 会先将值暂存，无论finally语句中对该值做什么处理，最终返回的都是try语句中的暂存值。

  2.当try与finally语句中均有return语句，会忽略try中return。

### 空值合并运算符

**空值合并操作符**（**`??`**）是一个逻辑操作符，当左侧的操作数为 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/null) 或者 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined) 时，返回其右侧操作数，否则返回左侧操作数。

与[逻辑或操作符（`||`）不同，逻辑或操作符会在左侧操作数为[假值](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)时返回右侧操作数。也就是说，如果使用 `||` 来为某些变量设置默认值，可能会遇到意料之外的行为。比如为假值（例如，`''` 或 `0`）时。

```js
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0

const baz = 0 || 42;
console.log(baz);
// expected output: 42
```

逻辑空赋值 ??=

x ??= y ，x为null或undefined时才赋值为右值。

### 可选链操作符

?.	TS中遇到过

判断是否为空



### 解构赋值

ES6 

- 数组结构赋值：

  只要某种数据结构具有 Iterator 接口，都可以采用数组形式的解构赋值。

  ```js
  let [a=1,b=2]=[4,5]
  
  
  ```

  

- 对象解构赋值：

  注意loc: { start }和loc: start是不一样的，前者把start也作为模式串，而后者只有loc是模式串

  ```js
    const node = {
      loc: {
        start: {
          line: 1,
          column: 5
        }
      }
    };
    let { loc, loc: { start }, loc: { start: { line }} } = node;
    line // 1
    loc  // Object {start: Object}
    start // Object {line: 1, column: 5}
  ```

  默认值，生效的条件是，对象的属性值严格等于`undefined`。

  ```js
  var {x = 3} = {x: undefined};
  x // 3var 
  
  {x = 3} = {x: null};
  x // null
  ```

  由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。

  ```js
  let arr = [1, 2, 3];
  let {0 : first, [arr.length - 1] : last} = arr;//index
  first // 1
  last // 3
  ```







## 常用方法

### forEach

```
// 箭头函数
forEach(() => { /* … */ } )
forEach((value) => { /* … */ } )
forEach((value, key) => { /* … */ } )
forEach((value, key, map) => { /* … */ } )

// 回调函数
forEach(callbackFn)
forEach(callbackFn, thisArg)

// 内联回调函数
forEach(function() { /* … */ })
forEach(function(value) { /* … */ })
forEach(function(value, key) { /* … */ })
forEach(function(value, key, map) { /* … */ })
forEach(function(value, key, map) { /* … */ }, thisArg)
```





## Map和Set

- 一个 Object 的键只能是字符串或者 Symbols，但一个 Map 的键可以是任意值。
- Map 的键值对个数可以从 size 属性获取，而 Object 的键值对个数只能手动计算。

Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。重复值的元素会被略去。

```
Set.prototype.add(value)
在Set对象尾部添加一个元素。返回该 Set 对象

Set.prototype.delete(value)
移除值为 value 的元素，并返回一个布尔值来表示是否移除成功。Set.prototype.has(value) 会在此之后返回 false。

Set.prototype.has(value)
返回一个布尔值，表示该值在 Set 中存在与否。

Set.prototype.clear()
移除Set对象内的所有元素。

Set.prototype.forEach(callbackFn[, thisArg])
按照插入顺序，为 Set 对象中的每一个值调用一次 callBackFn。如果提供了thisArg参数，回调中的 this 会是这个参数。

Set.prototype.values()
返回一个新的迭代器对象，该对象包含 Set 对象中的按插入顺序排列的所有元素的值。
```

let newArray = Array.from(set)，可以把括号内的对象转换为array并返回



```
Map.prototype.size
返回 Map 对象中的键值对数量。

Map.prototype.get(key)
返回与 key 关联的值，若不存在关联的值，则返回 undefined。

Map.prototype.has(key)
返回一个布尔值，用来表明 Map 对象中是否存在与 key 关联的值。

Map.prototype.set(key, value)
在 Map 对象中设置与指定的键 key 关联的值 value，并返回 Map 对象。

Map.prototype.delete(key)
移除 Map 对象中指定的键值对，如果键值对存在并成功被移除，返回 true，否则返回 false。调用 delete 后再调用 Map.prototype.has(key) 将返回 false。
```

### WeakMap

**`WeakMap`** 对象是一组键/值对的集合，其中的键是弱引用的。其键必须是对象，而值可以是任意的。

map API *可以* 通过使其四个 API 方法共用两个数组（一个存放键，一个存放值）来实现。给这种 map 设置值时会同时将键和值添加到这两个数组的末尾。从而使得键和值的索引在两个数组中相对应。当从该 map 取值的时候，需要遍历所有的键，然后使用索引从存储值的数组中检索出相应的值。

但是存在两个缺点：

1. 首先赋值和搜索操作都是 *O(\*n*)* 的时间复杂度（*n* 是键值对的个数），因为这两个操作都需要遍历全部整个数组来进行匹配。
2. 另外一个缺点是可能会导致内存泄漏，因为数组会一直引用着每个键和值。这种引用使得垃圾回收算法不能回收处理他们，即使没有其他任何引用存在了。

相比之下，原生的 `WeakMap` 持有的是每个键对象的“弱引用”，这意味着在没有其他引用存在时垃圾回收能正确进行。原生 `WeakMap` 的结构是特殊且有效的，其用于映射的 key _只有_在其没有被回收时才是有效的。

**正由于这样的弱引用，`WeakMap` 的 key 是不可枚举的**（没有方法能给出所有的 key）。如果 key 是可枚举的话，其列表将会受垃圾回收机制的影响，从而得到不确定的结果。因此，如果你想要这种类型对象的 key 值的列表，你应该使用 [`Map`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)。

- [`WeakMap.prototype.delete(key)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/delete)

  删除 WeakMap 中与 `key` 相关联的值。删除之后， `WeakMap.prototype.has(key)` 将会返回 `false`。

- [`WeakMap.prototype.get(key)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/get)

  返回 WeakMap 中与 `key` 相关联的值，如果 `key` 不存在则返回 `undefined`。

- [`WeakMap.prototype.has(key)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/has)

  返回一个布尔值，断言一个值是否已经与 `WeakMap` 对象中的 `key` 关联。

- [`WeakMap.prototype.set(key, value)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakMap/set)

  给 `WeakMap` 中的 `key` 设置一个 `value`。该方法返回一个 `WeakMap` 对象。

## 数组



```
pop()删除末尾元素 ，返回移除的数组元素；
push(ele)末尾添加元素 ele，返回添加后数组的长度；
shift()删除首元素并移动数组位置 ，返回添加后数组的长度；
unshift(ele)在数组首位置添加元素ele，返回添加后数组的长度；

shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。

unshift方法
unshift() 方法将一个或多个元素添加到数组的开头，并返回该数组的新长度（该方法修改原有数组）
arr.unshift(element1, ..., elementN)
返回其 length 属性值

Array.isArray(val):判断val是不是数组

Array.prototype 属性表示 Array 构造函数的原型，并允许向所有Array对象添加新的属性和方法。或者说，允许利用prototype向任何对象添加属性和方法，从而应用到对象的所有实例上
```

### 遍历操作

最常用for (var i = 0; i < a.length; i++) {
    // Do something with a[i]
}

for (. of array)：for (const currentValue of a)  {// Do something with currentValue}

for(. in array)：for (var i in a) {// 操作 a[i]}遍历索引，如果直接向 Array.prototype 添加了新的属性，使用这样的循环这些属性也同样会被遍历。不推荐这个循环

forEach()：
array.forEach(function(currentValue, index, array) {// 操作 currentValue 或者 array[index]});

注: forEach() 对于空数组是不会执行回调函数的。

### [Array.prototype.at()](https://link.segmentfault.com/?enc=gIfegOUoZxxLBOg%2FnmpAvw%3D%3D.it%2BTiWVsG0Y95ewedhlKoMS2CPXZSbhECA0MBePgVHbbS3pnn4r95vxFQo8Wnk%2BrFcIZQbnMXjwFeEBW9uTMx%2FmnhkJ0ck8W%2FV9MIZGB4SZb5GqmV1RLVPtXvNkMw7kq)

- 返回at中参数指向的index的数组元素，支持负数

### array.prototype.concat()

拼接两个数组

```js
concat()
concat(value0)
concat(value0, value1)
concat(value0, value1, /* … ,*/ valueN)
```

`valueN` 可选

数组和/或值，将被合并到一个新的数组中。如果省略了所有 `valueN` 参数，则 `concat` 会返回调用此方法的现存数组的一个浅拷贝。详情请参阅下文描述。



###  array.indexOf

- 判断数组中是否存在某个值，如果存在返回数组元素的下标，否则返回-1

### array.includes(searchElement[, fromIndex])

- 判断一个数组是否包含一个指定的值，如果存在返回 true，否则返回false。

### reduce()方法 

**`reduce()`** 方法对数组中的每个元素按序执行一个由您提供的 **reducer** 函数，每一次运行 **reducer** 会将先前元素的计算结果作为参数传入，最后将其结果汇总为单个返回值。

```js
array.reduce(
	function(
		total,
		currentValue,
		[currentIndex,
		[arr]]),
	[initialValue])
```

reduce() 方法接收一个回调函数作为参数，reduce 为数组中的每一个元素依次执行回调函数，回调函数接受四个参数：初始值（或者上一次回调函数的返回值），当前元素值，当前索引，调用 reduce 的数组。
reduce方法的返回值为回调函数最后的返回值。

如果没有提供initialValue，那么reduce的第一轮回调函数中的“total”就arr[0]，“current_Value”就是arr[1]，index就是1；

如果提供initialValue，那么reduce的第一轮回调函数中的“total”就是initialValue，“current_Value”就是arr[0]，index就是0。

所以在使用reduce函数时，回调函数中return最好不要省，而且initialValue也最好不要省！！！

### filter() 方法

返回一个新数组，其包含通过所提供函数实现的测试的所有元素。 

var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])

上式中 element 必需，后三者都可选，但没传 index 则 array 也不能传

注：当所过滤的数组是对象数组的情况时，对新返回的数组元素属性做出修改，同时对原数组也会造成影响；当过滤数组为纯数组时，修改不会改变原数组。也就是浅拷贝

### from方法

Array.from(arrayLike[, mapFn[, thisArg]])参数分别为伪数组对象或可迭代对象，新数组中的每个元素会执行的回调函数，执行回调函数 mapFn 时 this 对象
返回一个新的数组实例。

Array.from(arrayLike[, mapFunction[, thisArg]])：arrayLike：必传参数，想要转换成数组的伪数组对象或可迭代对象。
mapFunction：可选参数，mapFunction(item，index){...} 是在集合中的每个项目上调用的函数。返回的值将插入到新集合中。
thisArg：可选参数，执行回调函数 mapFunction 时 this 对象。这个参数很少使用。

### array.map() 方法

返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值，按照原始数组元素顺序依次处理元素。
array_back=array.map(function(currentValue,index,arr), thisValue)

注：map()方法不会对空数组进行检测；也不会改变原数组

eg.	let arr = new Array( 2 ).fill( 0 ).map( _ => new Array( 3 ) ); //作用类似C语言：int arr[2][3]

### arr.slice([begin[, end]])

- `begin` 可选，可为负。如果 `begin` 大于[数组](https://so.csdn.net/so/search?q=数组&spm=1001.2101.3001.7020)长度，返回空数组。slice(-1) 提取最后一个元素，slice(-2)提取最后两个元素，依次类推。前包后不包。
  [slice](https://so.csdn.net/so/search?q=slice&spm=1001.2101.3001.7020)() 返回整个数组。

### array.splice(start[, deleteCount[, item1[, item2[, ...]]]])

- start 指定修改的开始位置（从 0 计数）。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位（从 -1 计数，这意味着 -n 是倒数第 n 个元素并且等价于 `array.length-n`）；如果负数的绝对值大于数组的长度，则表示开始位置为第 0 位。
- deleteCount :整数，表示要移除的数组元素的个数。如果 `deleteCount` 大于 `start` 之后的元素的总数，则从 `start` 后面的元素都将被删除（含第 `start` 位）。如果 `deleteCount` 被省略了，或者它的值大于等于`array.length - start`(也就是说，如果它大于或者等于`start`之后的所有元素的数量)，那么`start`之后数组的所有元素都会被删除。如果 `deleteCount` 是 0 或者负数，则不移除元素。这种情况下，至少应添加一个新元素。
- item:从start位置要添加进数组的元素，不指定时 splice 将只删除元素。

返回被删除的元素

### arr.sort([compareFn])

如果没有指明 `compareFn` ，那么元素会按照转换为的字符串的诸个字符的 Unicode 位点进行排序。例如 "Banana" 会被排列到 "cherry" 之前。当数字按由小到大排序时，9 出现在 80 之前，但因为（没有指明 `compareFn`），比较的数字会先被转换为字符串，所以在 Unicode 顺序上 "80" 要比 "9" 要靠前。

- 如果 `compareFn(a, b)` 大于 0 ， b 会被排列到 a 之前。
- 如果 `compareFn(a, b)` 小于 0 ，那么 a 会被排列到 b 之前；
- 如果 `compareFn(a, b)` 等于 0 ， a 和 b 的相对位置不变。备注： ECMAScript 标准并不保证这一行为，而且也不是所有浏览器都会遵守（例如 Mozilla 在 2003 年之前的版本）；
- `compareFn(a, b)` 必须总是对相同的输入返回相同的比较结果，否则排序的结果将是不确定的。



### strArr.join(separator)

- 用输入参数分隔输入字符串数组的每个元素，返回新字符串

## 字符串

可以直接用比较符号比较字符串

### 编码

escape-unescape方法不会对 ASCII 字母和数字进行编码，也不会对下面这些 ASCII 标点符号进行编码： * @ - _ + . / 。其他所有的字符都会被转义序列替换。

encodeURI-decodeURI对以下在 URI 中具有特殊含义的 ASCII 标点符号，encodeURI() 函数是不会进行转义的： , / ? : @ & = + $ # 

encodeURIComponent-decodeURIComponent该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码： - _ . ! ~ * ' ( ) 。其他字符（比如 ：;/?:@&=+$,# 这些用于分隔 URI 组件的标点符号），都是由一个或多个十六进制的转义序列替换的。

**1、如果只是[编码字符串](https://www.zhihu.com/search?q=编码字符串&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A20300871})，不和URL有半毛钱关系，那么用escape。**

**2、如果你需要编码整个URL，然后需要使用这个URL，那么用encodeURI。**

比如

```js
encodeURI("http://www.cnblogs.com/season-huang/some other thing");
```

编码后会变为

```js
"http://www.cnblogs.com/season-huang/some%20other%20thing";
```

其中，空格被编码成了%20。但是如果你用了encodeURIComponent，那么结果变为

```js
"http%3A%2F%2Fwww.cnblogs.com%2Fseason-huang%2Fsome%20other%20thing"
```

看到了区别吗，连 "/" 都被编码了，整个URL已经没法用了。

**3、当你需要编码URL中的参数的时候，那么encodeURIComponent是最好方法。**

### String.prototype.charAt(index)

指定 `index` 处字符，参数不在 0 和字符串的 length-1 之间，则返回空字符串

### String.prototype.charCodeAt(index)

index：一个大于等于 `0`，小于字符串长度的整数。如果不是一个数值，则默认为 `0`。

返回值：指定 `index` 处字符的 UTF-16 代码单元值的一个数字；如果 `index` 超出范围，`charCodeAt()` 返回 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)。

如果指定的 `index` 小于 `0` 、等于或大于字符串的长度，则 `charCodeAt` 返回 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)。

### String.fromCharCode()

静态 **`String.fromCharCode()`** 方法返回由指定的 UTF-16 代码单元序列创建的字符串。

```js
String.fromCharCode(str[i].charCodeAt()+1); 
```

### indexOf() 和 lastIndexOf()

使用字符串的 indexOf() 和 lastIndexOf() 方法，可以根据参数字符串，返回指定子字符串的下标位置。这两个方法都有两个参数，说明如下。

- 第一个参数为一个子字符串，指定要查找的目标。
- 第二个参数为一个整数，指定查找的起始位置，取值范围是 0~length-1。

对于第二个参数来说，需要注意一下几个特殊情况。

- 如果值为负数，则视为 0，相当于从第一个字符开始查找。
- 如果省略了这个参数，也将从字符串的第一个字符开始查找。
- 如果值大于等于 length 属性值，则视为当前字符串中没有指定的子字符串，返回 -1。

### str.match(reg)

/./g

方法对字符串对象进行检索,返回包含所有匹配结果的数组。而 正则表达式 /./g 匹配的是所有的字符， 所以str.match(/./g)返回的是由字符串str中所有的字符组成的数组，以此达到将字符串转换为数组的目的。 

### str.replace(pattern, replacement)

两个参数均为字符串，寻找到模式串替换为后者。

### str.search(regexp)

返回 str 中给定正则表达式对应索引

### str.slice(a,b)

前包后不包；截取出来的字符串的长度为第二个参数与第一个参数之间的差；若参数值为负数,则将该值加上字符串长度后转为正值；若第一个参数等于大于第二个参数,则返回空字符串.

### str.substring(a,b)

前包后不包；若参数值为负数,则将该值转为0;两个参数中,取较小值作为开始位置,截取出来的字符串的长度为较大值与较小值之间的差.

### str.split(separator)

分割字符串，返回字符数组

### str.substr(a,length)

第一个参数代表开始位置,第二个参数代表截取的长度





## 函数

### 基础

```javascript
var avg = function(){}<=>function avg(){}
//js创建函数有两种：一是函数声明function fnName () {…};，二是函数表达式var fnName = function () {…};前者因 js 具有 函数声明提升 所以定义在任何位置均可成功调用，而后者必须等到定义语句被解释后才能正常调用(与 var 的变量定义提升不同)
//还有一种匿名函数：function () {…}; 使用function关键字声明一个函数，但未给函数命名，所以叫匿名函数，匿名函数属于函数表达式，匿名函数有很多作用，赋予一个变量则创建函数，赋予一个事件则成为事件处理程序或创建闭包等等。
函数表达式后面可以加括号立即调用该函数，*函数声明不可以，只能以fnName()形式调用* 存疑，在控制台中调用成功。是因为在函数定义前面加了运算符，比如用括号包裹，把它也转换为了表达式。
总结一下就是当把函数定义为表达式时总能在后面加上括号来立即调用。
(function() {})()  (function a() {})()  var a=function() {}()  三者均能立即调用。
```

由 `Function` 构造函数创建的函数不会创建当前环境的闭包，它们总是被创建于全局环境，因此在运行时它们只能访问全局变量和自己的局部变量，不能访问它们被 `Function` 构造函数创建时所在的作用域的变量。这一点与使用 [`eval()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval) 执行创建函数的代码不同。

```js
let a = 1
function foo() {
    console.log(a)
}
function too() {
    let a = 2
    foo()
}
too() // 1, not 2
```

```python
lambda x: x * 8
let ans=x => x*8;
```

js的=>符号类似于lambda

```
=>是es6中的arrow function语法
(x) => x + 6
相当于
function(x){return x + 6;};

const funcname=(args)=>{...}		函数调用：funcname(args)
```

eval()函数，参数是一个字符串。如果字符串表示的是表达式，`eval()` 会对表达式进行求值。如果参数表示一个或多个 JavaScript 语句，那么`eval()` 就会执行这些语句。如果 `eval()` 的参数不是字符串， `eval()` 会将参数原封不动地返回。

永远不要使用eval()







### 回调

嵌套函数可以访问父函数作用域中的变量，可以利用这个特性减少全局变量的数量，有效地防止“污染”你的全局命名空间——你可以称它为“局部全局（local global）”。换种思路，把全局当作整体函数，那么就能形成作用域链(scope chain)，嵌套者能访问被嵌套者的变量，反之则不行，寻找变量的定义时总是从当前嵌套层或者说从金字塔的当前区域往外(往下)寻找，就近选择。需要注意的是每个函数的金字塔是不同的，在该函数被定义的时候就已经确定了，所以当在函数内部调用之前已定义的函数时，应当回到那个函数的“金字塔”寻找其所需要的变量，当前函数的作用域不会与产生交集。

头等函数(first-class functions)，可以当作参数被传递的函数。回调函数(callback)是被作为参数传递的函数，注意是函数作为参数，而非函数返回值作为参数，与其对应的是高阶函数，是使用回调函数的函数。

回调机制包括三方：起始函数，中间函数，回调函数；起始函数调用中间函数，把回调函数作为参数传递给中间函数。起始函数一般是当前运行的主函数，一般隐藏忽略，主要关注回调函数和把回调函数作为参数的中间函数。

回调实际上有两种：阻塞式回调和延迟式回调。两者的区别在于：阻塞式回调里，回调函数的调用一定发生在起始函数返回之前；而延迟式回调里，回调函数的调用有可能是在起始函数返回之后。



### call、apply和bind

`myfunc(...args)<=>myfunc.apply(null,args)`展开语法将数组展开为数组元素。**剩余参数**语法允许我们将一个不定数量的参数表示为一个数组，与展开语法恰恰相反，形式为定义函数时`function fun1(...theArgs){alert(theArgs.length);}`。剩余参数也可以被解构为包含变量，形式为`function f(...[a, b, c]) {return a + b + c;}`. 

`apply()` 的第一个参数应该是一个被当作 `this` 来看待的对象。于是这里是全局对象。



`apply()` 有一个姐妹函数，名叫 [`call`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)，它也可以允许你设置 `this`，但它带有一个扩展的参数列表而不是一个数组。

```js
function.call(thisArg, arg1, arg2, ...)
```

thisArg是函数执行时的 this 对象。call 实现

```js
Function.prototype.myCall = function (context,...args) {
    // 判断调用对象
    if (typeof this !== "function") {
    	throw new Error("Type error");
    }
    let result = null;
    // 判断 context 是否传入，如果没有传就设置为 window
    context = context || window;
	args = args ? args : []
	const key = Symbol();
    context[key] = this;
	//通过隐式绑定的方式调用函数
    result = context[key](...args);
    // 删除手动增加的属性方法
    delete context[key];
    // 将执行结果返回
    return result;
};
```

类似的apply实现

```js
Function.prototype.myApply=function(context,args){
	if (typeof this !== "function") {
    	throw new Error("Type error");
    }
    args = args ? args : [];
    const key = Symbol();
    context[key]=this;
    const result = context[key](...args);
    delete context[key];
    return result;
}
```



bind先返回一个绑定了this的函数，再次执行则在this中执行

```
var a ={
	name : "Cherry",
	fn : function (a,b) {
		console.log( a + b)
	}
}

var b = a.fn;
b.bind(a,1,2)()           // 3
```

bind利用apply实现

```js
Function.prototype.myBind = function (context, ...args) {
    const fn = this
    args = args ? args : []
    return function newFn(...newFnArgs) {
        if (this instanceof newFn) {
            return new fn(...args, ...newFnArgs)
        }
        return fn.apply(context, [...args,...newFnArgs])
    }
}
```





### 函数柯里化

将接受 **n 个参数的 1 个函数改为只接受一个参数的 n 个互相嵌套的函数**，当前置部分参数一致时，可以通过固定前置参数生成指定函数，简化代码。

对应偏函数是柯里化的宽松情况，不一定需要每一层都只固定一个参数，继承思想即可。



### 函数参数的传递

有值传递和引用传递，基本类型传递值，引用类型(对象)传递对象的地址，如果在函数中对对象重新赋值，则传递进来的地址改变，即在堆中重新分配一段空间，改变传递进来地址的值指向这个新的地址，不影响原对象。

```
function changeAgeAndReference(person) {
    person.age = 25;
    person = {
        name: "John",
        age: 50
    };

    return person;
}
var personObj1 = {
    name: "Alex",
    age: 30
};
var personObj2 = changeAgeAndReference(personObj1);
console.log(personObj1); // -> {name: 'Alex', age: 25}
console.log(personObj2); // -> {name: 'John', age: 50}
```

对对象的重新赋值在任何地方都是如此：在动态堆中重新分配内存空间并赋值，再把原地址的值改为新对象的地址。

当一个对象没有对应地址指向时，也就是上一段中原对象的情况，这个对象的内存地址会被回收，这是js的垃圾回收机制

"如果连续五次垃圾回收之后，内存占用一次比一次大，就有内存泄漏。这就要求实时查看内存占用。"避免内存泄漏的要点在于往后不会使用的变量要及时赋空。





## 闭包、词法环境(作用域)

一个函数和对其周围状态（**lexical environment，词法环境**）的引用捆绑在一起（或者说函数被引用包围），这样的组合就是**闭包**（**closure**）。

- 用于保存私有属性：将不需要对外暴露的属性、函数保存在闭包函数的父函数里，避免外部操作对值的干扰
- 避免局部属性污染全局变量空间导致的命名空间混乱
- 模块化封装，将对立的功能模块通过闭包进去封装，只暴露较少的 API 供外部应用使用

缺点：内存消耗，由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题。

c语言退出函数时局部变量也会退出其作用域，所以难以创建闭包；js创建函数时会保留其能访问的变量的地址，这也是创建闭包的前提所在。



词法环境有两大成员：**「Environment Record（环境记录）」**，可能为 null 的 **「Outer Lexical Environment（外部词法环境引用）」**。**任何在环境记录中的标识符都可以在当前词法环境直接以标识符形式访问**。

Environment Record 是一个抽象类，存在三个具体的子类，**「Declarative Environment Record」** ，**「Object Environment Record」**，**「Global Environment Record（全局环境记录）」**

声明式环境记录保存 let、const、function 等非 var 声明标识符，对象式环境记录保存 var 声明标识符。 

### 对象式环境记录

对象式记录也是用于记录标识符与变量的映射，但是它只记录var声明的标识符 ； 并且它有一个关联的绑定对象(binding object)。

- 在词法环境中，会为对象式环境记录中所有的标识符绑定到绑定对象的同名属性上。
  例如var number=1000; , 也能够通过window.number形式获取到number的值。
- 反过来也可以，会将绑定对象的所有属性名（自然也必须是能做标识符的）绑定到对象式环境记录中的同名标识符上。
  例如：window.thousand = 1000; 然后直接以 thousand就能获取到该值（严格模式下报错）

- 每个标识符在绑定后都会直接实例化并初始化为undefined ，如果标识符已经绑定了绑定对象上的原有属性上，那么该变量就是对应属性值 。
  比如之前的isNaN在声明前使用时就有值，就是这个原因。
  变量提升也是这个原因造成的。
- 如果标识符已经存在，那么无视之，所以var可以重复声明。

### 声明式环境记录

同样的，声明式环境记录也比较特殊，它只记录非var声明的标识符，例如let、const、function……声明的标识符等等。并且它没有关联的绑定对象。

- 所有声明的标识符（这里应该包含var声明的标识符，但不建立关联）都位于此处。

- 将所有非var声明的标识符实例化，但不初始化，也就是变量处于uninitialized状态。也就是说内存中已经为变量预留出空间，但是还没有和对应的标识符建立绑定关系。
- 在执行上下文的运行（perform状态）阶段，并执行到声明语句时，才会真正初始化并默认赋值为undefined。
  所以你就懂了，let声明的标识符之前无法访问，就是因为还没有建立绑定。
  暂存死区的根本原因在此。

- 在声明式环境记录中，**不允许出现重复的标识符**，所以它无法重复。甚至和var声明的标识符冲突。注意，它会在代码加载后的预编译阶段（只能说是运行前，因为JS没有真正的预编译啊……）就已经完成。

全局环境记录包含前两者，是底层记录形式，绑定对象为 window 。

```js
function f(a,b){
    var t = 10;
    let sum = 10;
    {
        let sum = a+b;
        var mul = a*b +sum;
    }
    return  mul*t;
}
f(20,30) // 6500


//词法环境
FunctionEnv = {
    This:<window>
    outerEnv:<GlobalEnv>,
    ObjRec:{
        t:<10>,
        mul:<650>
    },
    DecRec:{
        sum:<10>
    }
}

BlockEnv={
    This:<window>,
    outerEnv:<FunctionEnv>,
    DecRec:{
        sum:<50>
    }
}

```

函数属于声明式环境记录是因为存在块级作用域，var 剥离出的环境记录只有全局作用域和函数作用域。

声明式对应 LexEnv，对象式对应 VarEnv

## 面向对象

this和new

```
this 使用在函数中时被用来指向当前调用函数的对象，也即在对象上使用. or [] 访问属性或者方法时，this就相当于这个对象，如果没有. or []依附对象进行直接访问时，this将指向全局对象（global object），也即访问全局属性/变量或者方法/函数。
Global execution context in scripts:this指全局对象-一个名字叫global的对象
Global execution context in modules：this返回undefined


new 创建一个崭新的空对象，然后使用指向那个对象的 this 调用特定的函数，修改this对象的属性。如果你没有使用 new 运算符，构造函数会像其他的常规函数一样被调用，并不会创建一个对象。在这种情况下，this 的指向也是不一样的。
```

for ... in object 可以遍历对象的所有属性，利用全局对象Object的方法keys可以获得属性名数组，例如有对象实例student，Object.keys(student)为student的所有属性名数组。判断对象是否包含某一属性可以用 in ，'keyName' in objectName 是bool值。当通过类实例化时，虽然方法可以访问，in也报true，但方法不属于对象本身，而是属于类。

const objectName={}，引用关系不可变，但引用内容可变，意思是仍然可以为对象分配属性。

类也是对象。js引擎会自动把常量包装成对象，以能使用对应的对象方法。如length等。



ES6中在对象中添加方法时可以不写key而是直接像下面这样

```
objectName{
...
	funcName(args...){
	...
	}
...
};

same as

objectName{
...
	funcName:function(args...){
	...
	}
...
};

λ-calculus
```



以下写法在ES6中合法

```
let obj={
	name:"hh",
	age:"13"
}

let {name,age}=obj;
```



`Object.defineProperty(object, property, methods)`	

第一个参数是一个对象，第二个参数是给该对象设置的属性名称，第三个参数是配置该属性的方法，比如set/get方法

在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

## 防抖节流

防抖：多次连续触发只执行一次

节流：一段时间内连续触发只执行一次，冷却时间过了可以继续

```vue
<template>
    <div id="content" 
         style="height:150px;
                line-height:150px;
                text-align:center; 
                color: #fff;background-color:black;
                font-size:80px;">
    </div>
</template>

 
<script>
    let num = 1;
    const content = document.getElementById('content');
    function count() {
        content.innerHTML = num++;
    };
    content.onmousemove = count;
	//防抖 非立即执行版
	function debunce(func,wait,...args){
        let timeout;
        return function(){
            const context = this;
            if(timeout) clearTimeout(timeout);
            timeout = setTimeout(()=>{
                func.apply(context,args);
            },wait);
        }
    }
    //防抖 立即执行
    function debunce(func,wait,...args){
        let timeout;
        return function(){
            const context = this;
            let callNow = !timeout;
            if(timeout) clearTimeout(timeout);
            timeout = setTimeout(()=>{
                timeout = null;
            },wait)
            if(callNow) func.apply(context,args);
        }
    }
    
    //节流 时间戳立即执行
    function throttle(func,wait,...args){
        let pre=0;
        return function(){
            const context = this;
            let now = Date.now();
            if(now-pre>=wait){
                func.apply(context,args);
                pre=Date.now();
            }
        }
    }
    //节流 延时器延迟执行
    function throttle(func,wait,...args){
        let timeout=0;
        return function(){
            const context = this;
            if(!timeout){
                timeout=setTimeout(()=>{
                    timeout=null;
                    func.apply(context,args);
                },wait);
            }
        }
    }
</script>
```



## 异步和同步

异步任务分为宏任务和微任务

**宏任务：**script/外层同步代码，定时器`setTimeout`，`setInterval`，node中的setImmediate，`事件绑定`，`回调函数`，`node中的fs模块`

**微任务：**`new Promise().then(回调)`，`process.nextTick()`，`async await`,`Object.observe`,`MutaionObserver`

Event Loop的执行顺序是：

1. 首先执行执行栈里的任务。
2. 执行栈清空后，检查微任务（microtask）队列，将可执行的微任务全部执行。
3. 取宏任务（macrotask）队列中的第一项执行。
4. 回到第二步。

await后面的函数会先执行一遍，然后就会跳出整个async函数来执行后面js栈（后面会详述）的代码。等本轮事件循环执行完了之后又会跳回到async函数中等待await后面表达式的返回值。

```
console.log("script start");

async function async1() {
  await async2();
  console.log("async1 end");
}

async function async2() {
  console.log("async2 end");
}

async1();

setTimeout(function () {
  console.log("setTimeout");
}, 0);

new Promise((resolve) => {
  console.log("Promise");
  resolve();
})
  .then(function () {
    console.log("promise1");
  })
  .then(function () {
    console.log("promise2");
  });

console.log("script end");
// script start => async2 end => Promise => script end => async1 end=> promise1 => promise2 => setTimeout
```

Promise

第一段调用了Promise构造函数，第二段是调用了promise实例的.then方法。promise的构造函数是同步执行，promise.then中的函数是异步执行。

promise实例有三种状态：

- pending（待定）
- fulfilled（已执行）/或者也可形象地叫做resolved
- rejected（已拒绝）

调用resolve和reject能将分别将promise实例的状态变成fulfilled和rejected，只有状态变成已完成（即fulfilled和rejected之一），才能触发状态的回调

```
let p = new Promise((resolve, reject) => {
  // 做一些事情
  // 然后在某些条件下resolve，或者reject
  if (/* 条件随便写^_^ */) {
    resolve()
  } else {
    reject()
  }
})

p.then(() => {
    // 如果p的状态被resolve了，就进入这里
}, () => {
    // 如果p的状态被reject
})
```



- 多个 then() 链式调用，**并不是连续的创建了多个微任务并推入微任务队列**，因为 then() 的返回值必然是一个 Promise，而后续的 then() 是上一步 then() 返回的 Promise 的回调

- 按照规范

  ```arcade
  async function async1(){
    console.log('async1 start')
    await async2()
    console.log('async1 end')
  }
  ```

  可以转化为：

  ```arcade
  function async1(){
    console.log('async1 start')
    return RESOLVE(async2())
        .then(() => { console.log('async1 end') });
  }
  ```

- `RESOLVE(p)`接近于`Promise.resolve(p)`，不过有微妙而重要的区别：p 如果本身已经是 Promise 实例，Promise.resolve 会直接返回 p 而不是产生一个新 promise；

- 如果`RESOLVE(p)`严格按照标准，应该产生一个新的 promise，尽管该 promise 确定会 resolve 为 p，**但这个过程本身是异步的**，也就是现在进入 job 队列的是**新 promise 的 resolve 过程**，所以该 promise 的 then 不会被立即调用，而要等到当前 job 队列执行到前述 resolve 过程才会被调用，然后其回调（也就是继续 await 之后的语句）才加入 job 队列，所以时序上就晚了

- 所以上述的 async1 函数我们可以进一步转换一下：

  ```arcade
  function async1(){
    console.log('async1 start')
    return new Promise(resolve => resolve(async2()))
      .then(() => {
        console.log('async1 end')
      });
  }
  ```

## JSON

JSON对象有两个方法。JSON支持三种类型值：简单值（不包括 undefined ，字符串、数字、null，布尔值均可），对象，数组。也没有分号

JSON.stringify()

JSON.parse()



## 网络请求和远程资源

Ajax

asynchronous JavaScript and XML

### XMLHttpRequest对象-XHR

XHR对象类型

```json
{
    //XMR方法
	open:function(method,url,isAsync),//必须首先使用的方法
	setRequestHeader:function(HeaderKey:string,HeaderValue:any)//自定义发送头部的信息，必须在open之后，send之前调用此函数。需要区别于浏览器正常发送头部，因为部分浏览器允许重写默认头部，某些则会引起错误
	send:function(arg),//参数为请求体数据，不存在请求体时参数需显示设置为 null

	//readyState变化时自动调用此函数
	onreadystatechange:function(),

	//获取响应头部信息
	getResponseHeaders:function(headerKey:string),
	getAllResponseHeaders():function(),
	
	//send 方法执行得到返回内容之后，这些属性会被填充
	responseType:"",
	responseText:"string",//响应体文本
	responseXML:XML DOM,//响应类型为 text/xml 或者 application/xml 时返回的包含响应式数据的 XML DOM 文档
	status:statusCode,//响应HTTP状态码 2xx表示成功，304表示资源未修改，直接从浏览器缓存读取，此两种情况都表示响应有效
	statusText:'description',//HTTP状态描述信息


	//状态属性
	readyState:0|1|2|3|4,//五种状态，0表示未调用 open 方法，未初始化，1表示已 open 但未 send，2表示 sent 但未收到响应，3表示收到部分响应 receiving ，4表示完成，已收到所有响应 complete。

	//收到响应之前可调用此方法终止异步请求，同时应当取消对该XHR对象的引用
	abort:function(),
	
}
```



```js
//创建XHR对象
let xhr = new XMLHttpRequest();

//使用XMR对象
//首先必须使用open方法，三个参数依次是	请求类型：string，请求URL：string，是否异步：Boolean；这里的URL是相对于代码所在的页面的，必须遵守同源策略（同一域名，同一端口，同一协议），否则抛出安全错误。
xhr.open('get','example.com',false);
xhr.setRequestHeader('myHaeder','myValue');
xhr.send(null);
```

send时XHR默认会发送的头部字段：

- Accept 浏览器可以处理的内容类型
- Accept-Charset 浏览器支持显示的字符集
- Accept-Encoding 浏览器可以处理的压缩编码类型
- Accept-Language 浏览器使用的语言
- Connection 浏览器与服务器的连接类型
- Cookie 页面中设置的Cookie
- Host 发送请求的页面所在的域
- Referer 发送请求的页面的 URL ，将错就错，正确拼法是 referre
- User-Agent 浏览器的用户代理字符串 

#### Get请求

The `encodeURIComponent()` method **encodes** a URI component. Use the [decodeURIComponent()](https://www.w3schools.com/jsref/jsref_decodeuricomponent.asp) function to **decode** an encoded URI component.

#### Post请求



### CORS

跨域资源共享 Cross-origin Resource Share使用场景：

- 由 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 或 [Fetch APIs](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API) 发起的跨源 HTTP 请求。
- Web 字体 (CSS 中通过 `@font-face` 使用跨源字体资源)，[因此，网站就可以发布 TrueType 字体资源，并只允许已授权网站进行跨站调用](https://www.w3.org/TR/css-fonts-3/#font-fetching-requirements)。
- [WebGL 贴图](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL_API/Tutorial/Using_textures_in_WebGL)
- 使用 [`drawImage`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/drawImage) 将 Images/video 画面绘制到 canvas。
- [来自图像的 CSS 图形 (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Shapes/Shapes_From_Images)

跨源资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源。另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 以外的 HTTP 请求，或者搭配某些 [MIME 类型](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types) 的 [`POST`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST) 请求），浏览器必须首先使用 [`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS) 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨源请求。服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 [Cookies](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies) 和 [HTTP 认证](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Authentication) 相关数据）。



## 接口

setInterval();  指每隔多少毫秒执行一次函数。因此它有两个参数，第一个参数为每次执行的函数，第二个参数为毫秒。如setInterval( fn, 16 )，返回值为id，用于标识一个setInterval调用。 

setTimeout() 方法只运行一次，也就是说当达到设定的时间后就开始运行指定的代码，运行完后就结束了，次数是一次。 setInterval() 是循环执行的，即每达到指定的时间间隔就执行相应的函数或者表达式，只要窗口不关闭或clearInterval() 调用就会无限循环下去。

date对象，包含一系列获取时间的方法



## [Control abstraction objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects#control_abstraction_objects)

控制抽象对象

- [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [`Generator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator)
- [`GeneratorFunction`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/GeneratorFunction)
- [`AsyncFunction`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AsyncFunction)
- [`AsyncGenerator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AsyncGenerator)
- [`AsyncGeneratorFunction`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/AsyncGeneratorFunction)



## Reflection

**Proxy** 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。

```
const p = new Proxy(target, handler)

handler 
包含捕捉器（trap）的占位符对象，可译为处理器对象。
traps
提供属性访问的方法。这类似于操作系统中捕获器的概念。
target
被 Proxy 代理虚拟化的对象。它常被作为代理的存储后端。根据目标验证关于对象不可扩展性或不可配置属性的不变量（保持不变的语义）。
```

**`Proxy.revocable()`** 方法可以用来创建一个可撤销的代理对象。细节见<a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy/revocable">此处</a>。

```
Proxy.revocable(target, handler);
```

