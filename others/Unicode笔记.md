>源地址 www.zcfy.cc/article/what-every-javascript-developer-should-know-about-unicode-1303.html

平面0比较特殊，被称为基本多文种平面或简称BMP。它包含了大多数现代语言的字符 (基本拉丁字母, 西里尔字母, 希腊字母等)和大量的符号。
BMP之后的16个平面（平面1，平面2，…，平面16）被称为星光平面或辅助平面。
星光平面的代码点被称为星光代码点。这些代码点的取值范围是从U+10000到U+10FFFF。
星光代码点可能会有5位或6位16进制数字：U+ddddd或U+dddddd。

字符编码将抽象层面的代码点转换为物理层面的比特序列：码元。
常用的字符编码有UTF-8, UTF-16 和 UTF-32

UTF-16（全称：16位统一码转换格式）是一种变长编码:
BMP中的代码点编码为单个16位的码元
星光平面的代码点编码为两个16位的码元

BMP中的代码点刚好能存进一个16位的码元。
编码一个星光代码点需要两个码元：即一个代理对。
高位代理码元的取值范围是从0xD800到0xDBFF。 低位代理码元的取值范围是从0xDC00到0xDFFF。

å在丹麦语书写系统中是一个不可再分的字素。但它是用U+0061 LATIN SMALL LETTER A (渲染为a) 结合一个特殊字符U+030A COMBINING RING ABOVE（渲染为◌̊）来显示的。
U+030A用来修饰前一个字符，这种字符称为组合用字符。
```javascript
console.log('\u0061\u030A'); // => 'å'  
console.log('\u0061');       // => 'a'
```

###16进制转义序列
最简短的形式称为16进制转义序列：\x<hex>. \x为前缀，后面跟一个2位的16进制数。
某些字符需要2个以上的码元来表示。所以在计算字符长度或通过字符串索引访问字符时要格外小心。
比如'\x30'（字符 '0'）和'\x5B'（字符 '['）。
```javascript
let smile = '\uD83D\uDE00';  
console.log(smile);        // => ''  
console.log(smile.length); // => 2
 
let letter = 'e\u0301';  
console.log(letter);        // => 'é'  
console.log(letter.length); // => 2
```

###Unicode转义序列
如果你想转义整个BMP中的代码点，那就用Unicode转义序列。转义形式是\u<hex>,\u为前缀，后面跟一个4位的16进制数。
比如 '\u0051' （字符 'Q'）和'\u222B' （积分符号 '∫'）.
```javascript
var str = 'I\u0020learn \u0055nicode';  
console.log(str);                 // => 'I learn Unicode'  
var reg = /\u0055ni.*/;  
console.log(reg.test('Unicode')); // => true
```
对于星光
```javascript
var str = 'My face \uD83D\uDE00';  
console.log(str); // => 'My face '
```

###代码点转义序列
ECMAScript 2015提供了能够表示整个Unicode空间：从U+0000到U+10FFFF，也就是BMP与星光平面的转义序列。
这种新格式被称为代码点转义序列：\u{<hex>}，<hex>是一个长度为1至6位的16进制数。 比如'\u{7A}'（字符'z'）和'\u{1F639}'（Funny cat符号）。
```javascript
var str = 'Funny cat \u{1F639}';  
console.log(str);                      // => 'Funny cat '  
var reg = /\u{1F639}/u;  
console.log(reg.test('Funny cat ')); // => true
```
注意正则表达式/\u{1F639}/u有一个特殊flagu,它支持额外的Unicode特性
```javascript
var niceEmoticon = '\u{1F607}';  
console.log(niceEmoticon);   // => ''  
var spNiceEmoticon = '\uD83D\uDE07'  
console.log(spNiceEmoticon); // => ''  
console.log(niceEmoticon === spNiceEmoticon); // => true
```

