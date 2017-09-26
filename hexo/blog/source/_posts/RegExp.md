---
title: 正则表达式学习总结
date: 2017-08-30 19:57:31
tags:
    - RegExp
---
正则表达式的基本语法：
```js
var expression = /pattern(模式)/flags(标识符);
```
pattern(模式)可以是由字符类、限定符、分组、向前查找以及反向引用。flags(标识符)取值为：i(不区分大小写)，g(全局匹配)，m(多行匹配),同一个正则表达式可以带有一个或多个flags。

### 创建正则表达式
#### 字面量创建
```js
//匹配字符串中所有“at”的实例
var e = /at/g;
//匹配第一个“bat”或“cat”,不区分大小写
var e = /[bc]at/i;
```
#### RegExp构造函数创建
RegExp构造函数接收两个参数，第一个参数是要匹配的字符串模式，第二个是可选的标识符字符串。
```js
//匹配第一个“bat”或“cat”,不区分大小写
var e = new RegExp("[bc]at","i");
```
两种创建方式的比较：
>在ECMAScript3中，字面量创建和RegExp对象创建区别是：字面量创建始终会共享同一个RegExp实例，而构造函数创建的每一个RegExp实例都是一个新实例  
> ECMAScript5明确规定：使用正则表达式字面量必须像直接调用RegExp构造函数一样，每次都创建新的RegExp实例。IE9+、Firefox 4+和Chrome都做出了修改。

正则表达式中的元字符必须转义。元字符有：
```js
( [ { \ ^ $ | ) ? * + . ] }
```
例如：
```js
//匹配第一个“[bc]at”,不区分大小写
var e = /\[bc\]at/i;
//在RegExp构造函数创建时，元字符需要双重on转义
var e = new RegExp("\\[bc\\]at","i");
```
### RegExp实例的属性和方法
#### RegExp实例属性

```sh
ignoreCase 返回布尔值，表示RegExp对象是否具有标志 i
global 返回布尔值，表示RegExp对象是否具有表示 g
multiline 返回布尔值，表示RegExp对象是否具有表示 m
lastIndex 一个整数，标识开始下一次匹配的字符位置
soure 返回正则表达式的原文本 （不包括反斜杠）
i 执行对大小写不敏感的匹配
g 执行全局匹配 （查找所有匹配而非在找到第一个匹配后停止）
m 执行多行匹配
```

#### 字符类匹配

```sh
[...]查找方括号之间的任何字符
[^..]查找任何不在方括号之间的字符
[a-z]查找任何从小写a到小写z的字符
[A-Z]查找任何从大写A到大写Z的字符
[A-z]查找任何从大写A到小写z的字符
. 查找单个字符，除了换行和行结束符
\w 查找单词字符，等价于 [a-zA-Z0-9]
\W 查找非单词字符，等价于 [^a-zA-Z0-9]
\s 查找空白字符
\S 查找非空白字符
\d 查找数字，等价于[0-9]
\D 查找非数字字符，等价于[^0-9]
\b 匹配单词边界
\r 查找回车符
\t 查找制表符
\0 查找NULL字符
\n 查找换行符
```

#### 重复字符匹配
```sh
{n,m}匹配前一项至少n次，但不能超过m次
{n,}匹配前一项n次或更多次
{n}匹配前一项n次
n?匹配前一项0次或者1次，也就是说前一项是可选的，等价于{0,1}
n+匹配前一项一次或多次，等价于{1,}
n*匹配前一项0次或多次，等价于{0，}
n$匹配任何结尾为n的字符串
^n匹配任何开头为n的字符串
？=n匹配任何其后紧接指定字符串n的字符串
?!n匹配任何其后没有紧接指定字符串n的字符串
```

#### 匹配特定数字
```sh
^[1-9]\d*$ 匹配正整数
^-[1-9]\d*$ 匹配负整数
^-?[0-9]\d*$ 匹配整数
^[1-9]\d*|0$ 匹配非负整数（正整数 + 0）
^-[1-9]\d*|0$ 匹配非正整数（负整数 + 0）
^[1-9]\d*.\d*|0.\d*[1-9]\d*$ 匹配正浮点数
^-([1-9]\d*.\d*|0.\d*[1-9]\d*)$ 匹配负浮点数
^-?([1-9]\d*.\d*|0.\d*[1-9]\d*|0?.0+|0)$ 匹配浮点数
^[1-9]\d*.\d*|0.\d*[1-9]\d*|0?.0+|0$ 匹配非负浮点数（正浮点数 + 0）
^(-([1-9]\d*.\d*|0.\d*[1-9]\d*))|0?.0+|0$ 匹配非正浮点数（负浮点数 + 0）
```

#### 方法
##### exec()方法
exec()方法为模式的捕获组而设计的，该方法接收一个参数，即要匹配的字符串，该方法返回一个包含捕获组的数组Array,如果没有捕获组匹配返回null。返回的数组Array中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配到的字符串。数组Array中还有两个参数input(返回要匹配的字符串)，index(返回匹配项在字符串中的位置)  
模式中的捕获组就是指圆括号中的字符串。
exg:
```js
var e = /do(es)(d)?/;
e.exec("ssdoesdo");
/**
array[0]:"doesd",
array[1]:"es",
array[2]:"d",
index:2,
input:"ssdoesdo"
**/
```
例子中，模式中包含两个捕获组"es"、"d"，即圆括号中的字符串。

##### test()方法
test()方法检索字符串中指定的值，该方法接收一个参数，如果字符串中含有与模式匹配的文本则返回true，否则返回false。
exg:
```js
var e = /do(es)?/;
e.test("doesdo");
//true
```
如果正则表达式中带有g标识符,则每一次调用test方法和exec方法都从上一次匹配结束位置开始匹配；如果正则表达式中没有g标识符，则每次调用方法都从字符串起始位置开始匹配。
exg:
```js
var e = /do(es)?/g;
e.exec("ssdoesdoesdoes");
/*
array[0]:"does",
array[1]:"es",
index:2,
input:"ssdoesdoesdoes"
*/
e.exec("ssdoesdoesdoes");
/*
array[0]:"does",
array[1]:"es",
index:6,
input:"ssdoesdoesdoes"
*/

```
```js
var e = /do(es)?/g;
console.log(e.test("ssdoesdoesdoes"));
console.log(e.lastIndex);
//true
//6
console.log(e.test("ssdoesdoesdoes"));
console.log(e.lastIndex);
//true
//10
console.log(e.test("ssdoesdoesdoes"));
console.log(e.lastIndex);
//true
//14
```
```js
var e = /do(es)?/;
console.log(e.test("ssdoesdoesdoes"));
console.log(e.lastIndex);
//true
//0
console.log(e.test("ssdoesdoesdoes"));
console.log(e.lastIndex);
//true
//0
```
### 常用的正则校验
```js
/^1[34578]\d{9}$/ //匹配手机号
/^(([0\+]\d{2,3}-)?(0\d{2,3})-)(\d{7,8})(-(\d{3,}))?$/ //匹配座机号
/^[0-9]\d*$/  //匹配正整数
/^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$/ //匹配ip地址
/^(\w-*\.*)+@(\w-?)+(\.\w{2,})+$/  //匹配邮箱
/^(\d{14}|\d{17})(\d|[xX])$/  //匹配身份证
```
