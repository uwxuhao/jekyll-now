---
layout: post
title: JavaScript Inheritance
---

## What is prototype chaining? ##

When the prototype of a function is an instance of another type, and for the another type, its prototype is an instance of the third type......This forms a prototype chaining.

## How the inheritance works with prototype chaining? ##

When a property is accessed on an instance, the property is searched in that instance. If there is no such property in that instance, the search will continue on its prototype. That search will continue up the prototype chaining.

## How to use the prototype chaining to implement inheritance? ##

```javascript
function SuperType(){
    this.property=true;
}

SuperType.prototype.getSuperValue = function (){
    return this.property;
}

function SubType(){
    this.subProperty=false;
}

SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function (){
    return this.subProperty;
}

var instance = new SubType();
console.log(instance.getSuperValue());

SubType.prototype.getSuperValue = function () {
    return false;
}

console.log(instance.getSubValue());
console.log(instance.getSuperValue());
```

## How to use inheritance? ##

Combination inheritance: use prototype chaining to inherit propertied and methods on the prototype and use constructor stealing to inherit instance properties.

```javascript
function SuperType(name) {
    this.name = name;
    this.colors = ["red", "yellow"];
}

SuperType.prototype.sayName = function(){
    console.log(this.name);
}

function SubType(name, age) {
    SuperType.call(this, name);
    this.age=age;
}

SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function(){
    console.log(this.age);
}
```

Parasitic Combination Inheritance


```javascript
function object(o) {
    function F(){}
    F.prototype = o;
    return new F();
}


function inheritPrototype(SubType, SuperType){
    var prototype = object(SuperType.prototype);
    SubType.constructor = SubType;
    SubType.prototype = prototype;
}

function SuperType(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function(){
    console.log(this.name);
}

function SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}

inheritPrototype(SubType, SuperType);
SubType.prototype.sayAge = function(){
    alert(this.age);
};
instance = new SubType("hao", 25);
instance.sayName();
```


