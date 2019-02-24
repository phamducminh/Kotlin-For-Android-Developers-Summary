# Kotlin-For-Android-Developers-Summary

> Summary of the book Kotlin for Android Developers by Antonio Leiva

## Contents

- [1. Classes and functions](README.md#1-classes-and-functions)
    - [1.1. How to declare a class](README.md#11-how-to-declare-a-class)
    - [1.2. Class inheritance](README.md#12-class-inheritance)
    - [1.3. Functions](README.md#13-functions)
    - [1.4. Constructor and functions parameters](README.md#14-constructor-and-functions-parameters)
- [2. Variables and properties](README.md#2-variables-and-properties)
    - [2.1. Basic types](README.md#21-basic-types)
    - [2.2. Variables](README.md#22-variables)
    - [2.3. Properties](README.md#23-properties)

- [3. Anko and Extension Functions](README.md#3-anko-and-extension-functions)
    - [3.1. What is Anko?](README.md#31-what-is-anko)
    - [3.2. Start using Anko](README.md#32-start-using-anko)
    - [3.3. Extension functions](README.md#33-extension-functions)

- [4. Retrieving data from API](README.md#4-retrieving-data-from-api)
    - [4.1 Performing a request](README.md#41-performing-a-request)
    - [4.2 Performing the request out of the main thread](README.md#42-performing-the-request-out-of-the-main-thread)

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

* Using ```init``` block to declare the body of constructor

```kotlin
class Person(name: String, surname: String) {
        init {
        ...
        }
}
```

### 1.2. Class inheritance

* A class always extends from **```Any```** (similar to Java _Object_).
* Classes are closed by default (final).
* Can only extend a class if it's exlicitly declared as **open** or **abstract**:

```kotlin
open class Animal(name: String)
class Person(name: String, surname: String) : Animal(name)
```

_Note that when using the single constructor nomenclature, we need to specify the parameters we’re using for the parent constructor. That’s the equivalent to super() call in Java._

### 1.3. Functions

* Using **```fun```** keyword:

```kotlin
fun onCreate(savedInstanceState: Bundle?) {
}
```

* Return **```Unit```** (kotlin) = **```void```** (Java), any type can be returned

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

* Classes, functions or properties are **```public```** by
default

## 2. Variables and properties

In Kotlin:
* **Everything is an object**
* No primivitve types as in Java

### 2.1. Basic types

Some differences:

* No automatic conversions among numeric types. Cannot assign an Int to a Double variable, must use explicit conversion

```kotlin
val i: Int = 7
val d: Double = i.toDouble()
```

* Characters (Char) cannot directly be used as numbers.

```kotlin
val c: Char = 'c'
val i: Int = c.toInt()
```

* Using "```and```" and "```or```" for bitwise arithmetical operations

```kotlin
// Java
int bitwiseOr = FLAG1 | FLAG2;
int bitwiseAnd = FLAG1 & FLAG2;

// Kotlin
val bitwiseOr = FLAG1 or FLAG2
val bitwiseAnd = FLAG1 and FLAG2
```

* Literals can give information about its type. Let the compiler infer the type:

```kotlin
val i= 12 // An Int
val iHex = 0x0f // An Int from hexadecimal literal
val l = 3L // A Long
val d = 3.5 // A Double
val f = 3.5F // A Float
```

* A String can be accessed as an array and can be iterated:

```kotlin
val s = "Example"
val c = s[2] // This is the Char 'a'

// Iterate over String
val s = "Example"
for(c in s){
    print(c)
}
```

### 2.2. Variables

* Variables in Kotlin can be easily defined as mutable (**var**) or immutable (**val**).
* **The key concept: just use val as much as possible**
* Don’t need to specify object types, they will be inferred from the value

```kotlin
val s = "Example" // A String
val i = 23 // An Int
val actionBar = supportActionBar // An ActionBar in an Activity context
```

However, a type needs to be specified if we want to use a more generic type:

```kotlin
val a: Any = 23
val c: Context = activity
```

### 2.3. Properties

Properties are the equivalent to fields in Java. Properties will do the work of a field plus a getter plus a setter.

In Java:

```java
public class Person {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

...

Person person = new Person();
person.setName("name");
String name = person.getName();
```

In Kotlin, only a property is required:

```kotlin
public class Person {}

    var name: String = ""

}

...

val person = Person()
person.name = "name"
val name = person.name
```

Custom getter and setter:

```kotlin
public class Person {
    
    var name: String = ""
        get() = field.toUpperCase()
    set(value) {
        field = "Name: $value"
    }

}
```

## 3. Anko and Extension Functions

### 3.1. What is Anko?

* Anko is a powerful library developed by JetBrains.
* Its main purpose is the generation of UI layouts by using code instead of XML.
* Anko includes a lot of extremely helpful functions and properties that will avoid lots of boilerplate.

### 3.2. Start using Anko

Anytime you use something from Anko, it will include an import with the name of the property or function to the file. This is because Anko uses **extension functions** to add new features to Android framework.

In ```MainActivity:onCreate```, an Anko extension function can be used to simplify how to find the ```RecyclerView```:

```kotlin
val forecastList: RecyclerView = find(R.id.forecast_list)
```

### 3.3. Extension functions

## 4. Retrieving data from API

### 4.1 Performing a request

Request class simply receives an url, reads the result and outputs the json in the Logcat

```kotlin
class Request(val url: String) {

    fun run() {
        val forecastJsonStr = URL(url).readText()
        Log.d(javaClass.simpleName, forecastJsonStr)
    }

}
```

### 4.2 Performing the request out of the main thread

```kotlin
async() {
    Request(url).run()
    uiThread { longToast("Request performed") }
}
```

* **```async```** function will execute its code in another thread, with the option to return to the main thread by calling ```uiThread```
* ```uiThread``` is implemented differently depending on the caller object. If it’s used by an ```Activity```, the ```uiThread``` code won’t be executed if ```activity.isFinishing()``` returns ```true```, and it won’t crash if the activity is no longer valid.
* use ```async``` executor:

```kotlin
val executor = Executors.newScheduledThreadPool(4)
async(executor) {
    // Some task
}
```

* to return a future with a result, use **```asyncResult```**.


