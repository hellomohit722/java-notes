
Java 8 --> minimal code, functional programing
Java 8 --> lambda expression, Streams, Date & Time API

#lambda expression is an anonymous function ( no name, no return type, no access modifier )

if we have to implement single method in the class then we can use lambda expression (e.g. run in Runable interface).
functional interface(Which have only one abstract methods rest can be default & static) use to hold lambda expression.
lambda expression implements only functional interface

# Predicate
Functional interface(Boolean valued function)

```java
//expression is treated as variable
//predicate just hold the condition in the variable

Predicate<Integer> isEven = x -> x % 2 == 0;
System.out.println(isEven.test(4));

Predicate<String> startsWithA = x->x.startsWith("M");
Predicate<String> endsWithT = x->x.endsWith("T");
System.out.println((startsWithA.and(endsWithT).test("Mohit")));
```

# Function
do the operation and return
```java
Function<Integer,Integer> doubleIt = x->2*x;
Function<Integer,Integer> tripleit = x->3*x;

System.out.println(doubleIt.andThen(tripleit).apply(5));
System.out.println(doubleIt.compose(tripleit).apply(5));

Identity method
Function<Object, Object> identity = Function.identity();
System.out.println(identity.apply(51));
```

# Consumer 
just do operation no return
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

# Suplier
Just do the operation No input
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

# Method reference
use method without invoking & in place of lambda expression
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

----
# Stream : is a feature which process collection of data in a functional[lambda] & declarative manner[filter..].
Simplify Data Processing
Embrace Functional Programming
Improve Readability and Maintainability
Enable Easy Parallelism

#What is stream ?
A sequence of elements supporting functional and declarative programing

#How to Use Streams ?
 Source, intermediate operations & terminal operation
        
```java
List<Integer> list = Arrays.asList(1,2,3);
num = list.stream().filter(x->x%2==0).count();
System.out.println(num);
```

# Creating Streams ---

1. From collections
```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
Stream<Integer> stream = list.stream();
```        
2. From Arrays
```java
String[] array = {"a", "b", "c"};
Stream<String> stream1 = Arrays.stream(array);
```     
3. Using Stream.of()
```java
Stream<String> stream2 = Stream.of("a", "b");
```     
4. Infinite streams
```java
Stream.generate(() -> 1);
Stream.iterate(1, x -> x + 1);
```
---
# Intermediate operation

Note: Intermediate operations transform a stream into another stream.
They are lazy, meaning they don't execute until a terminal operation is invoked.


1. filter
```java
List<String> list = Arrays.asList("Akshit", "Ram", "Shyam", "Ghanshyam", "Akshit");
Stream<String> filteredStream = list.stream().filter(x -> x.startsWith("A"));
// no filtering at this point
long res = list.stream().filter(x -> x.startsWith("A")).count();
System.out.println(res);
```

2.map

```java
Stream<String> stringStream = list.stream().map(String::toUpperCase);
Stream<String> stringStream = list.stream().map(String::toUpperCase);
```

3. sorted
```java
Stream<String> sortedStream = list.stream().sorted();
Stream<String> sortedStreamUsingComparator = list.stream().sorted((a, b) -> a.length() - b.length());
```

 4. distinct

```java
System.out.println(list.stream().filter(x -> x.startsWith("A")).distinct().count());
```

5. limit

```java
System.out.println(Stream.iterate(1, x -> x + 1).limit(100).count());
```
        
6. skip

```java
System.out.println(Stream.iterate(1, x -> x + 1).skip(10).limit(100).count());
```
7. peek

```java
// Performs an action on each element as it is consumed.
Stream.iterate(1, x -> x + 1).skip(10).limit(100).peek(System.out::println).count();
```

8. flatMap

```java
// Handle streams of collections, lists, or arrays where each element is itself a collection
// Flatten nested structures (e.g., lists within lists) so that they can be processed as a single sequence of elements
// Transform and flatten elements at the same time.

List<List<String>> listOfLists = Arrays.asList(
                Arrays.asList("apple", "banana"),
                Arrays.asList("orange", "kiwi"),
                Arrays.asList("pear", "grape")
);

System.out.println(listOfLists.get(1).get(1));
System.out.println(listOfLists.stream().flatMap(x -> x.stream()).map(String::toUpperCase).toList());

List<String> sentences = Arrays.asList(
                "Hello world",
                "Java streams are powerful",
                "flatMap is useful"
);

System.out.println(sentences
                .stream()
                .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
                .map(String::toUpperCase)
                .toList()
);
```
















