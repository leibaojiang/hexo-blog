---
title: JavaScript学习笔记-栈
date: '2015-05-29'
description:
categories:
- JavaScript
tags:
- JavaScript
---

### 什么是栈

栈是一种被称为后入先出(LIFO,last in first out)的数据结构.是一个特殊的列表,栈内的元素只能通过一端访问,这个端被称为栈顶.
### 如何定义栈
- 方法:将一个元素压入栈push(element)
- 方法:将一个元素弹出栈pop()->element   被弹出的元素从栈中永久性删除
- 方法:预览栈顶元素peek()->element         只返回栈顶元素,不删除该元素
- 方法:清除栈内所有元素clear()   可以将top=0快速清空栈
- 方法:栈内元素的个数:length()->number //可以使用top获取
- 属性:栈顶元素位置number:top
- 属性:栈内是否有元素boolean:empty  //可以使用top=0进行判断,

<!--more-->

```
function Stack(){
    this.dataStore = [];
    this.top = 0;
    this.push = function(element){
        this.dataStore[this.top++] = element;
    };
    this.pop = function(){
        return this.dataStore[--this.top];
    };
    this.peek = function(){
        return this.dataStore[this.top - 1];
    };
    this.length = function(){
        return this.top;
    };
    this.clear = function(){
        this.top = 0;
    };
}
```
### 使用栈
```
var  s = new Stack();
s.push('iDarker');
s.push('xiachuan');
s.push('qianxia');
console.log('length:' + s.length());    //length:3
console.log('peek:' + s.peek());           //peek: qianxia
var poped = s.pop();
console.log('poped :' + poped);    //poped : qianxia
console.log('peek:' + s.peek());      //peek: xiachuan
s.push('love');
console.log('peek:' + s.peek());     //peek: love
s.clear();
console.log('length:' + s.length()); //length:0
console.log('peek:' + s.peek());      //peek:undefined
```
### 栈能解决什么问题
**使用栈进行进制转换**

- 十六进制转为十进制  (这个问题不应该用栈解决,数组本身比这更好,想岔了 (ˇˍˇ))

```
function Hex2Dec(hex){
    var chars = hex.toUpperCase().split('').reverse(); //统一转换为大写-->分割成字符数组-->倒序排列
    var stack = new Stack();
    chars.forEach(function(char){
        var code = char.charCodeAt(); //获取字符串第一个元素的ascii码(实际上是Unicode码)
        var num = 0;
        if(code > 64){                     //判断是否大于'A'=65
            num = code - 55;           //'A'->10  'B'->11
            stack.push(num);
        }else{
            num = code - 48;           //char('0')=ascii(48)
            stack.push(num);
        }
    });
    var converted = 0;
    while(stack.length()>0){
        converted = converted*16  +  stack.pop();
    }
    return converted;
}
console.log(Hex2Dec('123'));   // 291
console.log(Hex2Dec('ABC')); //2748
console.log(Hex2Dec('121ABCcd')); // 303742157
```
- 十进制转为十六进制/二进制/8进制

```
function convert(num,base){
    var s = new Stack();
    do{
        s.push(num % base);
        num = Math.floor(num / base);
    }while(num > 0);
    
    var converted ="";
    while(s.length() > 0){
        var poped = s.pop();
        if(poped > 9){
             converted += String.fromCharCode(poped+55);
        }else{
            converted += poped;
        }
    }
    return converted;
}
console.log(convert(303742157,16)); //121ABCCD
console.log(convert(100,16)); //64
console.log(convert(100,10));  //100
console.log(convert(100,8));   //144
console.log(convert(100,2));  //1100100
console.log(convert(303742157,20)); //4EI7F7H (ˇˍˇ) ～
```
- 数组的reverse函数

```
Array.prototype.myReverse = function(){
    var s = new Stack();
    this.forEach(function(element){
        s.push(element);
    });
    
    for(var i = 0;i< this.length;i++){
        this[i] = s.pop();
    }
    return this;
}

var nums = [1,2,3,4,5];
console.log(nums.myReverse()); // [ 5, 4, 3, 2, 1 ]
console.log(nums);                      // [ 5, 4, 3, 2, 1 ]
```

- 判断字符串是否为回文
回文是指这样一种现象： 一个单词、 短语或数字， 从前往后写和从后往前写都是一样的。

```
function isPalindrome(word) {
    var s = new Stack();
    for (var i = 0; i < word. length; ++i) {
        s. push(word[i] );
    }
    var rword = "";
    while (s. length() > 0) {
        rword += s. pop();
    }
    if (word == rword) {
        return true;
    }else {
        return false;
    }
}

var word = "hello" ;
if (isPalindrome(word)) {
    console.log(word + " is a palindrome. " );
}else {
    console.log(word + " is not a palindrome. " );
}
word = "racecar"
if (isPalindrome(word)) {
    console.log(word + " is a palindrome. " );
}else {
    console.log(word + " is not a palindrome. " );
}
```
- 用栈实现函数的递归调用
求阶乘5! = 5*4*3*2*1 = 120

```
function  factorial(n){
    if(n == 0){
        return 1;
    }else{
        return n * factorial(n-1);
    }
}

function fact(n){
    var s = new Stack();
    while(n>1){
        s.push(n--);
    }
    var product = 1;
    while(s.length() > 0){
        product *= s.pop();
    }
    return product;
}

console.log(factorial(5));
console.log(fact(5));
```