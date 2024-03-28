---
Title: Flutter personal Reference
Date: 2022-02-27
Tags: flutter
Summary: Notes from learning Flutter
---

# Flutter personal Reference

## One and done packages
- IntroductionScreen
- flutter_native_splashscreen
- AnimatedIcon
- InteractiveViewer (for pinch/zoom)

## Operators
- `...` is the 'spread operator'

## Converting to a Provider
Converting to Provider instead of custom InheritedWidget:

- I care about providing the signed in user and the list of recipes, and the isLoading bool
- I need to be able to notify anything about the user signing out
- I need to be able to notify about a recipe being uploaded
- I need to be able to notify about a recipe being favorited/unfavorited

- A class needs to extends ChangeNotifier to be able to call notifyListeners()
- ChangeNotifierProvider goes above the widgets that need access to it, but no higher than necessary (so app, I think)
- ChangeNotifierProvider takes a builder for the model (the thing that extends ChangeNotifier [I think this will end up being new recipes list model?])
- Use MultiProvider to have multiple Providers (I think I need one for user and one for recipe)
- We use a Consumer inside the widget to gain access to the model. The Consumer uses a builder to rebuild the widget according to the provider
- The consumer's builder gives the (context, ChangeNotifier[Model], 
- Good practice to put the Consumer as deep as possible

- If you need access to the model, but not the data inside it, use Provider.of<>