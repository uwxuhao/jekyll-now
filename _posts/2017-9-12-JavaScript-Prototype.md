---
layout: post
title: Some understanding in JavaScript Prototype
---

## What is Prototype? ##

_Prototype_ is an object. It contains properties and methods that are shared among all the instances of a particular reference type. 

Each function has a property called _prototype_. The _prototype_ actually is the prototype of all the instances created by the construction function.

## Why Prototype ##

_prototype_ is shared by all the instances created by the construction function. Using _prototype_, the instances can share the same methods instead of creating duplicated methods in the construction function.

## How Prototype works ##

When a function is created, its property _prototype_ is also created. The _prototype_ as a pointer called _constructor_ points back to the function it belongs to.
When an instance is created by the construction function, the instance will have a pointer called _[[Prototype]]_ points to the _prototype_ property of the function.

Whenever a property of an instance is accessed, a search is started for that property. The search begins on the property of the the object instance itself. If it cannot find a property with the specific name in the object instance, it will search in the _prototype_ object.

## How to use Prototype ##

Combine the constructor pattern and prototype pattern

```javascript
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
}

Person.prototype = {
    constructor: Person,
    sayName: function() {
        console.log(this.name);
    }
}
```

An improved version:
```javascript
function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;

    if (typeof this.sayName != "function") {
        Person.prototype.sayName = function() {
            console.log(this.name);
        };
    }

}
```
