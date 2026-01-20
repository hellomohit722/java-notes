# Generics in Java

## Object for Generalization

Object can be used for generalization, but it has some problems:

### Problems
1. There is no type safety (e.g., storing a String, then later an Integer).
2. Typecasting is required.

```java
package genericdemo;

public class GenericDemo<T>
{
    T data[] = (T[]) new Object[3];

    public static void main(String[] args)
    {
        GenericDemo<String> gd = new GenericDemo();

        gd.data[0] = "hi";
        gd.data[1] = "bye";
        gd.data[2] = new Integer(10);

        String str = gd.data[0];
    }
}
```

# Generic Class
## Templates are for objects, not for primitive data types.

```
package genericdemo;

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

### Notes on Generics
1. If no arguments are passed to the generic class, it will be treated like Object.
2. Multiple parameters (generally used for key-value pairs).
3. Subtype: When you inherit from a generic class, you must pass the type; otherwise, it defaults to Object. For generics, use for `child<T> extends parent<T>`.

```
class MyArray<T>
{
}

class MyArray2<T> extends MyArray<T>
{
   // super(T)
}

MyArray2<String>
```
4. Bounded Type:
```
class MyArray<T extends Number> // now only subclass of Number is permitted, extends must be used for class & interface
```
# 1. Generic Methods (Bound also works)

```
public class GenericDemo
{
    static <E> void show(E... list)
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
 2. Wildcard ?

```
public class GenericDemo
{
    static void fun(MyArray<?> obj)
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
3. Lower Bound {super}
4. Upper Bound {extends}

# Do’s and Don’ts in Generics
1. Only extends is allowed in Generic class definition.
2. extends is used for interfaces also.
3. extends from only one class and multiple interfaces.
4. extends and super are allowed with ? in methods.
5. <?> will accept all types but cannot access.
```
MyArray<?> ma1 = new MyArray<Integer>();
ma1.append(10);
fun(ma1);
```
6. Base type of an Object should be same or ?
```














