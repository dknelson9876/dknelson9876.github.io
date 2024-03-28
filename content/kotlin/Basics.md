---
Title: Kotlin Basics
Date: 2022-07-26
Tags: kotlin
Summary: The absolute basics of using Kotlin
---
 > [Learn Kotlin by Example](https://play.kotlinlang.org/byExample/overview)

# Introduction

## Functions
```kotlin
fun aFunction(arg1: Arg1Type, optionalArg: OptionalArgType = defaultVal): ReturnType {
// ...
}

fun printStuff(stuff: String, prefix: String = "Info") {
    print("[$prefix] $stuff")
}

fun main() {
    // Many valid ways to call printStuff
    print("thing")
    print("thing", "hello")
    print(stuff = "thing", prefix = "hello")
    print(prefix = "hello", stuff = "thing")
}
 ```

 ## [Infix Functions](https://kotlinlang.org/docs/functions.html#infix-notation): `infix`
 Member and extension functions with a single parameter can be turned into `infix` functions (calling it doesn't require a dot or parentheses)
 > Almost in a way making your own keywords? Probably how Boolean or is just '`or`'
```kotlin
infix fun Int.times(str: String) = str.repeat(this)
println(2 times "Bye ")
```
> Bye Bye

## [Operator Overloading](https://kotlinlang.org/docs/operator-overloading.html)
In an addition to infix functions, using the `operator` keyword with some special names will overload an operator symbol
```kotlin
operator fun Vector.plus(v2: Vector) = Vector(this.x + v2.x, this.y + v2.y)
println(aVector + anotherVector)
```

## Varargs
A funky kind of inline array? Come back to this. 

The mechanism by which `listOf()` works, I think

## Class Declarations
In Kotlin, both the header and the body are optional, since the compiler can build the constructor and appropriate getters/setters for you
```kotlin
// All valid class declarations

class Customer
// a default constructor is inferred

class Contact(val id: Int, var email: String)
// builds a constructor

fun main() {
    val contact = Contact(1, "sample@example.com")
    println(contact.email)
    contact.email = "steve@website.com"
    println(contact.email)
}
```
> sample@example.com
> steve@website.com

#### Note: `new` keyword doesn't exist in Kotlin

## Inheritance
Classes and functions are *final* by default in Kotlin. If you want one to be able to be overriden, mark it with the `open` keyword.
```kotlin
open class Dog {
    open fun sayHello() {
        println("woof")
    }
}

class Yorkshire : Dog() {
    override fun sayHello() {
        println("yip")
    }
}

fun main() {
    val dog: Dog = Yorkshire()
}
```

# Control Flow
To be continued...