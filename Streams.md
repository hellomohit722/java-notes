
Java 8 --> minimal code, functional programing

Java 8 --> lambda expression, Streams, Date & Time API

lambda expression is an anonymous function ( no name, no return type, no access modifier )

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
---
# Streams

# Key Characteristics of Streams
- Process collections of data in a **functional (lambda-based)** manner
- Use **declarative programming** (describe *what* to do, not *how* to do it)
- Do not store data; they **process data on demand**
- Can be processed **sequentially or in parallel**

## Benefits of Using Streams
- **Simplifies data processing**
- **Encourages functional programming**
- **Improves readability and maintainability**
- **Enables easy parallelism**

 
# Creating Streams

## Infinite streams

- a. Infinite stream (Java 8)
    - ```Stream.iterate(T seed, UnaryOperator<T> f)```

- b. Finite stream (Java 9+)
    - ```Stream.iterate(T seed, Predicate<T> hasNext, UnaryOperator<T> f)```
- c. Always infinite
    - ```Stream.generate(Supplier<T> s)```
---
# Terminal Operation

- List<Integer> list = Arrays.asList(1, 2, 3);

## reduce
> Combines elements to produce a single result
- Optional<T> reduce(BinaryOperator<T> accumulator)
- T reduce(T identity, BinaryOperator<T> accumulator)

## forEachOrdered

```java
List<Integer> numbers0 = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

numbers0.parallelStream().forEach(System.out::println);   // 76910835421
numbers0.parallelStream().forEachOrdered(System.out::println);  //12345678910
```
Note: Streams cannot be reused after a terminal operation has been called

```java
Stream<String> stream = names.stream();
stream.forEach(System.out::println);
List<String> list1 = stream.map(String::toUpperCase).toList(); // exception
```

- Statefull: which know about the other elements like sorted
- stateless: which do not know about the other elements like map

# Parallel Stream
* A type of stream that enables parallel processing of elements
* Allowing multiple threads to process parts of the stream simultaneously
* This can significantly improve performance for large data sets
* workload is distributed across multiple threads

```java
public class ParallelStream {
    public static void main(String[] args) {

        long startTime = System.currentTimeMillis();
        List<Integer> list = Stream.iterate(1, x -> x + 1).limit(20000).toList();
        List<Long> factorialsList = list.stream().map(ParallelStream::factorial).toList();
        long endTime = System.currentTimeMillis();
        System.out.println("Time taken with sequential stream: " + (endTime - startTime) + " ms");

        startTime = System.currentTimeMillis();
        factorialsList = list.parallelStream().map(ParallelStream::factorial).toList();
//      factorialsList = list.parallelStream().map(ParallelStream::factorial).sequential().toList();
        endTime = System.currentTimeMillis();
        System.out.println("Time taken with parallel stream: " + (endTime - startTime) + " ms");

        // Parallel streams are most effective for CPU-intensive or large datasets where tasks are independent
        // They may add overhead for simple tasks or small datasets

        // Cumulative Sum
        // [1, 2, 3, 4, 5] --> [1, 3, 6, 10, 15]
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        AtomicInteger sum =  new AtomicInteger(0);   // to achieve thread safety as lambda expression execute in parallel 
        List<Integer> cumulativeSum = numbers.stream().sequential().map(sum::addAndGet).toList();
        System.out.println("Expected cumulative sum: [1, 3, 6, 10, 15]");
        System.out.println("Actual result with parallel stream: " + cumulativeSum);

    }
    private static long factorial(int n) {
        long result = 1;
        for (int i = 2; i <= n; i++) {
            result *= i;
        }
        return result;
    }
}
```

---
# Collector
* Collectors is a utility class (Like Arrays)
* provides a set of methods to create common collectors

` List<String> names = Arrays.asList("Alice", "Bob", "Charlie");`
` List<Integer> nums = Arrays.asList(1, 2, 2, 3, 4, 4, 5);`
1. Collecting to a List

