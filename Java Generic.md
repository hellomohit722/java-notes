# Generics in Java
- Generics allow you to write type-safe, reusable code by parameterizing classes, interfaces, and methods with types.
- Instead of working with Object and casting everywhere, you specify the exact type at compile time.
- before generic objects are used for generalization
# Object for Generalization

Object can be used for generalization, but it has some problems:

### Problems
1. There is no type safety (e.g., storing a String, then later an Integer).
2. Typecasting is required.
3. No error on type casting
   
```java
// class object
package genericdemo;

public class GenericDemo
{
    public static void main(String[] args)
    {
        Object obj = new String("Hello");  
       // obj = new Integer(10);          // No typecasting saftey

        String str = (String)obj;         //typecasting is requird & No error if obj were integer and typecasting by String
    }
}
```
```java
//array of objects
public class GenericDemo
{
    public static void main(String[] args)
    {
        Object obj[] = new Object[3];    // problem is it can store mix of objects which create issue in like type casting

        obj[0] = "hi";
        obj[1] = "bye";
        obj[2] = new Integer(10);

        String str;

        for(int i = 0; i < 3; i++)
        {
            str = (String)obj[i];
            System.out.println(str);
        }
    }
}
```
# What actually happens — 2 stages:
## 🔵 Stage 1 — Compile Time (your .java → .class)
### The compiler:
- ✅ Uses String to check for type errors
- ✅ Inserts hidden casts automatically
- ❌ Then THROWS AWAY the type info — this is Erasure

```java
// What YOU write:
GenericBox<String> box = new GenericBox<>();
box.set("Hello");
String s = box.get();       // compiler knows it's String here

// What compiler GENERATES in bytecode (.class file):
GenericBox box = new GenericBox();  // <String> is GONE
box.set("Hello");
String s = (String) box.get();     // compiler secretly inserted this cast!
```

## 🔴 Stage 2 — Runtime (JVM runs the .class)
### JVM only sees:``
```java
class GenericBox {
    Object value;        // T became Object

    void set(Object v)  { this.value = v; }
    Object get()        { return this.value; }
}
```
**T is completely gone. JVM has no idea T ever existed.**

---

## So where does T become `String` and where does it become `Object`?
```
YOUR CODE (compile time)     →    JVM / BYTECODE (runtime)
─────────────────────────────────────────────────────────
GenericBox<String>           →    GenericBox          (String gone)
T value                      →    Object value        (T → Object)
box.get() returns String     →    box.get() returns Object
                                  + hidden (String) cast added by compiler
```
<img width="1440" height="1102" alt="image" src="https://github.com/user-attachments/assets/c06933de-e467-4359-b954-ff64e698281e" />

## Here's the simple mental model:
- At compile time — the compiler pretends T = String. It uses that info to catch your mistakes and then secretly inserts casts. Then it throws String away.
- At runtime — the JVM only sees Object (or whatever the bound is). T is completely gone from memory.

```java
// Generic type array
// This is a classic generics + arrays problem in Java.
// Java forbids new T[3] for this exact reason.
// Never create generic arrays using (T[]) new Object[].
package genericdemo;

public class GenericDemo<T>
{
    T data[] = (T[]) new Object[3];         // static is not allowed because T type is not defined yet.

    public static void main(String[] args)
    {
        GenericDemo<String> gd = new GenericDemo();   //

        gd.data[0] = "hi";
        gd.data[1] = "bye";
        gd.data[2] = new Integer(10);  // compiler give error here as only String can be use

        String str = gd.data[0];     // No need of typeCasting
    }
}
```

```java
// ✅ Use List<T> instead of array
import java.util.ArrayList;
import java.util.List;

class GenericDemo<T> {
    List<T> data = new ArrayList<>();

    public static void main(String[] args) {
        GenericDemo<String> gd = new GenericDemo<>();
        gd.data.add("hi");
        gd.data.add("bye");

        String s = gd.data.get(0);
    }
}

```
usually we do not have to create Generic class we just use
# Generic Class
## Templates are for objects, not for primitive data types.

