---
Title: ViewModel
Date: 2022-07-26
Tags: 
- kotlin
Summary: Notes on how the ViewModel architecture works in kotlin
---
An architecture practice for separating data from UI. By extending the `ViewModel` class and instantiating via the `by viewModels()` *delegate*, data inside the ViewModel is automagically retained through configuration changes (Orientation, light/dark mode, etc.)

```
+--------------------------------+------------------------------------------+
|      UI Responsibilities       |       `ViewModel` Responsibilities       |
+--------------------------------+------------------------------------------+
| `Activity` and `Fragment` are  | `ViewModel` is responsible for holding   |
| responsible for drawing views  | and processing all the data needed for   |
| and data to the screen and     | the UI. It should never access your view |
| responding to user events      | hierarchy (like view binding object) or  |
|                                | hold a reference to the activity or the  |
|                                | fragment.                                |
+--------------------------------+------------------------------------------+
```

### Sidenote: Delegates
> **Delegates**: Property delegation in Kotlin helps you by handing off the getter-setter functionality to a different class. Defined using the *`by`* keyword. In this case, the `viewModels` delegate is what does the retaining through configuration changes.
>```kotlin
> var <property-name> : <property-type> by <delegate-class>()
> //For example:
> private val viewModel: GameViewModel by viewModels()
>```

## Backing Properties
The correct practice for giving a private property a public getter that still limits access to the actual value. The convention is to prefix the private property with _
```kotlin
private var _count = 0

val count: Int
    get() = _count
```

Thus, a `ViewModel` can hold the actual value, and the `View` that holds the `ViewModel` can access the property in a safe manner

> **Warning**: Use this practice to ensure that mutable data inside your `ViewModel` is always private

## `ViewModel`'s Lifecycle
The Android framework will keep the `ViewModel` alive as long as the scope of the `Activity` is alive, including a new instance of an `Activity` reconnecting to an existing `ViewModel`
<img src="https://developer.android.com/static/codelabs/basic-android-kotlin-training-viewmodel/img/18e67dc79f89d8a.png" width="500" />

### Sidenote: `lateinit`
In Kotlin, the `lateinit` keyword is used to promise the compiler that despite not giving a variable an initial value, you will give it one before trying to access it (typically inside `init`). Breaking this promise will cause the app to crash

## Adjacent: `MaterialAlertDialog`
A dialog is a popup window in the middle of the screen. Use `MaterialAlertDialogBuilder` to setup and configure the things that you want about the dialog, and calling `show()` at the end. Some of the configuration options include:
- `setTitle(String)`
- `setMessage(String)`
- `setCancelable(Boolean)`
- `setNegativeButton(String, DialogInterface.OnClickListener)`
- `setPositiveButton(String, DialogInterface.OnClickListener)`

## Adjacent: `TextInputLayout` Error Options
Material text fields contain built in ways to show something wrong with a text field, using the `isErrorEnabled` property in conjunction with `error`
