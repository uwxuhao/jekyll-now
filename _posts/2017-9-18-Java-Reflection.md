---
layout: post
title: Some Understanding about Java Reflection
---

## What is reflection? ##

Reflection is commonly used by programs which require the ability to examine or modify the runtime behavior of applications running in the Java virtual machine. In another word, reflection gives programmer the ability to write code to call the methods of a class even if the class has not been compiled by the compiler. The true difference between RTTI and reflection is that with RTTI, the compiler opens and examines the _.class_ file at compile time. Put another way, you can call all the methods of an object in the "normal" way. With reflection, the _.class_ file is unavailable at compile time; it is opened and examined by the runtime environment.

## How to use reflection? ##

```java
class Solution {
    public static void main(String[] args) {
        try {
            Class<?> c = Class.forName("FamilyVsExactType");
            Method[] methods = c.getMethods();
            Constructor[] ctors = c.getConstructors();
            for (Method method : methods) {
                System.out.println(method.toString());
            }

            for (Constructor constructor : ctors) {
                System.out.println(constructor.toString());
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

//output:
//public void FamilyVsExactType.Say()
//public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException
//public final native void java.lang.Object.wait(long) throws java.lang.InterruptedException
//public final void java.lang.Object.wait() throws java.lang.InterruptedException
//public boolean java.lang.Object.equals(java.lang.Object)
//public java.lang.String java.lang.Object.toString()
//public native int java.lang.Object.hashCode()
//public final native java.lang.Class java.lang.Object.getClass()
//public final native void java.lang.Object.notify()
//public final native void java.lang.Object.notifyAll()
//public FamilyVsExactType()
//public FamilyVsExactType(int)

```

The result produced by _Class.forName()_ is unknown at compile time. All the method signature information are extracted at run time.


### Example: The dynamic proxy pattern based on reflection _invoke()_ ###
```java
interface Building {
    int getPrice();
    void buy();
}

class House implements Building {
    public int getPrice() {
        System.out.println("House price is 1000");
        return 1000;
    }

    public void buy() {
        System.out.println("Buy a house");
    }
}

class hall implements Building {
    public int getPrice() {
        System.out.println("Hall price is 5000");
        return 1000;
    }

    public void buy() {
        System.out.println("Buy a hall");
    }
}

class BuildingHander implements InvocationHandler {

    private Object target;

    public BuildingHander(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("start to call the method");
        Object res = method.invoke(target, args);
        System.out.println("end of the method");
        return res;
    }
}

class Solution {
    public static void main(String[] args) {
        Class<House> houseClass=House.class;
        Building building = (Building)Proxy.newProxyInstance(Building.class.getClassLoader(),
                               new Class[] {Building.class},
                               new BuildingHander(new House()));
        building.buy();
        System.out.println(building.getPrice());
    }
}

//output:
//        start to call the method
//        Buy a house
//        end of the method
//        start to call the method
//        House price is 1000
//        end of the method
//        1000

```

## Use reflection to access fields ##

```java
import typeinfo.interfacea.*;
import static net.mindview.util.Print.*;

public interface A { void f(); } ///:~

class C implements A {
  public void f() { print("public C.f()"); }
  public void g() { print("public C.g()"); }
  void u() { print("package C.u()"); }
  protected void v() { print("protected C.v()"); }
  private void w() { print("private C.w()"); }
}

public class HiddenC {
  public static A makeA() { return new C(); }
} ///:~
```


```java
import typeinfo.interfacea.*;
import typeinfo.packageaccess.*;
import java.lang.reflect.*;

public class HiddenImplementation {
  public static void main(String[] args) throws Exception {
    A a = HiddenC.makeA();
    a.f();
    System.out.println(a.getClass().getName());
    // Compile error: cannot find symbol 'C':
    /* if(a instanceof C) {
      C c = (C)a;
      c.g();
    } */
    // Oops! Reflection still allows us to call g():
    callHiddenMethod(a, "g");
    // And even methods that are less accessible!
    callHiddenMethod(a, "u");
    callHiddenMethod(a, "v");
    callHiddenMethod(a, "w");
  }
  static void callHiddenMethod(Object a, String methodName)
  throws Exception {
    Method g = a.getClass().getDeclaredMethod(methodName);
    g.setAccessible(true);
    g.invoke(a);
  }
} /* Output:
public C.f()
typeinfo.packageaccess.C
public C.g()
package C.u()
protected C.v()
private C.w()
*///:~
```

### Reflection can even modify the private fields ###

```java
import java.lang.reflect.*;

class WithPrivateFinalField {
  private int i = 1;
  private final String s = "I'm totally safe";
  private String s2 = "Am I safe?";
  public String toString() {
    return "i = " + i + ", " + s + ", " + s2;
  }
}

public class ModifyingPrivateFields {
  public static void main(String[] args) throws Exception {
    WithPrivateFinalField pf = new WithPrivateFinalField();
    System.out.println(pf);
    Field f = pf.getClass().getDeclaredField("i");
    f.setAccessible(true);
    System.out.println("f.getInt(pf): " + f.getInt(pf));
    f.setInt(pf, 47);
    System.out.println(pf);
    f = pf.getClass().getDeclaredField("s");
    f.setAccessible(true);
    System.out.println("f.get(pf): " + f.get(pf));
    f.set(pf, "No, you're not!");
    System.out.println(pf);
    f = pf.getClass().getDeclaredField("s2");
    f.setAccessible(true);
    System.out.println("f.get(pf): " + f.get(pf));
    f.set(pf, "No, you're not!");
    System.out.println(pf);
  }
} /* Output:
i = 1, I'm totally safe, Am I safe?
f.getInt(pf): 1
i = 47, I'm totally safe, Am I safe?
f.get(pf): I'm totally safe
i = 47, I'm totally safe, Am I safe?
f.get(pf): Am I safe?
i = 47, I'm totally safe, No, you're not!
*///:~
```
