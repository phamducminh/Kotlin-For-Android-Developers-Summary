# Kotlin-For-Android-Developers-Summary

> Summary of the book Kotlin for Android Developers by Antonio Leiva

## Contents

- [1. Classes and functions](README.md#1-classes-and-functions)
    - [1.1. How to declare a class](README.md#11-how-to-declare-a-class)
    - [1.2. Class inheritance](README.md#12-class-inheritance)
    - [1.3. Functions](README.md#13-functions)
    - [1.4. Constructor and functions parameters](README.md#14-constructor-and-functions-parameters)

---

## 1. Classes and functions
### 1.1. How to declare a class

```kotlin
class MainActivity {

}
```

* Class with extra constructor:

```kotlin
class Person(name: String, surname: String)
```

* Using _init_ block to declare the body of constructor

```kotlin
class Person(name: String, surname: String) {
        init {
        ...
        }
}
```

### 1.2. Class inheritance

* A class always extends from **_Any_** (similar to Java _Object_).
* Classes are closed by default (final).
* Can only extend a class if it's exlicitly declared as **open** or **abstract**:

```kotlin
open class Animal(name: String)
class Person(name: String, surname: String) : Animal(name)
```

_Note that when using the single constructor nomenclature, we need to specify the parameters we’re using for the parent constructor. That’s the equivalent to super() call in Java._

### 1.3. Functions

* Using **fun** keyword:

```kotlin
fun onCreate(savedInstanceState: Bundle?) {
}
```

* Return **Unit** (kotlin) = **void** (Java), any type can be returned

```kotlin
fun add(x: Int, y: Int) : Int {
    return x + y
}
```

* If the result can be calculated using single expression, get rid of brackets and use equal

```kotlin
fun add(x: Int, y: Int) : Int = x + y
```

### 1.4. Constructor and functions parameters

* Name First - Type Later (just like Swift :)

```kotlin
fun add(x: Int, y: Int) : Int {
    return x + y
}
```

* **Support default value, avoid function overloading**

```kotlin
fun niceToast(message: String,
              tag: String = MainActivity::class.java.simpleName,
              length: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, "[$tag] $message", length).show()
}
```

Using: 

```kotlin
niceToast("Hello")
niceToast("Hello", "MyTag")
niceToast("Hello", "MyTag", Toast.LENGTH_SHORT)
```

* **named arguments** can be used, mean you can write the name of the argument preceding the value to specify which one you want:

```kotlin
niceToast(message = "Hello", length = Toast.LENGTH_SHORT)
niceToast(message = "Hello", tag = "MyTag")
```

* Classes, functions or properties are **_public_** by
default
