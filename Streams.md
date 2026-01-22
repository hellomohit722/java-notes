
# Java 8 --> minimal code, functional programing
# Java 8 --> lambda expression, Streams, Date & Time API

## lambda expression is an anonymous function ( no name, no return type, no access modifier )

if we have to implement single method in the class then we can use lambda expression (e.g. run in Runable interface).
functional interface(Which have only one abstract methods rest can be default & static) use to hold lambda expression.
lambda expression implements only functional interface

# Predicate - Functional interface(Boolean valued function)

```java
//expression is treated as variable
//predicate just hold the condition in the variable

Predicate<Integer> isEven = x -> x % 2 == 0;
System.out.println(isEven.test(4));

Predicate<String> startsWithA = x->x.startsWith("M");
Predicate<String> endsWithT = x->x.endsWith("T");
System.out.println((startsWithA.and(endsWithT).test("Mohit")));
```

# Function - do the operation and return
```java
Function<Integer,Integer> doubleIt = x->2*x;
Function<Integer,Integer> tripleit = x->3*x;

System.out.println(doubleIt.andThen(tripleit).apply(5));
System.out.println(doubleIt.compose(tripleit).apply(5));

Identity method
Function<Object, Object> identity = Function.identity();
System.out.println(identity.apply(51));
```

# Consumer - just do operation no return
```java
Consumer<Integer> consumer = x-> System.out.println(x);
consumer.accept(46);

List<Integer> list = Arrays.asList(1,2,3,4);
Consumer<List<Integer>> consumer1 = x ->
{
    for(int i : x)
    {
       System.out.println(i);
    }
};
consumer1.accept(list);
```

# Suplier - Just do the operation No input
```java
Supplier<String> supplier = ()-> "Hello Mohit";
System.out.println((supplier.get()));
```

```java
// combined example
Predicate<Integer> predicate = x -> x % 2 == 0;
Function<Integer, Integer> function = x -> x * x;
Consumer<Integer> consumer = x -> System.out.println(x);
Supplier<Integer> supplier = () -> 100;

if (predicate.test(supplier.get())) {
    consumer.accept(function.apply(supplier.get()));
}
```
```java
// BiPredicate, BiConsumer, BiFunction

 BiPredicate<Integer, Integer> isSumEven = (x, y) -> (x + y) % 2 == 0;
 System.out.println(isSumEven.test(5, 5));

 BiConsumer<Integer, String> biConsumer = (x, y) -> {
        System.out.println(x);
        System.out.println(y);
};

BiFunction<String, String, Integer> biFunction = (x, y) -> (x + y).length();
System.out.println(biFunction.apply("a", "bc"));

// UnaryOperator, BinaryOperator
UnaryOperator<Integer> a = x -> 2 * x;

BinaryOperator<Integer> b = (x, y) -> x + y;
```
Note: - we can pass the method as parameter by two way
1. Lambda
2. Method refernce 

# Method reference --> use method without invoking & in place of lambda expression
```java
List<String> students = Arrays.asList("Ram", "Shyam", "Ghanshyam");
students.forEach(System.out::println);

```

# Constructor reference
```java
List<String> names = Arrays.asList("A", "B", "C");
List<MobilePhone> mobilePhoneList = names.stream().map(MobilePhone::new).collect(Collectors.toList());

class MobilePhone{
    String name;

    public MobilePhone(String name) {
        this.name = name;
    }
}
```




