```java
List<String> res = names.stream().collect(Collectors.toList());
```

2. Collecting to a Set

```java
Set<Integer> set = nums.stream().collect(Collectors.toSet());
```

3. Collecting to a Specific Collection

```java
ArrayDeque<String> collect = names.stream().collect(Collectors.toCollection(() -> new ArrayDeque<>()));
```

4. Joining Strings
Concatenates stream elements into a single String

```java
String concatenatedNames = names.stream().map(String::toUpperCase).collect(Collectors.joining(", "));
```

5. Summarizing Data
Generates statistical summary (count, sum, min, average, max)

```java
IntSummaryStatistics stats = nums.stream().collect(Collectors.summarizingInt(x -> x));

System.out.println("Count: " + stats.getCount());
System.out.println("Sum: " + stats.getSum());
System.out.println("Min: " + stats.getMin());
System.out.println("Average: " + stats.getAverage());
System.out.println("Max: " + stats.getMax());

```
`Double average = numbers.stream().collect(Collectors.averagingInt(x -> x));`

6. Counting Elements
```java
Long count = nums.stream().collect(Collectors.counting());
```

7. Grouping Elements

```java
List<String> words = Arrays.asList("hello", "world", "java", "streams", "collecting");
        System.out.println(words.stream().collect(Collectors.groupingBy(String::length)));
// {4=[java], 5=[hello, world], 7=[streams], 10=[collecting]}
     System.out.println(words.stream().collect(Collectors.groupingBy(String::length, Collectors.joining(", "))));
// {4=java, 5=hello, world, 7=streams, 10=collecting}
     System.out.println(words.stream().collect(Collectors.groupingBy(String::length, Collectors.counting())));
// {4=1, 5=2, 7=1, 10=1}

        TreeMap<Integer, Long> treeMap = words.stream().collect(Collectors.groupingBy(String::length, TreeMap::new, Collectors.counting()));
        System.out.println(treeMap);
// {4=1, 5=2, 7=1, 10=1}
```

8. Partitioning Elements
Partitions elements into two groups (true and false) based on a predicate
```java
System.out.println(words.stream().collect(Collectors.partitioningBy(x -> x.length() > 5)));
```

9. Mapping and Collecting
Applies a mapping function before collecting
```java
 System.out.println(words.stream().collect(Collectors.mapping(x -> x.toUpperCase(), Collectors.toList())));
```

10. toMap

```java
List<String> words2 = Arrays.asList("apple", "banana", "apple", "orange", "banana", "apple");
        System.out.println(words2.stream().collect(Collectors.toMap(k -> k, v -> 1, (x, y) -> x + y)));;
```

```java

        // Example 1: Collecting Names by Length
        List<String> l1 = Arrays.asList("Anna", "Bob", "Alexander", "Brian", "Alice");
        System.out.println(l1.stream().collect(Collectors.groupingBy(String::length)));

        // Example 2: Counting Word Occurrences
        String sentence = "hello world hello java world";
        System.out.println(Arrays.stream(sentence.split(" ")).collect(Collectors.groupingBy(x -> x, Collectors.counting())));

        // Example 3: Partitioning Even and Odd Numbers
        List<Integer> l2 = Arrays.asList(1, 2, 3, 4, 5, 6);
        System.out.println(l2.stream().collect(Collectors.partitioningBy(x -> x % 2 == 0)));

        // Example 4: Summing Values in a Map
        Map<String, Integer> items = new HashMap<>();
        items.put("Apple", 10);
        items.put("Banana", 20);
        items.put("Orange", 15);
        System.out.println(items.values().stream().reduce(Integer::sum));
        System.out.println(items.values().stream().collect(Collectors.summingInt(x -> x)));

        // Example 5: Creating a Map from Stream Elements
        List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry");
        System.out.println(fruits.stream().collect(Collectors.toMap(x -> x.toUpperCase(), x -> x.length())));

```














