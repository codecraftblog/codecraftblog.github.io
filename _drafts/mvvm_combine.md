---
layout: post
title:  "MVVM Combine"
date:   2021-12-01 00:16:00 +0530
categories: iOS Swift 
author:
  name: Prashanth 
  twitter: codecraftblog 
---

### Introduction is MVVM


Strucutare our code 

Compartmentalize
Strucutare our code 

Moularity 
Cohesion 

Separation of concerns : Defining bounddarids.. and defending them.
Coupling : 

Organize our code 

Benefits :
Easy to change :

Models : 

## MVVM Overview 

Structural Pattern
3 Main layers 
- **Model** - Contains the business logic
- **View** - User interace of the app 
- **ViewModel** - Coordintates between dataflow and events between view and model

<!-- 
![MVVM Componenets](/images/mvvm_combine/mvvm_components.png)
-->

<figure>
<img src="/images/mvvm_combine/mvvm_components.png" width="400">
  <figcaption>Fig 1. Componets of MVVM pattern</figcaption>
</figure>

figure. 1 shows how the different compoenets are related to each other.

MVVM pattern also defines the rules of interactions between the compnenets, represented by the arrows in figure 1.
Both the direction of the arrorw indicates how the componenets communicate with each other.
The - - -> ---> solid arrows show the nature of the dependency.

Prashanth we need to get the terminology right, layers, componenets etc.

Let look at each of these layers/componenets? To better understand how things work, we look at some code in parallel. 

We will build a simple counter app.

<figure>
<img src="/images/mvvm_combine/timer_final.gif" width="400">
  <figcaption>Fig 1. Componets of MVVM pattern</figcaption>
</figure>

Here we have a UI :
- Label : Shows the current value of the counter.
- + button, that increments the counter.
- - button, the decrements the timer, it has an addtion behvaiour, when the value is zero, this button is disabled. 

## MVVM in detail

Lets look at the "M" in MVVM.


### Model
The Model layer, deals encapsulates all the business logic.

The Model layer contains all the domain or business rules. 
All code that deals with things like data access, networking etc. would all be considered model.

In the counter app, the domain/business rules is code that increments or decrements the counter value.

```swift

class Calculator {
  
   PassThrough value subjedt

   func addOne(currentValue: Int) -> Int { ... }
   func minusOne(currentValue: Int) -> Int { ... }
}

```

1. Does not know anything about the counter app. 
2. Does not store State or have any context 

In MVVM model layer is completely unaware of the view or the view model.

However, the model still needs a way to notify anyone who is interested. i.e Needs a way to return the result of a calculation : Observable, ie other objects can watch for changes.

As show in fig1. the arrow communicates to the view model indirectly. Observalbe, pub sub.. that pattern.

### View 

View - Is the user inteface of the app. Its all the UI components such as labels, buttons, textfieds etc.

In the MVVM pattern, views are the "lightest" component. Their role is limited to :
 - Display Information & react to changes in that information.
 - Optionally, Detect User interaction events (eg: Tap, Swipe etc) and pass that information to the view model for processing.

View binds or map to properties on the view model. i.e when the property they are bound to changes, that specific portion of the view reloads. i.e you need field level synchronisation.

Dont store state : For eg : In the counter app the cuurent value of the count i stored in the view, but we should not rely on it and state should be stored outside the view.

Testability : Keeping the view light weight and dumb, ensure the tested independently

Views can be developer indepenntly and swapped out. Use any UI framework, SwiftUI vs UIKit.

Each view should depend on only one view model.

Might have User actions, passes back to the view model. View should not contain any logic, even if that logic is related to the view. say for eg: YOu have a swicth based on which some fields the UI are enabled or disabled, it might be tempting to add that logic in the view (Foreg: when user turns off a switch, you set the isenabled state)but the right way is to pass the action back to the view model and let a property onthe view model.


## View-Model

Central component in MVVM Pattern

While the view and Model are fiercely independent, the view model is more affable, it acts as the interface between the view and model.

In its simplest from a View Model is a value converter, i.e it converts values to a value that can be consumed by the view.

Lets consier a simple .. no button.. in that case.. the modle pulishes a Int, but that view needs a String.

View model.. converts the int to a string and maps it to the view.

Example 1 : 
In a simple version of the counter app, lets say we just show the value.
Here, the model returns an Int, but the view needs a String, here he VM acts a value converter.

In a more realistic case, the views not only display informaiton, but they also allow users to interact with them.
The View controller in MVVM, interpretes the view  

Also the view model is reponsible for manageing state. i.e it has an idea of the current context etc.

Note, a view model might work with many differnet views.

Updates the model

Co-ordinates between view and view model

Back to our counter example.
Here our View model has 
Here the controller not only responds to the view but also 

```swift
var counterValue : CurrentValueSubject<Int> 
func bind()

```
in a sense the VM is a differnet representation of the view, i.e in our case the value of `count` tell us what the value displaye don the UI woulud be.

## Why MVVM? 

