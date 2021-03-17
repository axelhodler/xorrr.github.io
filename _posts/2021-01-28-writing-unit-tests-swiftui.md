---
layout: post
title: "Writing Unit Tests for SwiftUI Views"
date:   2021-01-28 09:22:22
description: ""
tags: []
---

Unit tests help us to make sure our app works as intended. Without having to start the app and navigating manually through it after every change. In comparison with UI tests they __test single views in isolation__ instead of a full user flow. They are a lot faster and focused.

To start off easy we use `ContentView`. It’s initially generated when creating a new project and draws the String `"Hello, world!"` in the center of the screen. It’s SwiftUI code.

<script src="https://gist.github.com/axelhodler/d06cb1517968105ed50b37c790ff10ea.js"></script>

How would we test whether it draws the String `"Hello, world!"`? One way is to create a UI Test as one could have done for UIKit.

Another approach is to use the library [ViewInspector](https://github.com/nalexn/ViewInspector).

> ViewInspector is a library for unit testing SwiftUI views. It allows for traversing a view hierarchy at runtime providing direct access to the underlying View structs.

## Testing Static Views

In test test we first create the subject under test. In our case `ContentView`. We access the `text` of the `Text` view via `inspect().text().string()` and are able to assert on it. The test passes.

<script src="https://gist.github.com/axelhodler/92d88a23e9b1b5a3ab191e12545d8437.js"></script>

`ContentView` adopts the `Inspectable` protocol to enable the magic of the library to do its work.

Let’s add some additional elements to our `ContentView`. Aside from `"Hello, world!"` there will be a `navigationTitle` stating `"Greetings"`.

<script src="https://gist.github.com/axelhodler/4b501edea9cf3df1738ee7ac01c84f3c.js"></script>

If we rerun our initial test it will fail.

![xcodeerror](https://www.dropbox.com/s/11bvwqptb88mcqc/xcode_error.png?raw=1)

Indeed `inspect().text()` will fail. The __view hierarchy has changed__. It should be `inspect().navigationView().text(0)`.

<script src="https://gist.github.com/axelhodler/103380296cd709a5547d04b8722812cd.js"></script>

`text(0)` means the `Text` view is the first element in our `NavigationView`. The test passes.

Let’s take a step back. The user was still greeted with `"Hello, world!"`, but the __test needed to be changed because the view hierarchy has changed__. The test is tightly coupled to the view. The act of changing the test when the view changes is something often used as an argument against testing.

To alleviate the issue, we extract a `GreetingsView`.

__Breaking up our views into smaller building blocks is a great practice__ — for readability, composability, and as we will soon see, for testing purposes.

<script src="https://gist.github.com/axelhodler/86df175edb2ec8287d8b90ff55020c2d.js"></script>

We add a test:

<script src="https://gist.github.com/axelhodler/afc0779fe9b991aab6d8e5bf647c4313.js"></script>

It looks pretty much the same as our initial `ContentView` and `ContentViewTests`. We start using the new `GreetingsView` inside of `ContentView`.

<script src="https://gist.github.com/axelhodler/5e226103f0860cfac826df7548c4fe6b.js"></script>

Our `Text` view is now part of `GreetingsView`. It’s time to adapt the tests for `ContentView`.

<script src="https://gist.github.com/axelhodler/8f59d925d6d9fb9e5e75c799ee29010f.js"></script>

Both our `GreetingsViewTests` and `ContentViewTests` pass. One issue though: We test the same thing twice. Both `ContentViewTests` and `GreetingsViewTests` test whether the user is greeted with `"Hello, world!”`, with a test `greetsWithHelloWorld`.

It's test duplication. Instead, we can test whether the `GreetingView` is present in `ContentView` in `ContentViewTests`. Then we can test the wording the user is greeted with in `GreetingsViewTests`. We change the `ContentViewTests` to reflect that change.

<script src="https://gist.github.com/axelhodler/265457139ba2da11bb59d929593f80ec.js"></script>

To sum it up, we:
1. _Arrange_: Create the view under test
2. _Act_: Navigate the view hierarchy to get the element we are interested in
3. _Assert_: Use assertions on the element

## Adding Logic

Until now we have only tested static views. There might be an argument to make that testing static views makes no sense. After all, we can trust SwiftUI to display the text `"Hello, world!"` when using the view `Text("Hello, world")`. We could still use the tests as specifications to tell us what the view does, though. Instead of rendering the view or reading the implementation, we can read the names of the tests.

Besides, the views might not stay static for long. Logic is introduced and the view becomes dynamic.

Say we need two greetings, one for logged-in users and another for guests. `UserState` is created to store whether the user is `loggedIn` and the `userName`.

<script src="https://gist.github.com/axelhodler/cff796a230477e6327e8f98babe637be.js"></script>

We use the new `UserState` in `GreetingsView`. Let’s start with passing it into the constructor for testability.

<script src="https://gist.github.com/axelhodler/e788dfe6b4484837f3bcd1229e4ea406.js"></script>

And we change the tests to adapt to the new `UserState`.

<script src="https://gist.github.com/axelhodler/2ba2c5283fe9f840eeb50f6361355544.js"></script>

If we have a `loggedIn` user with the name `Peter`, he will be greeted with `Hello, Peter!`. If the app is used by a non-authenticated user, a guest, we will display `Hello, world!`.

## Using EnvironmentObject

What if we move the state into an `@EnvironmentObject` because we will use it in multiple views in our app?

Now `UserState` adopts `ObservableObject`.

<script src="https://gist.github.com/axelhodler/33eb1b5d5ca6c03a2cea44c1fdf4112b.js"></script>

The `EnvironmentObject` is created in our `@main`.

<script src="https://gist.github.com/axelhodler/0ece626c4306b83a81b5d4797b4c2d01.js"></script>

Then it’s used in `GreetingsView`.

<script src="https://gist.github.com/axelhodler/8814ab3ea800924e385b27a74fa22639.js"></script>

We need to add the `didAppear` functionality to allow `ViewInspector` to test the view. A synchronous test would not work. The tests look as follows:

<script src="https://gist.github.com/axelhodler/62faf3ff5ef60166409796200d7d00a7.js"></script>

Having to use asynchronous test syntax seems weird at first but is required. We would need to use an asynchronous test for `@State` and `@Environment` too. It’s not required for `@ObservedObject` or `@Binding` but should still be used for [test consistency reasons](https://github.com/nalexn/ViewInspector/blob/master/guide.md#views-using-observedobject).

Some might have an issue with having extra code for testability in the `GreetingsView`. Personally I think testability is a lot more important than the implementation aesthetics of the view.

Maybe Apple will offer a native testing solution for SwiftUI views in the future? Maybe we will have new tooling creating an initial test for every new view we create? Let’s see!

The post has been cross-posted on [Medium](https://betterprogramming.pub/writing-unit-tests-for-swiftui-views-11aa5eb48d8e)
