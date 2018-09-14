
# Basic Types

* In this section we describe the basic types used in Kotlin: numbers, characters, booleans, arrays, and strings.

## Numbers

* built-in types

| Type | Bit With | Tag |
|------|----------|-----|
| Double | 64 | default |
| Float | 32 | f or F|
| Long | 64 | L |
| Int | 32 | default |
| Short | 16 | |
| Byte | 8 | |

* Note: Characters are not numbers in kotlin.
* Note: Octal liteals are not supported.


#### Underscores in numeric literals(since 1.1)

```
val oneMillion = 1_000_000
```

#### Representation

* On the Java platform, numbers are physically stored as JVM primitive types, unless we need a nullable number reference(e.g. Int?) or generics are involved. In the latter cases numbers are boxed.

```
val a: Int = 10000
println(a === a) // Prints 'true'
val boxedA: Int? = a
val anotherBoxedA: Int? = a
println(boxedA === anotherBoxedA) // !!!Prints 'false'!!!
```

```
val a: Int = 10000
println(a == a) // Prints 'true'
val boxedA: Int? = a
val anotherBoxedA: Int? = a
println(boxedA == anotherBoxedA) // Prints 'true'
```



## Characters

## Booleans

## Arrays

## Strings
