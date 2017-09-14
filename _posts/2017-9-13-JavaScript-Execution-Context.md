---
layout: post
title: JavaScript execution context
---

## What is execution context? ##

The execution context of a function or variable defines what other data it has access to, and how it should behave. Each execution context has an assocaited variable object upon which all of its defined variables and functions exists.

### Example ###
The object for the global context is _window_. All the global variables and functions are created as properties and methods of _window_.

Each function has its own execution context. When the code execution flows into a function, the execution context of that function will be pushed into the context stack. After the function is finished, the context of that function is poped and let the previous context to take control.

## What is scope chain ##
When the code is executed in a context, the scope chain of the variable object is created. The purpose of the scope chain is to provide ordered access to all the variables and functions that an execution context has access to. The front end of a scope chain is always the variable object of the context. If the context is a function, the activation object (_arguments_, plus the variables and functions defined in the function) is used as the variable object. The next variable object in the chain is from the outside context. And it goes out until the global.

### Example ###

```javascript
var color = "blue";
function show(){
    console.log(color);
}
show();
```

First of all, the variable object in the function _show()_ is the activation object. But there is no variable called _color_ in activation object. So it searched to the outside context, global. It finds the variable _color_ in the global context.

```javascript
var color = "blue";
function show(){
    var color = "green";
    console.log(color);
}
show();
```

For the function _show()_, it will first search the variable _color_ in the variable object of the function _show()_. And it finds the _color_, then the search ends.


