---
title: JavaScript学习笔记-列表
date: '2015-05-29'
description:
categories:
- JavaScript
tags:
- JavaScript
---

#### JavaScript学习笔记-列表
```
function List(){
  this.listSize = 0;
  this.pos = 0;
  this.dataStore = [];
  this.clear = clear;
  this.find = find;
  this.toString = toString;
  this.insert = insert;
  this.append = append;
  this.remove = remove;
  this.front = front;
  this.end = end;
  this.prev = prev;
  this.next = next;
  this.length = length;
  this.currPos = currPos;
  this.moveTo = moveTo;
  this.getElement = getElement;
  this.contains = contains;
}

function clear(element){
  delete this.dataStore;
  this.dataStore = [];
  this.listSize = this.pos = 0;
}

function insert(element,after){
  var index = this.find(element);
  if(index > -1){
    this.dataStore.splice(index + 1, 0, element);
    ++this.listSize;
    return true;
  }
  return false;
}

function front(){
  this.pos = 0;
}

function end(){
  this.pos = this.listSize - 1;
}

function prev(){
  --this.pos;
}

function next(element){
  ++this.pos;
}

function currPos(){
  return this.pos;
}

function moveTo(position){
  this.pos = position;
}

function getElement(){
  return this.dataStore[this.pos];
}

function contains(element){
  if(this.find(element) > -1)
  {
    return true;
  }
  return false;
}
function find(element){
  return this.dataStore.indexOf(element);
}

function append(element){
  this.dataStore[this.listSize++] = element;
}

function remove(element){
  var index = this.find(element);
  if(index > -1){
    this.dataStore.splice(index,1);
    --this.listSize;
    return true;
  }
  return false;
}

function length(){
  return this.listSize;
}

function toString(){
  return this.dataStore;
}

var names = new List();
names.append("Clayton");
names.append("Raymond");
names.append("Cynthia");
names.append("Jennifer");
names.append("Bryan");
names.append("Danny");

for(names.front(); names.currPos() < names.length(); names.next()) {
  console.log(names.getElement());
}

for(names.end(); names.currPos() >= 0; names.prev()) {
  console.log(names.getElement());
}

```