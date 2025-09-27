---
layout: post
title:  "SwiftUI - Router Setup"
---

## Creating a Router in SwiftUI

You might have learned OOP (Object-Oriented Programming), Functional Programming, and Imperative Programming. These influence how us developers write our code through concepts, principles, and certain practices called a "paradigm". Paradigm is a fundamental approach or a style of programming that serves as a framework for how to structure code and solve problems.

When we look back at how we have previously done routing navigation. It was done with the core purporse to go from view-to-view transition. But we could easily clean this up with a more centralize navigation route.

We are going to break this up into multiple different parts to make it easier for us to implement. 

First, let's start off with building the core component of our routing system.

### Core Router Class

```
class Router: ObservableObject {
    @Published var path = NavigationPath()
    // ... Methods
}
```
* ***class Router***: Defines a reference type for navigation management.
* ***ObservableObject***: Enables SwiftUI views to observe navigation changes
* ***@Published var path***: Creates an observable NavigationPath instance

## Method Creations

Second, let's setup some basic navigational methods to handle moving around our application.

### Navigation to Routes

```
func navigate(to route: Route) {
    path.append(route)
}
```
This method solves a few things for us. One it handles navigation in a forward path, it handles a new view by pushing it onto the navigation hierarchy, and lastly it adds a new destination to the navigation stack.

Imagine you’re in a big city. You don’t want to go back home, and you’re not just stepping back one block. Instead, you pull out your map and say:
➡️ “Take me to the coffee shop on 5th Street.”

### Navigation Back

```
func navigateBack() {
    path.removeLast()
}
```

With navigateBack method, this handles back navigation, pops to the previous screen, and removes the current view from the navigation stack.

Think about walking through a museum. You go from the lobby → dinosaur exhibit → ancient Egypt → modern art. At some point, you might want to go back one room, not all the way to the entrance.

That’s what “navigation back” is in coding: instead of restarting at the root (the lobby), you just rewind a single step to the previous screen.

### Navigation Root

```
func navigateToRoot() {
    path.removeLast(path.count)
}
```

Back to the beginning of your problems, the root! Joking! navigateToRoot method acts as a way to clean our entire navigation stack. It allows to return back to the beginning.

Imagine you’ve been wandering around a big shopping mall. You’ve gone up escalators, down hallways, into stores, maybe even down into the food court. Suddenly you think: “I just want to get back to the entrance.”

That entrance is the root. No matter where you are in the mall, if you keep following the “EXIT →” signs, eventually you’ll get back there.

In coding, “navigating to root” works the same way. If your app has multiple pages or screens — like Home → Profile → Settings → Security — you might find yourself four layers deep. If you decide you want to go back to the very beginning (the root), you don’t have to walk back step by step. Instead, you tell the system: “Take me straight to the entrance.”

### Pop to Specific View

```
func popToView(count: Int) {
    path.removeLast(count)
}
```

Imagine you’re on a subway ride. You got on at the first station (Home), then stopped at Profile, Settings, and finally Security. Now you realize you don’t want to go all the way back to the beginning — and you don’t just want to step back one stop either.

You want to jump directly from Security back to Profile, skipping all the stations in between.

That’s what “pop to a specific view” does. It clears everything between where you are and where you want to land.

### Route Extension
extension Router {
    func canNavigateBack() -> Bool {
        return path.count > 0
    }
}

Right now, when you use navigation in SwiftUI with a NavigationStack, your router.path is like your travel history — a stack of places you’ve visited. But typing out the low-level instructions every time can feel clunky.