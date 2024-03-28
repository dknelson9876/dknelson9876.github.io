---
Title: Java Friendly Kotlin Code
Date: 2023-03-09
Tags: kotlin
Summary: Notes on making kotlin code behave better when used alongside java
---
> [Calling Kotlin Code from Java Codelab](https://developer.android.com/codelabs/java-friendly-kotlin)

## JVM Tags

- **`@JvmStatic`**: When creating a singleton Kotlin class using the `object` keyword, the instance is created inside the class as `<ClassName>.INSTANCE`, unless we specify property or method as `@JvmStatic`, then we'll be able to access it the more typical way without the `INSTANCE`
- **`@file:JvmName`**: When creating extensions in Kotlin, a new class name is created based on the file name for the JVM, such as `StringUtilKt`, unless we specify a name to use
- **`@JvmOverloads`**: When using default values in a Kotlin method, we won't be able to access them in Java because it doesn't support defaults, unless we specify to the compiler to create all possible overload methods
- **`@get:JvmName`**: When the compiler generates getters and setters for properties, it will prefix the name of the property with 'get' and 'set', but if a different name makes more sense, we can specify one
    > with the notable exception of a property that begins with `is`, in which case the getter will have the same name
- **`@JvmField`**: Default Kotlin behavior is to generate a getter and setter for each property, but in the case that we want the backer field to be directly available (i.e. `user.displayName`, over `user.getDisplayName()`), we can specify such
- **`@Throws`**: Something something Java has checked exceptions and Kotlin doesn't, so declare them