```java
package genericdemo;
// This generic class only store one type of object
class Data<T>
{
    private T obj;

    public void setData(T v)
    {
        obj = v;
    }

    public T getData()
    {
        return obj;
    }
}

public class GenericDemo
{
    public static void main(String[] args)
    {
        Data<String> d = new Data<>();
        d.setData("Hi");

        System.out.println(d.getData());
    }
}
```
```java
// This generic class store array of object of one type only

import java.util.ArrayList;
import java.util.List;

class MyArray<T>
{
    List<T> list = new ArrayList<>();
    int length = 0;

    public void append(T v)
    {
        length++;
        list.add(v);
    }

    public void display()
    {
        for(int i=0; i<length; i++)
        {
            System.out.println(list.get(i));
        }
    }
}
class genericdemo {
    public static void main(String[] args) {
        MyArray<Integer> ma = new MyArray<>();

        ma.append(10);
        ma.append(20);
        ma.append(30);

        ma.display();
    }
}
```


### Notes on Generics
1. If no ParameterType use for the generic class, it will be treated like Object `MyArray ma = new MyArray<>()`.
2. Multiple parameters (generally used for key-value pairs & if no ParameterType is present then it will treat like object).
3. Subtype:
   
   (a) if Parent ParameterType while extending subclass is mentioned then subclass only store that type else any object type `class MyArray2 extends MyArray<String>`.
   
   (b) if you want child class extends as generic then you must make child class as generic `class MyArray2<T> extends MyArray<T>`

5. Bounded Type:
```
class MyArray<T extends Number> // now only subclass of Number is permitted, extends must be used for both class & interface
```
# Since we write Generic class we can also write generic method
as in generic class we need to write <T> (ParametricType) after className, but in generic methods we need to write <T>(ParametricType) before return type.

1. Generic Methods (Bound also works)

```java
public class GenericDemo
{
    static <E> void show(E[] list)      // static <E> void show(E... list) we can also use variable arguments
    {
        for(E x : list)
        {
            System.out.println(x);
        }
    }

    public static void main(String[] args)
    {
        show("Hi", "Go", "Bye");
        show(new Integer[]{10, 20, 30, 40});
    }
}
```
 2. Wildcard `?` : we can use it as argument list/parameter list

```java
static <T> void fun(MyArray<T> obj)       //if you want to add <T>(Parametric Type) as argument then must add <T> before return type
    {
        obj.display();
    }
```

```
public class GenericDemo
{
    static void fun(MyArray<?> obj)       //static void fun(MyArray obj) works same
    {
        obj.display();
    }

    public static void main(String[] args)
    {
        MyArray<String> ma1 = new MyArray<>();
        ma1.append("Hi");
        ma1.append("Bye");

        MyArray<Integer> ma2 = new MyArray<>();
        ma2.append(10);
        ma2.append(20);

        fun(ma1);
        fun(ma2);
    }
}
```
3. Lower Bound: `void fun(MyArray<? super child> obj)` {parent of child & child is only accepted} 
4. Upper Bound `void fun(MyArray<? extends parent> obj)` {child of parent & parent is only accepted} 

### Do’s and Don’ts in Generics

1. Only extends is allowed in Generic class definition.
2. extends is used for interfaces also.
3. extends from only one class and multiple interfaces.
   `class MyArray<T extends A & B & C>{}` A is class, B & C is interface, Here MyArray only accept that object/class if it extends A, implements B & C.
4. extends and super are allowed with ? in methods.
5. <?> will accept all types but cannot access.
```java
public class GenericDemo
{
    static void fun(MyArray<?> obj)  //`void fun(MyArray<String> obj)` or `void fun(MyArray<? extends object> obj)` then you can access any class inside the methods.
    {
        obj.append(null);            // if ? is there then you can just use null because ? is generic it does not know what you are storing.
    }

    public static void main(String[] args)
    {
        MyArray<String> ma1 = new MyArray<>();
        fun(ma1);
    }
}
```
6. Base type of an Object should be same or ?
```java
public class GenericDemo
{
    static void fun(MyArray<?> obj)
    {
        obj.append(null);
    }

    public static void main(String[] args)
    {
        MyArray<Number> ma1 = new MyArray<Integer>();    // not allowed
        MyArray<Integer> ma1 = new MyArray<Integer>();  // allowed
        MyArray<?> ma1 = new MyArray<Integer>();  // allowed but you can not access `ma1.append(5)`
        fun(ma1);
    }
}
```












