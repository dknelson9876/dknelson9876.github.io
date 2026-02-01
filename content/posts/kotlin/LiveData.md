---
Title: LiveData
Date: 2022-07-26
Tags: 
- kotlin
Summary: Notes on using the LiveData type and associated other objects in kotlin
---
An *observable* data holder class that is lifecycle aware. 
- It holds data, and is designed to serve as a *wrapper* for any type of data
- It is *observable*, so another class can sign up to be notified when the data being held changes
- It is *lifecycle-aware*. When a class signs up to observe `LiveData`, it is associated with a **`LifecycleOwner`**. The `LiveData` then only updates observers that are in an active lifecycle state.

## `MutableLiveData`
The mutable version of `LiveData`. Because `LiveData` is an object, when we want to access the data that is being stored, use the `value` property of the `LiveData`. Common practices recommend having a `MutableLiveData` backing object, with a public `LiveData`
```kotlin
private val _word = MutableLiveData<String>()
val word: LiveData<String>
    get() = _word
//...
fun ...(){
    _word.value = "..."
}
```

## Attaching an observer
```kotlin
    viewModel.word.observe(viewLifecycleOwner) { newValue -> {}}
```
- `viewLifecycleOwner` is how the `LiveData` is aware of the lifecycle state of the view, and is inherited from the Activity/Fragment
- The second parameter is a lambda that provides the updated value for whatever logic it is you wanted to do with it.

## Data Binding
Allows you to access `LiveData` objects directly in the .xml view files. The root element of the view you want to use data binding in should be a `<layout>` and should also contain a `<data>` element before the body of the view. In the class that corresponds to the view, inflate it using the `DataBindingUtil.inflate()` method.

Inside the `<data>` tag, include `<variable>` tags that you then initialize inside the `onViewCreated` callback of the associated class. Be sure to also provide the binding with the lifecycleOwner, so that it can observe `LiveData` correctly.
```xml
<data>
    <variable
        name="max"
        type="int" />
</data>
```
```kotlin
override fun onViewCreated(...){
    ...
    binding.max = ...
    binding.lifecycleOwner = viewLifecycleOwner
}
```

Once you have done that, you can reference those variables inside the rest of your xml view using *binding expressions*
```xml
<TextView 
    ...
    android:text="@{viewModel.name}" />
```
