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
    - [4.1. Performing a request](README.md#41-performing-a-request)
    - [4.2. Performing the request out of the main thread](README.md#42-performing-the-request-out-of-the-main-thread)

- [5. Data Classes](README.md#5-data-classes)
    - [5.1. Extra functions](README.md#51-extra-functions)
    - [5.2. Copying a data class](README.md#52-copying-a-data-class)
    - [5.3. Mapping an object into variables](README.md#53-mapping-an-object-into-variables)

- [6. Parsing data](README.md#6-parsing-data)
    - [6.1. Converting json to data classes](README.md#61-converting-json-to-data-classes)
    - [6.2. Shaping the domain layer](README.md#62-shaping-the-domain-layer)
    - [6.3. Drawing the data in the UI](README.md#63-drawing-the-data-in-the-ui)

- [7. Operator overloading](README.md#7-operator-overloading)
    - [7.1. Operators tables](README.md#71-operators-tables)
    - [7.2. Operators in extension functions](README.md#72-operators-in-extension-functions)

- [8. Lambdas](README.md#8-lambdas)
    - [8.1. Simplifying setOnClickListener()](README.md#81-simplifying-setOnClickListener)
    - [8.2. Click listener for ForecastListAdapter](README.md#82-click-listener-for-forecastListAdapter)
    - [8.3. Extending the language](README.md#83-extending-the-language)

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

### 4.1. Performing a request

Request class simply receives an url, reads the result and outputs the json in the Logcat

```kotlin
class Request(val url: String) {

    fun run() {
        val forecastJsonStr = URL(url).readText()
        Log.d(javaClass.simpleName, forecastJsonStr)
    }

}
```

### 4.2. Performing the request out of the main thread

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

* To return a future with a result, use **```asyncResult```**.

## 5. Data Classes

Data classes in Kotlin = POJO classes in Java

```kotlin
data class Forecast(val date: Date, val temperature: Float, val details: String)
```

### 5.1. Extra functions

* ```equals()```
* ```hashCode()```
* ```copy()```: copy an object, modify the properties.
* A set of numbered functions to map an object into variables.

### 5.2. Copying a data class

If using immutability, if we want to change the state of an object, a new instance of the class is required

```kotlin
val f1 = Forecast(Date(), 27.5f, "Shiny day")
val f2 = f1.copy(temperature = 30f)
```

This way, we copy the first forecast and modify only the temperature property without changing the state of the original object.

### 5.3. Mapping an object into variables

* This process is known as **multi-declaration** and consists of mapping each property inside an object into a variable.
* The ```componentX``` functions are automatically created.

Example:

```kotlin
val f1 = Forecast(Date(), 27.5f, "Shiny day")
val (date, temperature, details) = f1
```

This multi-declaration is compiled down to the following code:

```kotlin
val date = f1.component1()
val temperature = f1.component2()
val details = f1.component3()
```

Another example:

```kotlin
for ((key, value) in map) {
    Log.d("map", "key:$key, value:$value")
}
```

## 6. Parsing data

### 6.1. Converting json to data classes

**Companion objects**

In Kotlin, we can’t create static properties or functions, but we need to rely on objects.

If we need some static properties, constants or functions in a class, we can use a **companion object**. This object will be shared among all instances of the class, the same as a static field or method would do in Java.

```kotlin
class ForecastRequest(val zipCode: String) {

    companion object {
        private val APP_ID = "15646a06818f61f7b8d7823ca833e1ce"
        private val URL = "https://api.openweathermap.org/data/2.5/forecast/daily?mode=json&units=metric&cnt=7"
        private val COMPLETE_URL = "$URL&APPID=$APP_ID&q="
    }

    fun execute(): ForecastResult {
        val forecastResult = URL(COMPLETE_URL + zipCode).readText()
        return Gson().fromJson(forecastResult, ForecastResult::class.java)
    }

}
```

### 6.2. Shaping the domain layer

```kotlin
class ForecastDataMapper {

    fun convertFromDataModel(forecast: ForecastResult): ForecastList {
        return ForecastList(forecast.city.name, forecast.city.country, convertForecastListToDomain(forecast.list))
    }

    private fun convertForecastListToDomain(list: List<Forecast>): List<ModelForecast> {
        return list.map { convertForecastItemToDomain(it) }
    }

    private fun convertForecastItemToDomain(forecast: Forecast): ModelForecast {
        return ModelForecast(
            convertDate(forecast.dt), forecast.weather[0].description, forecast.temp.max.toInt(),
            forecast.temp.min.toInt(), generateIconUrl(forecast.weather[0].icon)
        )
    }

    private fun convertDate(date: Long): String {
        val df = DateFormat.getDateInstance(DateFormat.MEDIUM, Locale.getDefault())
        return df.format(date * 1000)
    }

}
```

* When using two classes with the same name, give a specific name to one of them so that we don’t need to write the complete package:

```kotlin
import com.minhpd.weatherapp.domain.model.Forecast as ModelForecast
```

* To convert the forecast list from the data to the domain model:

```kotlin
return list.map { convertForecastItemToDomain(it) }
```

In a single line, we can loop over the collection and return a new list with the converted items.

### 6.3. Drawing the data in the UI

```kotlin
override fun onBindViewHolder(holder: ViewHolder,
            position: Int) {
    with(weekForecast.dailyForecast[position]) {
        holder.textView.text = "$date - $description - $high/$low"
    } 
}
```

_```with``` is a useful function included in the standard Kotlin library. It basically receives an object and an extension function as parameters, and makes the object execute the function. This means that all the code we define inside the brackets acts as an extension function of the object we specify in the first parameter, and we can use all its public functions and properties, as well as this. Really helpful to simplify code when we do several operations over the same object._

## 7. Operator overloading

Kotlin has a fixed number of symbolic operators. A function with a reserved name that will be mapped to the symbol.

To overload an operator, functions must be annotated with the ```operator``` modifier

### 7.1. Operators tables

#### **Unary operations**

| Operator | Function |
| ----------- | ----------- |
| +a | a.unaryPlus() |
| -a | a.unaryMinus() |
| !a | a.not() |
| a++ | a.inc() |
| a- | a.dec() |

#### **Binary operations**

| Operator | Function |
| ----------- | ----------- |
| a + b | a.plus(b) |
| a - b | a.minus(b) |
| a * b | a.times(b) |
| a / b | a.div(b) |
| a % b | a.mod(b) |
| a..b | a.rangeTo(b) |
| a in b | b.contains(a) |
| a !in b | !b.contains(a) |
| a += b | a.plusAssign(b) |
| a -= b | a.minusAssign(b) |
| a *= b | a.timesAssign(b) |
| a /= b | a.divAssign(b) |
| a %= b | a.modAssign(b) |

#### **Array-like operations**

| Operator | Function |
| ----------- | ----------- |
| a[i] | a.get(i) |
| a[i, j] | a.get(i, j) |
| a[i_1, ..., i_n] | a.get(i_1, ..., i_n) |
| a[i] = b | a.set(i, b) |
| a[i, j] = b | a.set(i, j, b) |
| a[i_1, ..., i_n] = b | a.set(i_1, ..., i_n, b) |

#### **Equals operation**

| Operator | Function |
| ----------- | ----------- |
| a == b | a?.equals(b) ?: b === null |
| a != b | !(a?.equals(b) ?: b === null) |

Operators === and !== do identity checks (they are == and != in Java respectively) and can’t be
overloaded.

#### **Function invocation**

| Operator | Function |
| ----------- | ----------- |
| a(i) | a.invoke(i) |
| a(i, j) | a.invoke(i, j) |
| a(i_1, ..., i_n) | a.invoke(i_1, ..., i_n) |

### 7.2. Operators in extension functions

We can extend existing classes using extension functions to provide new operations to third party libraries. For instance, we could access to ViewGroup views the same way we do with lists:

```kotlin
operator fun ViewGroup.get(position: Int): View 
    = getChildAt(position)
```

Now to get a view from a ViewGroup by its position:

```kotlin
val container: ViewGroup = find(R.id.container)
val view = container[2]
```

## 8. Lambdas

* A lambda expression is a simple way to define an anonymous function.
* Prevent us from having to write the specification of the function in an abstract class or interface, and then the implementation of the class.
* In Kotlin, we can use a function as a parameter to another function.

### 8.1. Simplifying setOnClickListener()

To implement a click listener behaviour in Java, we first need to write the OnClickListener interface:

```java
public interface OnClickListener {
    void onClick(View v);
}
```

And then we write an anonymous class that implements this interface:

```java
view.setOnClickListener(new OnClickListener() {
    @Override
    public void onClick(View v) {
        Toast.makeText(v.getContext(), "Click", Toast.LENGTH_SHORT).show();
    }
});
```

This would be the transformation of the code into Kotlin (using Anko toast function):

```kotlin
view.setOnClickListener(object : OnClickListener {
    override fun onClick(v: View) {
        toast("Click")
    }
}
```

Any function that receives an interface with a single function can be substituted by the function.

```kotlin
fun setOnClickListener(listener: (View) -> Unit)
```

A lambda expression is defined by the parameters of the function to the left of the arrow (surrounded by parentheses), and the return value to the right. In this case, we get a ```View``` and return ```Unit``` (nothing).

So with this in mind, we can simplify the previous code a little:

```kotlin
view.setOnClickListener({ view -> toast("Click")})
```

We can even get rid of the left part if the parameters are not being used:

```kotlin
view.setOnClickListener({ toast("Click") })
```

If the function is the last one in the parameters of the function, we can move it out of the parentheses:

```kotlin
view.setOnClickListener() { toast("Click") }
```

And, finally, if the function is the only parameter, we can get rid of the parentheses:

```kotlin
view.setOnClickListener { toast("Click") }
```

### 8.2. Click listener for ForecastListAdapter

In Java

```java
interface OnItemClickListener {
    operator fun invoke(forecast: Forecast)
}

class ViewHolder(view: View, val itemClick: OnItemClickListener) :
        RecyclerView.ViewHolder(view) {
            ...
}

public class ForecastListAdapter(val weekForecast: ForecastList,
         val itemClick: ForecastListAdapter.OnItemClickListener) :
        RecyclerView.Adapter<ForecastListAdapter.ViewHolder>() {
    ...
}

forecastList.adapter = ForecastListAdapter(result,
        object : ForecastListAdapter.OnItemClickListener{
            override fun invoke(forecast: Forecast) {
                toast(forecast.date)
            }
        })
```

Using lambda:

```kotlin
public class ForecastListAdapter(val weekForecast: ForecastList,
                                 val itemClick: (Forecast) -> Unit)
```

The function will receive a forecast and return nothing. The same change can be done to the ```ViewHolder```:

```kotlin
class ViewHolder(view: View, val itemClick: (Forecast) -> Unit)
```

Then:

```kotlin
val adapter = ForecastListAdapter(result) { forecast -> toast(forecast.date) }
```

In functions that only need one parameter, we can make use of the **_it_** reference, which prevents us from defining the left part of the function specifically.

```kotlin
val adapter = ForecastListAdapter(result) { toast(it.date) }
```

### 8.3. Extending the language

Check 13.3 Extending the language in the book for further details






