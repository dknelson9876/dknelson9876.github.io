---
Title: Jetpack Compose Navigation
Date: 2022-07-25
Tags: 
- kotlin
Summary: Navigation inside an app based on Compose
---
## `NavController`
The central component for navigation in Compose:
- tracks back stack composable entries
- moves the stack forward
- enables back stack manipulation
- navigates between destination states

Because of how central it is, it is the first step for setting up navigation.

The `NavController` is obtained by calling `rememberNavController()`. This should always be done in the top level of your composable hierarchy. 
```kotlin
@Composable
fun MyApp() {
    MyAppTheme {
        val navController = rememberNavController()
        // ...
        Scaffold(
            // ...
        ) {
            // ...
        }
    }
}
```

## `NavHost`
Part 2 of the 3 components required for navigation. The `NavHost` acts as a container and is responsible for displaying the current destination of the graph. Every `NavController` should always be associated with exactly one `NavHost`. It also serves to link the `NavController` to the third component, the `NavGraph`, which maps out the composable destinations to navigate between.

When creating the `NavHost`, you also must specify the `startDestination` route of which screen to display when the app is opened.
```kotlin
    Scaffold(...) {
        NavHost(
            navController = navController,
            startDestination = HomeScreen.route /*a string*/,
        ) {
            //...
        }
    }
```

## `NavGraph`
Part 3 of 3. Acts as the destination/route handler, and is defined using the `NavGraphBuilder.composable` extension function. Each screen needs to individually have it's route and matching screen added.
```kotlin
NavHost(...){
    composable(route = HomeScreen.route){
        HomeScreen()
    }
    composable(route = SettingsScreen.route){
        SettingsScreen()
    }
}
```

From here, you can react to an appropriate button press within your app by calling
```kotlin
navController.navigate(newRoute)
```

## Navigation Options
These come from the `NavOptionsBuilder` class, and are for use with calls to `navController.navigate()`
- `launchSingleTop` : ensures that only one instance of each screen exists. If false, each call to `navigate()` will launch a new instance
- `popUpTo(route) {saveState = true}` : pops up to the beginning of the back stack, removing long chains of screens.
- `restoreState` : should state (such as scroll position) be saved and restored when navigating between screens?

## Passing Screen Changes
In the cases where you need to know the current screen (like a custom tab row that shows the selected tab a different color), we can get the top of the backstack from the `NavController`. Because this is returned as a `NavDestination`, you'll also need a way to connect it to one of your screens (i.e. using the `route` String as a psuedo id).

```kotlin
val navController = rememberNavController()
val currentBackStack by navController.currentBackStackEntryAsState()
val currentDestination = currentBackStack?.destination
```

> #### Aside: `list.find`
> A handy shortcut Kotlin method
> ```kotlin
> val currentScreen = TabRowScreens.find {it.route == currentDestination.route} ?: Overview
> ```
> is equivalent to 
> ```kotlin
> var currentScreen = Overview
> for (screen in TabRowScreens) {
>    if (screen.route == currentDestination.route)
>       currentScreen = screen
> }
> ```

## Navigation Arguments
