---
layout: post
title: Java Polymorphism
---

## What is Polymorphism? ##

Polymorphism is an ability to use one interface to represent different classes. And when the function of the instance is callled, the function of that specific class will be called.

## How does Polymorphism works? ##

The Polymorphism is based on _late binding_. For late binding, the binding occurs at run time, based on the type of the object. The compiler dose not know the type of the object but there is some information stored in the object to determine the type of the object.

### Tips ###

All the binding in Java is late binding except the static and final methods. Private methods are automatically final.

## Constructor ##

The order of constructors called:
1. The based class is called recursively such that the root of the hierarchy is constructed first, followed by the next-drived class.
2. Member of initialization is called in the order of declaration.
3. The constructor of the drived class is called.

### Polymorphism in Constructor ###

1. The storage allocated for objects are set to binary zero before anything happens.

2. The base-class constructor is called. If there is any methods in the base-class constructor that is overridden in the derived class, the overridden method will be called. 

3. Member initializers are called in the order of declaration. 

4. The derived-class constructor is called.


