---
title: JavaScript学习笔记-数组
date: '2015-05-29'
description:
categories:
- JavaScript
tags:
- JavaScript
---

### 什么是数组
通常数组是一种线性结构，但是JavaScript中的数组是一个**特殊的对象**，其索引值在内部被转换成字符串处理。
### 创建数组
```
var array = [];   //length = 0
var array = [10]; //length = 1
var array = [1,2,3,4,5];//length = 5
var array = new Array(); //length = 0
var array = new Array(10); //length = 10
var array = new Array(1,2,3,4,6); //length = 5
```
*使用[]创建数组的效率高，因为字符少。*

<!--more-->

### 遍历数组
```
var nums = [1,3,4,5,6];
for(var i = 0; i < nums.length; ++i){
    console.log(nums[i]);  //1 23 4 5 6
}

for(var i = 0; i < nums.length; ++i){
    nums[i] = i;
}
```
length属性反映的是当前数组中元素的个数，使用它，可以确保循环遍历了数组中的所有元素。

### 字符串生成数组
```
var sentence = "the quick brown fox jumped over the lazy dog";
var words = sentence.split(" ");
for (var i = 0; i < words.length; ++i) 
    console.log("word " + i + ": " + words[i]);
}
```
### 数组的类方法
```
bool = Array.isArray(var);//检查变量是否为数组
```
### 数组的属性
**获取长度**
```
var nums = [1,2,3,4,5];
length = array.length;//length = 5
```

### 数组的对象方法

**查找元素**
```
/*
index:
查找到返回下标
查找失败返回-1
*/
index = array.indexOf(value);//从前往后找元素value，找到1个就返回
index = array.lastIndexOf(value);// 从后往前找元素value，找到1个就返回
```
**数组转字符串**
```
string = array.join();  //1,2,3,4,5
string = array.toString();  //1,2,3,4,5
string = array.join('-');//1-2-3-4
```

**拼接数组**
```
var a = [1,2,3];
var b = [4,5,6];

var c = a.concat(b);
//c = [ 1, 2, 3, 4, 5, 6 ]
var c = b.concat(a);
//c = [ 4, 5, 6, 1, 2, 3 ]
```
**添加元素**
push和unshift都返回数组的新长度.
```
//push方法在数组的尾部添加元素
//unshift方法在数组的头部添加元素
var array = [1,2,3];
array.push(4);//[1,2,3,4]   rtn = length =  4
array.unshift(0);//[0,1,2,3,4] rtn = length =  5
```

**删除元素**
pop和shift都返回被删除元素组成的数组
```
//pop方法在数组的尾部删除元素
//shift方法在数组的头部删除元素
var array = [0,1,2,3,4];
array.pop();//[0,1,2,3]   rtn = 4
array.shift();//[1,2,3] rtn= 0
```
**splice操作**
被删除元素数组 =  splice(起始位置, 删除字符数,  插入的字符...)
- 起始索引（也就是你希望开始添加元素的地方） ；
- 需要删除的元素个数（添加元素时该参数设为 0） ；
- 想要添加进数组的元素。

