---
layout: post
title: JavaScript Closures
---

## Review of scope chain ##
When a function is called, its execution context is created, together with the scope chain. The front end on its scope chain is the activation object, which is initialized by the _arguments_ and the any named arguments. The outer function's activation object is add as the second object on the scope chain. This procedure goes on until the global execution context.

As the function executes, variables are looked up in the scope chain for reading or writing values.

When a function is defined, its scope chain is created, preloaded with the global variable object, and saved to the internal _[[Scope]]_ property. When the function is called, an execution context is created and its scope chain is built up by copying the objects in the function's _[[Scope]]_ property. After that, an activation object is created and push to the front end of the scope chain.

## What is closure ##
A function defined in another function adds the outer function's activation object into its scope chain. Closure is that the inner function has access to the variables and function of the outer function after the outer function is finished. The reason behind this ability is the scope chain.

### Example 1 ###
```javascript
var name = "The Window";
                   
var object = {
    name : "My Object",
                   
    getNameFunc : function(){
        return function(){
            return this.name;
        };
    }
};
                   
console.log(object.getNameFunc()()); // output is "The Window"

```
The output of the code is "The Window". That is because the function returned by the getNameFunc() is actually

```javascript
function(){
    return this.name;
}
```
When that function is called in the global environment, _this_ is actually global (_Window_). So the output is "The Window".

### Example 2 ###
```javascript
var name = "The Window";

var object = {
   name : "My Object",

   getName: function(){
       return this.name;
   }
};

(object.getName = object.getName)(); //output "The Window"

```

The output of the code is "The Window". We can view the code
```javascript
(object.getName = object.getName)();
```
as 
```javascript
var anotherFunction = object.getName;
anotherFunction();
```

When the _anotherFunction_ is called, the variable _this_ is actually _Window_.

### Example 3 ###
```javascript
var gLogNumber, gIncreaseNumber, gSetNumber;
function setupSomeGlobals() {
  // Local variable that ends up within closure
  var num = 42;
  // Store some references to functions as global variables
  gLogNumber = function() { console.log(num); }
  gIncreaseNumber = function() { num++; }
  gSetNumber = function(x) { num = x; }
}

setupSomeGlobals();
gIncreaseNumber();
gLogNumber(); // 43
gSetNumber(5);
gLogNumber(); // 5

var oldLog = gLogNumber;

setupSomeGlobals();
gLogNumber(); // 42

oldLog() // 5
```

The reason _oldLog()_'s output is 5 is because after the second _setupSomeGlobals()_, the _gLogNumber_, _gIncreaseNumber_, _gSetNumber_ get totally new values(functions). The new closure is created. The _num_ is 42. But the _oldLog_ keeps the old closure. So the output of _oldLog()_ is still 5.



