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
```javascript
let smile = '\uD83D\uDE00';  
console.log(smile);        // => ''  
console.log(smile.length); // => 2
 
let letter = 'e\u0301';  
console.log(letter);        // => 'é'  
console.log(letter.length); // => 2
```