先**删除**元素,再**增加**元素.
```
var nums = [1,2,3,4,5];
nums.splice(2,0);//rtn = [] nums = [ 1, 2, 3, 4, 5 ]
nums.splice(2,1);//rtn =[ 3 ] nums = [ 1, 2, 4, 5 ]
nums.splice(2,2);//rtn = [ 4, 5 ]  nums = [ 1, 2 ]
nums.splice(2,0,10,20,30);//rtn = []  nums = [ 1, 2, 10, 20, 30 ]
nums.splice(1,1,40);//rtn = [ 2 ]  nums = [ 1, 40, 10, 20, 30 ]
```
**数组排序**
reverse方法是将数组反转
```
var array = [1,2,3,4,5];
array.reverse();//rtn = array =  [ 5, 4, 3, 2, 1 ];
```
sort方法默认是按照字典顺序进行排序
适用于字符串排序,对于数字排序不适用.
*数组本身被改变*
Array = sort([function compare]);
```
var words = ['one','two','three','four'];
words.sort();//[ 'four', 'one', 'three', 'two' ]
var nums = [4,5,2,20,11,1,10,11,23,42];
nums.sort();//[ 1, 10, 11, 11, 2, 20, 23, 4, 42, 5 ]
nums.sort(function(a,b){return a-b; });//[ 1, 2, 4, 5, 10, 11, 11, 20, 23, 42 ]
nums.sort(function(a,b){return b-a; });//[ 42, 23, 20, 11, 11, 10, 5, 4, 2, 1 ]
```
### 迭代器
forEach() 该方法接受一个函数作为参数,对数组中的每个元素使用该函数.
```
function square(num){
    console.log(num, num* num);
}

var nums = [1,2,3,4,5];
nums.forEach(square);
```
every()， 该方法接受一个返回值为布尔类型的函数， 对数组中的每个元素使用该函数。 如果对于所有的元素， 该函数均返回 true， 则该方法返回 true。 
```
var nums = [2,4,6,8,10];
var rtn = nums.every(function(num){
    return num%2==0;
});
console.log(rtn); // true
```
some() 方法也接受一个返回值为布尔类型的函数， 只要有一个元素使得该函数返回 true，该方法就返回 true。
```
var nums = [2,3,5,7,9];
var rtn = nums.some(function(num){
    return num%2==0;
});
console.log(rtn); // true
```
reduce() 方法接受一个函数， 返回一个值。 该方法会从一个累加值开始， 不断对累加值和
数组中的后续元素调用该函数， 直到数组中的最后一个元素， 最后返回得到的累加值。
计算数组中元素的平方和.
```
var nums = [1,2,3,4];
var sum = nums.reduce(function(sum,num){
    return sum + num*num;
});
console.log(sum);//30  1*1 + 2*2 + 3*3 + 4*4 =30
```
reduceRight() 方法， 和 reduce() 方法不同， 它是从右到左执行。 
```
function concat(accumulatedString, item) {
    return accumulatedString + item;
}
var words = [" the " , " quick " , " brown " , " fox " ] ;
var sentence = words. reduceRight(concat);
console.log(sentence); // 显示 " fox brown quick the"
```
map() 和 forEach() 有点儿像， 对数组中的每个元素使用某个函数。 两者的区别是 map() 返回一个新的数组， 该数组的元素是对原有元素应用某个函数得到的结果。
```
function square(num){
    return num* num;
}

var nums = [1,2,3,4,5];
var news = nums.map(square); //rtn = [ 1, 4, 9, 16, 25 ]
console.log(nums);//[ 1, 2, 3, 4, 5 ]
console.log(news);//[ 1, 4, 9, 16, 25 ]
```
给定一个字符串生成首字母缩写形式
```
var str = 'Asynchronous Javascript And XML';
var arr = str.split(" ");
var newArr = arr.map(function(word){return word.charAt(0);});
var newStr = newArr.join("");
console.log(str + '->' + newStr); // Asynchronous Javascript And XML->AJAX
```

filter() 和 every() 类似， 传入一个返回值为布尔类型的函数。 和 every() 方法不同的是，当对数组中的所有元素应用该函数， 结果均为 true 时， 该方法并不返回 true， 而是返回一个新数组， 该数组包含应用该函数后结果为 true 的元素。
随机生成20个数字,取出大于60的数字.
```
var nums = [];
for(var i=0;i<20;i++){
    nums[i] = Math.floor(Math.random()*101);
}
var bigNums = nums.filter(function(num){return num >= 60;});
console.log(nums);//[ 46, 56, 53, 65, 49, 3, 25, 26, 46, 25, 36, 73, 44, 43, 58, 97, 100, 65, 97, 47 ]
console.log(bigNums);//[ 65, 73, 97, 100, 65, 97 ]
```

```
var words = ["recieve" , " deceive" , "percieve" , "deceit" , "concieve" ] ;
var filter = 'cie';
var misspelled = words.filter(function(word){
    if(word.indexOf(filter)>-1){
        return true;
    }else{
        return false;
    }
});
console.log(misspelled );//[ 'recieve', 'percieve', 'concieve' ]
```