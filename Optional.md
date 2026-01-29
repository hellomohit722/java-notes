# Optional in Java 

## 1. Introduction to `Optional`

`Optional<T>` is a container object introduced in **Java 8** (`java.util.Optional`) that may or may not contain a non-null value.  
Its main goal is to **reduce `NullPointerException` (NPE)** and make the **absence of a value explicit** in APIs.

> **Key idea:** Instead of returning `null`, return an `Optional`.

---
 
## 2. Why `Optional` Was Introduced

### Problems with `null`
- Causes `NullPointerException`
- No clear indication whether a value can be absent
- Requires constant null checks
- Makes APIs ambiguous

### Benefits of `Optional`
- Explicitly represents presence or absence of a value
- Encourages functional-style programming
- Improves API readability and safety
- Reduces runtime null checks

---

## 3. Creating `Optional` Objects

### 3.1 `Optional.empty()`
Creates an empty `Optional`.

`Optional<String> opt = Optional.empty();`

Creates an Optional containing a non-null value
`Optional<String> opt = Optional.of("Java");`
⚠️ Throws NullPointerException if value is null.

Creates an Optional that may hold a null value.
`Optional<String> opt = Optional.ofNullable(null);`
✅ Safest way when value may be null.

Returns true if value exists.
`if (opt.isPresent()) {
    System.out.println(opt.get());
}`

ifPresent(Consumer)
Executes code only if value exists.
`opt.ifPresent(value -> System.out.println(value));`

ifPresentOrElse(Consumer, Runnable)
`opt.ifPresentOrElse(
    value -> System.out.println(value),
    () -> System.out.println("No value")
);`

Returns true if value is absent.
`if (opt.isEmpty()) {
    System.out.println("No value present");
}`

Returns the value if present.
`String value = opt.get();`
⚠️ Throws NoSuchElementException if empty.

Returns value if present, otherwise returns default.
`String value = opt.orElse("Default");`

orElseGet(Supplier)
Returns value if present, otherwise computes default lazily.
`String value = opt.orElseGet(() -> "Computed Default");`

Throws NoSuchElementException if empty.
`String value = opt.orElseThrow();`

orElseThrow(Supplier<Exception>)
Throws custom exception.
`String value = opt.orElseThrow(
    () -> new IllegalArgumentException("Value missing")
);`

Transforming Values

map(Function)
`Optional<Integer> length = Optional.of("Java").map(String::length);`
- If empty → remains empty
- If function returns null → empty Optional

flatMap(Function)
Used when the mapping function returns an Optional.
`Optional<String> result = opt.flatMap(this::getOptionalValue);`
Prevents nested Optional<Optional<T>>.

filter(Predicate)
Keeps value only if condition matches.
`Optional<String> filtered =Optional.of("Java").filter(s -> s.length() > 3);`
Condition fails → empty Optional

Converting Optional to Stream
`opt.stream().forEach(System.out::println);`
- Empty → empty stream
- Present → single-element stream

Using Optional in Stream Pipelines
`List<String> result =
    list.stream()
        .map(this::findValue) // returns Optional<String>
        .flatMap(Optional::stream)
        .toList();`

# Best Practices
✅ Use Optional for return values
❌ Do not use Optional for fields or parameters
❌ Do not call get() without checking
✅ Prefer map, flatMap, and orElseGet
❌ Do not serialize Optional





