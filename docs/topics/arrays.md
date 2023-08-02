[//]: # (title: Arrays)

 Before you use an array, we strongly recommend that you consider using a [collection](collections-overview.md) instead. 
 Collections have many benefits:
   * They can be mutable or read-only
   * You can add or remove elements from mutable collections

Arrays in Kotlin are represented by the `Array` class which stores objects. Arrays are always mutable and have a fixed size. 

## Create arrays

To create an array, use the [`arrayOf()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of.html) function and pass the item values to it. For example:

```kotlin
fun main() {
//sampleStart
    // Creates an array with values [1, 2, 3]
    val simpleArray = arrayOf(1, 2, 3)
    println(simpleArray.joinToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="simple-array-kotlin"}

> When creating arrays, you can use a [trailing comma](coding-conventions.md#trailing-commas).
>
{type="note"}

Alternatively, use the [`arrayOfNulls()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/array-of-nulls.html#kotlin$arrayOfNulls(kotlin.Int)) function to create an array of a given size filled with `null` elements.

```kotlin
fun main() {
//sampleStart
    // Creates an array with values [null, null, null]
    val nullArray: Array<Int?> = arrayOfNulls(3)
    println(nullArray.joinToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="null-array-kotlin"}

Another option is to use the `Array` constructor that takes the array size and a function that returns values
for array elements given its index:

```kotlin
fun main() {
//sampleStart
    // Creates an Array<Int> that initializes with zeros [0, 0, 0]
    val initArray = Array<Int>(3) { 0 }
    println(initArray.joinToString())
    
    // Creates an Array<String> with values ["0", "1", "4", "9", "16"]
    val asc = Array(5) { i -> (i * i).toString() }
    asc.forEach { println(it) }
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3"}

> Indices start at 0 in Kotlin.
>
{type="note"}

You can assign an array to a variable. With this approach it may seem that you can dynamically add or remove elements to the array, but Kotlin is actually creating a new array each time. We recommend that you use [mutable collections](collections-overview.md) instead, which are designed to make it easy to add and remove elements.

```kotlin
fun main() {
//sampleStart
    var riversArray = arrayOf("Nile", "Amazon", "Yangtze")
    
    // Using the += assignment operation creates a new riversArray, copies over the original elements and adds "Mississippi"
    riversArray += "Mississippi"
    println(riversArray.joinToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="rivers-array-kotlin"}

### Empty arrays

To create an empty array, use [`emptyArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/empty-array.html).

```kotlin
    var exampleArray = emptyArray<String>()
```

> You can specify the type on the left-hand or right-hand side of the assignment due to Kotlin's type inference.
> 
> For example:
> ```Kotlin
> var exampleArray = emptyArray<String>()
> 
> var exampleArray: Array<String> = emptyArray()
>```
>
{type="note"}

### Multidimensional arrays

Arrays can be nested within each other to create multidimensional arrays:

```kotlin
fun main() {
//sampleStart
    // Creates a two-dimensional array
    val twoDArray = Array(2) { Array<Int>(2) { 0 } }
    println(twoDArray.contentDeepToString())

    // Creates a three-dimensional array
    val threeDArray = Array(3) { Array(3) { Array<Int>(3) { 0 } } }
    println(threeDArray.contentDeepToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="multidimensional-array-kotlin"}

> Nested arrays don't have to be the same type or the same size.
>
{type="note"}

## Access and modify elements

Arrays are always mutable. To access and modify elements in an array, use the [indexed access operator](operator-overloading.md#indexed-access-operator)`[]`:

```kotlin
fun main() {
//sampleStart
    val simpleArray = arrayOf(1, 2, 3)
    val twoDArray = Array(2) { Array<Int>(2) { 0 } }
    
    // Accesses the element and modifies it
    simpleArray[0] = 10
    twoDArray[0][0] = 2
    
    // Prints the modified element
    println(simpleArray[0].toString()) // 10
    println(twoDArray[0][0].toString()) // 2
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="access-array-kotlin"}

Arrays in Kotlin are _invariant_. This means that Kotlin does not let us assign an `Array<String>`
to an `Array<Any>`, which prevents a possible runtime failure (but you can use `Array<out Any>`,
see [Type Projections](generics.md#type-projections)).

## Compare arrays

To compare whether two arrays have the same elements in the same order, use [`contentEquals()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/content-equals.html).

```kotlin
fun main() {
//sampleStart
    val simpleArray = arrayOf(1, 2, 3)
    val anotherArray = arrayOf(1, 2, 3)
    
    // Compares contents of arrays: true
    println(simpleArray.contentEquals(anotherArray))

    // Compares contents of arrays after an element is changed: false
    simpleArray[0] = 10
    println(simpleArray.contentEquals(anotherArray))
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="compare-array-kotlin"}

> Don't use equality (`==`) and inequality (`!=`) [operators](equality.md#structural-equality) to compare the contents of arrays. These operators
> check whether the assigned variables point to the same object.
> 
{type="warning"}

## Transform arrays

There are many useful functions that you can use to transform arrays. We've highlighted a few below but this isn't an exhaustive list. For the full list of functions, see our [API reference](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-array/).

### Sum

To return the sum of all elements in an array, use the [sum()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/sum.html) function.

```Kotlin
fun main() { 
// sampleStart
    val sumArray = arrayOf(1, 2, 3)
    
    // Sums array elements: 6
    println(sumArray.sum()) 
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="sum-array-kotlin"}

> The `sum()` function can only be used with arrays of numeric data types.
>
{type="note"}

### Shuffle

To randomly shuffle the elements in an array, use the [shuffle()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/shuffle.html) function.

```Kotlin
fun main() {
// sampleStart
    val simpleArray = arrayOf(1, 2, 3)

    // Shuffles elements [3, 2, 1]
    simpleArray.shuffle()
    println(simpleArray.joinToString())

    // Shuffles elements again [2, 3, 1]
    simpleArray.shuffle()
    println(simpleArray.joinToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="shuffle-array-kotlin"}

### Zip

To combine two arrays into an array of [`Pair`]((https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-pair/)), use the [zip()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/zip.html) function.

```Kotlin
fun main() { 
//sampleStart
    val firstArray = arrayOf(1, 2, 3)
    val secondArray = arrayOf(1, 2, 3)

    // Zips arrays [(1, 1), (2, 2), (3, 3)]
    val zipArray = firstArray.zip(secondArray)
    println(zipArray.joinToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="zip-array-kotlin"}

To return a pair of lists from an array of `Pair`, use the [unzip()](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/unzip.html) function.

```Kotlin
fun main() { 
//sampleStart
    val pairArray = arrayOf(Pair(1,1), Pair(2,2), Pair(3,3))

    // Unzips array ([1, 2, 3], [1, 2, 3])
    val unzipArray = pairArray.unzip()
    println(unzipArray)
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="unzip-array-kotlin"}

## Concatenate arrays

Arrays of type `String` can be concatenated using the `+` operator.

```kotlin
fun main() {
//sampleStart
    val simpleArray = arrayOf("a", "b", "c")
    val anotherArray = arrayOf("d", "e", "f")
    
    // Creates a new array by concatenating elements: ["a", "b", "c", "d", "e", "f"]
    println((simpleArray + anotherArray).joinToString())
//sampleEnd
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="concatenate-array-kotlin"}

## Pass variable number of arguments to a function

In some cases it is useful to be able to pass a variable number of arguments to a function without having to define the number of arguments in advance.
In Kotlin you can use the [`vararg`](functions.md#variable-number-of-arguments-varargs) parameter for this. To pass a variable number of arguments to a function as the contents of an array, use the _spread_ operator (`*`).
The spread operator passes each element of the array as individual arguments to your chosen function.

```kotlin
fun main() {
 val lettersArray = arrayOf("c", "d")
 printAllStrings("a", "b", *lettersArray)
}

fun printAllStrings(vararg strings: String) {
 for(string in strings) {
  println(string)
 }
}
```
{kotlin-runnable="true" kotlin-min-compiler-version="1.3" id="vararg-array-kotlin"}

For more information, see [Variable number of arguments (varargs)](functions.md#variable-number-of-arguments-varargs).

## Convert to collection

Arrays can be converted to [collections](collections-overview.md).

### Convert to List or Set
To convert an array to a `List` or `Set`, use the [`toList()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-list.html) and [`toSet()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-set.html) functions.

```kotlin
fun main() {
//sampleStart
    val simpleArray = arrayOf("a", "b", "c", "c")
    
    // Convert to Set ["a", "b", "c"]
    println(simpleArray.toSet())
    
    // Convert to List ["a", "b", "c", "c"]
    println(simpleArray.toList())
//sampleEnd
}
```

### Convert to Map

To convert an array to a `map`, use the [`toMap()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-map.html) function.
Only an array of [`Pair`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-pair/) can be converted to a map: `Array<Pair<K,V>>`.
The first value of a `Pair` instance becomes a key and the second becomes a value.

```kotlin
fun main() {
//sampleStart
    val pairArray = arrayOf(Pair("Afghanistan","Kabul"), Pair("Albania","Tirana"), Pair("Algeria","Algiers"))

    // Convert to Map {Afghanistan=Kabul, Albania=Tirana, Algeria=Algiers}
    // The keys are countries and the values are their capital cities
    println(pairArray.toMap())
    
//sampleEnd
}
```

## Primitive type arrays

Kotlin also has classes that represent arrays of primitive types without boxing overhead:

| | | | |
|--|--|--|--|
|[`BooleanArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-boolean-array/)|[`ByteArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-byte-array/)|[`CharArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-char-array/)|[`DoubleArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-double-array/)|
|[`FloatArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-float-array/)|[`IntArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-int-array/)|[`LongArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-long-array/)|[`ShortArray`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-short-array/)|

These classes have no inheritance relation to the `Array` class, but they
have the same set of methods and properties. They are equivalent to `boolean[]`, `byte[]`, `char[]`, `double[]`, `float[]`, `int[]`, `long[]`, and `short[]` arrays of primitive types in Java.

```kotlin
fun main() {
//sampleStart
    // Array of Int of size 5 with values [0, 0, 0, 0, 0]
    val exampleArray = IntArray(5)
    println(exampleArray.joinToString())
//sampleEnd
}
```

> To convert primitive arrays to typed arrays, use [`toTypedArray()`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/to-typed-array.html).
>
{type="note"}

## What's next?

* Learn about [collections](collections-overview.md)
* Learn about other [basic types](basic-types.md)
