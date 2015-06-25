---
title: JavaScript学习笔记-数组2
date: '2015-05-29'
description:
categories:
- JavaScript
tags:
- JavaScript
---

### 二维数组
JavaScript只支持一维数组，但是通过在数组里保存数组元素的方式，可以轻松创建多维
数组。
### 创建二维数组
```
var twod = [];
var rows = 5;
for (var i = 0; i < rows; ++i) {
    twod[i] = [];
}
console.log(twod);//[ [], [], [], [], [] ]
```

<!--more-->

为数组类添加方法matrix(行数,列数,初始值),用于创建二维数组并初始化.
```
Array.matrix = function(rows,cols,initial){
    var arr = [];
    for(var i = 0; i < rows; i++){
        var col = [];
        for(var j = 0; j < cols; j++){
            col[j] = initial;
        }
        arr[i] = col;
    }
    return arr;
}

var nums = Array.matrix(5,5,0);
console.log(nums[1][1]); // 0
var strs = Array.matrix(5,5,"");
strs[1][2] = "iDarker";
console.log(strs);
 /* [ [ '', '', '', '', '' ],
  [ '', '', 'iDarker', '', ''
  [ '', '', '', '', '' ],
  [ '', '', '', '', '' ],
  [ '', '', '', '', '' ] ]
*/
```
### 对象数组
```
function Point(x,y){
    this.x = x;
    this.y = y;
}
var p1 = new Point(1,1);
var p2 = new Point(2,2);
var p3 = new Point(3,3);
var p4 = new Point(4,4);
var points = [p1,p2,p3,p4];
points.forEach(function(point){
    console.log('Point  x:' + point.x + '  y:' + point.y);
});
/*
Point  x:1  y:1
Point  x:2  y:2
Point  x:3  y:3
Point  x:4  y:4
*/
var p5 = new Point(5,5);
points.push(p5);
console.log(points);
/*
[ { x: 1, y: 1 },
  { x: 2, y: 2 },
  { x: 3, y: 3 },
  { x: 4, y: 4 },
  { x: 5, y: 5 } ]
*/
points.pop();
console.log(points);
/*
[ { x: 1, y: 1 },
  { x: 2, y: 2 },
  { x: 3, y: 3 },
  { x: 4, y: 4 } ]
*/
points.unshift(p5);
console.log(points);
/*
[ { x: 5, y: 5 },
  { x: 1, y: 1 },
  { x: 2, y: 2 },
  { x: 3, y: 3 },
  { x: 4, y: 4 } ]
*/
points.shift();
console.log(points);
/*
[ { x: 1, y: 1 },
  { x: 2, y: 2 },
  { x: 3, y: 3 },
  { x: 4, y: 4 } ]
*/
var filter = new Point(2,2);
var newPoints = points.filter(function(point){
    return (point.x == filter.x) && (point.y == filter.y);
});
console.log(newPoints );
/* [ { x: 2, y: 2 } ] */
```