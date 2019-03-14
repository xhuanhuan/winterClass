##### 类型

- Udefined

  表示未定义，值为`undefined`。

  全局变量`undefined`为只读，值为`undefined`不可修改；

  建议使用`void 0`获取`undefined`值

- Null

  表示空，值为`null`

  是关键字，任何代码中都可以用`null`关键字来获取`null`值

- Boolean

  值`true`或`false`

- String

  将每个`UTF16`单元当做一个字符，处理超过BMP范围的字符时应注意；

  最大长度`2^53 - 1`，但是由于是`UTF16`编码，实际字符串的最大长度受编码长度影响；

  基本字符区域（BMP）（`U+0000`~`U+FFFF`）（0-65535）；

  [简介`UTF8`、`UTF16`编码](https://juejin.im/post/5ace27c96fb9a028dc416195)

  | Unicode码范围        | UTF-8编码方式                       |
  | -------------------- | ----------------------------------- |
  | `U+0000`~`U+007F`    | 0????????                           |
  | `U+0080`~`U+07FF`    | 110????? 10??????                   |
  | `U+0800`~`U+FFFF`    | 1110???? 10?????? 10??????          |
  | `U+10000`~`U+10FFFF` | 11110??? 10?????? 10?????? 10?????? |

  例

  ```
  U+0020，这个字符的小于0000 007F，所以只需要用1 Byte来进行编码。U+0020的二进制表示为0000(0)0000(0) 0010(2)0000(0)，那么从后往前截取7位得到010 0000，放入UTF-8编码方式中，得到的结果为00101111，转换为十六进制得到2F。因此存储在内存中的的顺序就是2F。
  U+A12B，这个字符大于0000 0800，小于0000 FFFF，因此需要用3 Byte来进行编码。U+A12B的二进制表示为1010(A)0001(1) 0010(2)1011(B)。，那么从后往前截取16位得到10100001 00101011（Unicode码本身），放入UTF-8编码中，得到的结果为11101010 10000100 10101011，转换十六进制得到EA84AB。因此，存储在内存中的顺序就是EA 84 AB
  ```

  | Unicode码范围        | UTF-16编码方式                                               |
  | -------------------- | ------------------------------------------------------------ |
  | `U+000`~`U+FFFF`     | 2 Byte存储，编码后等于Unicode值                              |
  | `U+10000`~`U+10FFFF` | 4 Byte存储，现将Unicode值减去（0x10000），得到20bit长的值。再将Unicode分为高10位和低10位。UTF-16编码的高位是2 Byte，高10位Unicode范围为`0`-`0x3FF`，将Unicode值加上`0XD800`，得到高位代理（或称为前导代理，存储高位）；低位也是2 Byte，低十位Unicode范围一样为`0`~`0x3FF`，将Unicode值加上`0xDC00`,得到低位代理（或称为后尾代理，存储低位） |

  例

  ```
  U+0020，这个值的范围在第一部分，即经过UTF-16编码后，结果仍然为U+0020，在内存中的顺序为00 20。
  U+12345, 这个值的范围在第二部分，因此需要先减去0x10000，得到0x02345，拆分成高10位00 0000 1000和低10位11 0100 0101。根据上面规则加上特定值后，高位代理值为D808，低位代理值为DF45，最终内存中的顺序为D8 08 DF 45。
  ```

- Number

  总共有`2^64-2^53+3 = 18437736874454810627`个值；

  `NaN`, 占用`9007199254740990`;

  `Infinity`, 无穷大；

  `-Infinity`, 负穷大；

  整数范围`-0x1fffffffffffff` ~ `0x1fffffffffffff`,即`-2^53-1 ~ 2^53-1`;

  浮点数的精度导致有时候无法用 `==` or `=== ` 来比较，例：

  ```
  console.log( 0.1 + 0.2 == 0.3); // false
  console.log( Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON); // true
  ```

  常用属性

  ```
  Number.EPSILON // 两个可表示(representable)数之间的最小间隔。
  Number.MAX_SAFE_INTEGER // JavaScript 中最大的安全整数 (2^53 - 1),最小的安全整数-MAX_SAFE_INTEGER。
  Number.MAX_VALUE // 能表示的最大正数。最小的负数是-MAX_VALUE。
  Number.MIN_VALUE // 表示的最小正数即最接近 0 的正数;最大的负数是 -MIN_VALUE。
  Number.NaN // 特殊的“非数字”值。
  Number.NEGATIVE_INFINITY //特殊的负无穷大值-Infinity，在溢出时返回该值。
  Number.POSITIVE_INFINITY // 正无穷大值，在溢出时返回改值。
  Number.prototype // Number 对象上允许的额外属性。
  ```



- Symbol

  使用全局`Symbol`函数来创建`Symbol`对象，不支持语法`new`;

  每个从`Symbol()`返回的symbol值都是唯一的；

  ```
  Symbol(1) == Symbol(1); // false
  ```

  利用`Symbol.iterator`属性值来for...of行为；

  ```
  var o = new Object
  o[Symbol.iterator] = function() {
      var v = 0
      return {
          next: function() {
              return { value: v++, done: v > 10 }
          }
      }        
  };
  for(var v of o) {
      console.log(v); // 0 1 2 3 ... 9
  }
  ```

- Object

  `Number`，`String`，`Boolean`跟`new`搭配产生对象，`Symbol`无需`new`，仍是对象构造器；


##### 类型转换

- StringToNumber

  parseInt，parseFloat，xxx - '0'，xxx / 1，+xxx

- NumberToString

- 装箱转换

  Number`，`String`，`Boolean`跟`new`搭配产生对象；

  Symbol采用call或Object函数来装箱：

  ```
  var symbolObject = (function(){ return this; }).call(Symbol("a"));
  console.log(typeof symbolObject); // object
  console.log(symbolObject instanceof Symbol); // true
  console.log(symbolObject.constructor == Symbol); // true
  
  var symbolObject = Object(Symbol("a"));
  console.log(Object.prototype.toString.call(symbolObject)); // [object Symbol]
  ```

  call 本身会产生装箱操作，所以需要配合 (`typeof` 来区分对象类型)和(基本类型`toString`）

- 拆箱转换

  ToPrimitive 函数是对象类型到基本类型的转换;

  拆箱转换会调用valueOf和toString来获取拆箱后的基本类型,若valueOf和toString不存在或没有返回基本类型，则会TypeError。可以使用@@toPrimitive Symbol来覆盖原有行为。

  ```
  var o = {
      valueOf : () => {console.log("valueOf"); return {}},
      toString : () => {console.log("toString"); return {}}
  }
  
  o * 2
  // valueOf
  // toString
  // TypeError
  
  o[Symbol.toPrimitive] = () => {console.log("toPrimitive"); return "hello"}
  
  o + ""
  // toPrimitive
  // hello
  
  ```

|         |   Null    |  Undefined  | Boolean(true) | Boolean(false) |     Number     |     String     |  Symbol   |  Object  |
| :-----: | :-------: | :---------: | :-----------: | :------------: | :------------: | :------------: | :-------: | :------: |
| Boolean |   false   |    false    |               |                | 0/NaN - false  |   "" - false   |   true    |   true   |
| Number  |     0     |     NaN     |       1       |       0        |                | StringToNumber | TypeError | 拆箱转换 |
| String  |  "null"   | "undefined" |    "true"     |    "false"     | NumberToString |                | TypeError | 拆箱转换 |
| Object  | TypeError |  TypeError  |   装箱转换    |    装箱转换    |    装箱转换    |    装箱转换    | 装箱转换  |          |



- `typeof`的结果和运行时类型的规定有不一致的情况

![typeof的结果和运行时类型的规定有不一致的情况](https://static001.geekbang.org/resource/image/ec/6b/ec4299a73fb84c732efcd360fed6e16b.png)