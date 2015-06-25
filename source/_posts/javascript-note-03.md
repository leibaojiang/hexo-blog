---
title: JavaScript学习笔记-数组练习题
date: '2015-05-29'
description:
categories:
- JavaScript
tags:
- JavaScript
---

#### 1.  创建一个记录学生成绩的对象，提供一个添加成绩的方法，以及一个显示学生平均成绩的方法。
```
function Grade(chinese,english,math){
  this.dataStore = [chinese,english,math]
  this.average = function(){
    var total = this.dataStore.reduce(function(currenTotal,currenNum){
      return currenTotal + currenNum;
    });
    return total/this.dataStore.length;
  }
}

function Grades(){
  this.dataStore = [];
  this.add = function(grade){
    this.dataStore.push(grade);
  };
  this.average = function(){
    var averages = this.dataStore.map(function(grade){
      return grade.average();
    });
    console.log(averages);
    var total = averages.reduce(function(currenTotal,currenNum){
      return currenTotal + currenNum;
    });
    return total/this.dataStore.length;
  };
  
  this.englishAverage = function(){
    var englishs = this.dataStore.map(function(grade){
      return grade.dataStore[0];
    });
    console.log(englishs);
    var total = englishs.reduce(function(currenTotal,currenNum){
      return currenTotal + currenNum;
    });
    return total/this.dataStore.length;
  };
}

var g1 = new Grade(90,98,95);
var g2 = new Grade(83,79,82);
var g3 = new Grade(46,53,22);

var grades = new Grades();
grades.add(g1);
grades.add(g2);
grades.add(g3);

console.log(grades.average());
console.log(grades.englishAverage());

```

<!--more-->

#### 2.  将一组单词存储在一个数组中，并按正序和倒序分别显示这些单词。

```
var words = ['one','two','three','four'];
words.sort();
console.log(words);
words.reverse();
console.log(words);
```
#### 3.  创建这样一个对象，它将字母存储在一个数组中，并且用一个方法可以将字母连在一起，显示成一个单词。
```
function Word(){
  this.dataStore = [];
  this.add = function(char){
    this.dataStore.push(char);
  };
  this.toString = function(){
    return this.dataStore.join("");
  }
}

var word = new Word();
word.add('g');
word.add('o');
word.add('o');
word.add('d');
console.log(word.toString());
```