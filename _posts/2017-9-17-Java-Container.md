---
layout: post
title: Java Container
---

## What is Java Container? ##

There are two basic interfaces in Java Container library.

* Collection: a sequence of individual elements with one or more rules applied to them.
    * List
    * Set
    * Queue
* Map: a group of key-value pairs.


## Collection and Array ##

_Collection_ and _Array_ looks similar. _Array_ can be converted into _Collection_. 

```java
import java.util.*;

class Solution {
    public static void main(String[] args) {
        Collection<Integer> collection = new ArrayList<Integer>(Arrays.asList(1, 2, 3, 4, 5));
        Integer[] more = {6, 7, 8, 9, 10};
        collection.addAll(Arrays.asList(more));

        Collections.addAll(collection, 11, 12, 13, 14, 15);
        Collections.addAll(collection, more);

        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
    }
}
```

The _Arrays.asList()_ can convert an _Array_ to _Collection_, but when using the _Arrays.asList()_, the size of the _Collection_ is fixed. That mean the user cannot change the size of the _Collection_ again. That is because the underlying representation of the list is _Array_.

Another problem about _Arrays.asList()_ is this function takes a best guess about the resulting type of the _List_, and does not pay attention to what you are assigning it to. (It seems that the problem has been fixed in Java 8)

```java
import java.util.*;

class Snow {}
class Powder extends Snow {}
class Light extends Powder {}
class Heavy extends Powder {}
class Crusty extends Snow {}
class Slush extends Snow {}

public class AsListInference {
  public static void main(String[] args) {
    List<Snow> snow1 = Arrays.asList(
      new Crusty(), new Slush(), new Powder());

    // Won't compile:
    // List<Snow> snow2 = Arrays.asList(
    //   new Light(), new Heavy());
    // Compiler says:
    // found   : java.util.List<Powder>
    // required: java.util.List<Snow>

    // Collections.addAll() doesn't get confused:
    List<Snow> snow3 = new ArrayList<Snow>();
    Collections.addAll(snow3, new Light(), new Heavy());

    // Give a hint using an
    // explicit type argument specification:
    List<Snow> snow4 = Arrays.<Snow>asList(
       new Light(), new Heavy());
  }
} ///:~
```
The _Collections.addAll()_ will not be confused because the first argument for _Collections.addAll()_ will tell the type of the _List_.

_Collection_ can also be converted to _Array_.
```java
class Solution {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list, 1, 2, 3, 4, 5);
        Integer[] array = (Integer[]) list.toArray();
        Integer[] anotherArray = list.toArray(new Integer[0]);
    }
}
```

Using _.toArray()_, the _List_ can be converted to _Array_. But if you do not add the argument in the _.toArray()_, the type of the result is _Object[]_. If the argument of the _.toArray()_ is filled, the returned type will become the type of the argument.


## Iterator ##

There are 3 API provided by Iterator. _next()_, _hasNext()_ and _remove_.

_next_ is used to get the next element in the sequence.
_hasNext()_ returns whether there is any more elements in the sequence.
_remove()_ removes the last element returned by the iterator.

