---
layout: post
title:  "MVVM using Combine Swift"
date:   2022-06-20 22:45:00 +0530
categories: iOS Swift MVVM Architecture 
author:
  name: Prashanth 
  twitter: codecraftblog 
---

Our code is a living entity. We constantly tinker with it. We add, move, delete and rearrange parts of our code in the hope of making it better.

The ease & confidence with which we can change our code, depends a great deal on how it is orgnized. 
Architectural patterns are time tested solutions help us and anyone working our code understand it better.

Patterns do impose some restrictions on what we can and cannot do, but in return our code is easy to reason about, safe to change and easy to test.

>An architectural pattern is a general, reusable solution to a commonly occurring problem in software architecture within a given context. - Wikipedia

In this post, we look at a structural design pattern called *MVVM* or *Model-View-ViewModel*. 

We answer the following questions.
1. What is the MVVM?
2. What is the roles of Model View View-Models?
3. Example: MVVM in action.

To help understand the pattern better we build a simple iOS app using Swift and Apple's Combine framework.

<!--more-->

<br>

## MVVM Introduction

When it comes to building GUI based applications there are many popular patterns. A few examples are: 

- **MVC** - Model-View-Controller
- **VIPER** - View-Interactor-Presenter-Entity-Router
- **MVVM** - Model-View-ViewModel

This post looks at the MVVM pattern.

The Model-View-ViewModel (MVVM) is a structural design pattern. Structural design patterns guide us on how to organize different entities in our app and how different entities interact with each other.

The main goal of MVVM is *separation of concerns*. i.e it helps organize our code in a way that each part of our code deals with a coherent & well defined set of functions.

MVVM splits our application to three parts
 * **Model** - Contains the Data & business logic of our app. 
 * **View** - User interface of the app
 * **ViewModel** - Entity that co-ordinates data flow and events between the Model and the View.

MVVM pattern also defines the rules of interactions between the three parts of the app. i.e. which component can interact with which other component. 

<figure>
<img src="/images/mvvm_combine/MVVM_Image1.png" width="400">
  <figcaption>Fig 1. Components of MVVM pattern</figcaption>
</figure>

fig. 1 shows how the different parts are related to each other.
In fig. 1 the arrows indicate how the the different components talk to each other.

Boxes shows the components and lines shows the rules of engagement between them.
Direction of the arrow - Indicates the direction of the dependency. Direction of communication.
Type of arrow :
     - - -> Loosely coupled.
     -----> Solid arrow -- losely bound.


As described in fig 1, the Model and View never talk to each other directly. 
i.e they are completely indpendent of each other. This structure implies the model and view can change without affecting each other.

The MVVM pattern also ensures that conforms to the [dependency rule](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html). 
The Dependency Rule. This rule says that source code dependencies can only point inwards. That is lower level components dont know anything about higher level components.

<figure>
<img src="/images/mvvm_combine/onion_dependency_diagram.png" width="400">
  <figcaption>Fig 2. Dependency Onion Diagram</figcaption>
</figure>

The green arrows shows how the direction of the depenecy. The UI layer depends on the inner layer, but the inner most layer Model does not depend on any of the out layers.

Model is lower level components that be consumed by many different componenets... think of api.. it really does not care who is using it.. 

This aligns nicely with the onion diagram, where the outers layers, like UI depend on innner layers, but innner layers do not depend on outer layer.

<br>

## OK.. fine.. Show me the benefit?

Separate presentation
    - Loosesly coupled : Can build and test UI separately
    - Single Resp Principle
        -  Views : only change when there is achange to UI and user interaction
        -  Model : Only changes when some domain secific behavior changes.
        -  VM changes : When only some change in view affects the odel and some model affects views... other wise it need not change.


Dependencies :
    All Dependencies point inward.. <Prashanth> Show the onion diagram here again. with arrows.. 
    Clean Architecture : All lower level layers know nothing about higher level layers.
    "The goal, as always, is to have a codebase that is loosely coupled and high cohesive, so that changes are easy, fast and safe to make."


## MVVM Deep Dive : Example

<!-- Introduce the counter example -->

Lets build a simple app that displays the price of our currency Dodgy Coin vs the US Dollar. 

<figure>
<img src="/images/mvvm_combine/sample_app_pricefetcher.jpg" width="400">
  <figcaption>Fig 3. Sample app MVVM pattern</figcaption>
</figure>

If we break down the UI, we find this.

1. We have a card title, that does not change.
2. One label that shows the price and changes everytime the card gets a new price.
3. A box at the that displays the change in price. It also shows a background box, that is either red or green or yellow 
depeding on the direction of price change.
4. Refresh button, that when tapped fetches the latest price.

Lets look at this through the lens of MVVM

#### Model : Fiercely independent.
The Model layer contains all the domain or business rules. In the counter app, the domain/business rules is the code that  increments or decrements the counter value.

Typically, the model layer would work with other components for data access, networking etc...

Important : Does not store state or context. In our counter app, the model layer does not keep a tab of the current value of the counter.

Relation to other components in MVVM : Model layer not know about any of the other components.

However, the model still needs a way to notify anyone who is interested. i.e Needs a way to return the result of a calculation : Observable, ie other objects can watch for changes.

Tells other ugys.. hey ia m observable .. ie. you can watch me I dont care.. i will publish values. you can listen to if you want.

<!-- Image of MVVM hilighting only the model -->

Sample Code :
Model : Sample Code  : 

```swift
class CounterModel {
    
    func incrementCounter(currValue: Int) -> Int {
        return currValue + 1
    }
    
    func reduceCounter(currValue: Int) -> Int {
        return currValue - 1
    }
}

```

CounterModel is a model object that encapsulates calculation logic.
In our case Counter model has two methods that 

Few things to note
- Idp

Takes in a number and returns the next number. (Pure function - that has no side effects)

<!-- In our case we dont need this..  -->
Has a published property that publishes the latest value.
    - Passthrough Subject : It just publishes the latest value.


<br>
<br>


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

Important
1. Does not know anything about the counter app. 
2. Does not store State or have any context 

In MVVM model layer is completely unaware of the view or the view model.

However, the model still needs a way to notify anyone who is interested. i.e Needs a way to return the result of a calculation : Observable, ie other objects can watch for changes.

As show in fig1. the arrow communicates to the view model indirectly. Observalbe, pub sub.. that pattern.


Points to note : Knows nothing about the outside, just specializes in add and subtract. and publishes a value. Ensures it s loosely coupled and therefore easy to isolate and test.
So model matches our goal, easy to test and islolated .. etc.

---

<br>
<br>


### View

User interface of our app, everything that is presented to the User... note that is need not be a visible. thereore presentation layer but be better option.
eg: Audio Graphs https://developer.apple.com/documentation/accessibility/audio_graphs

View - Is the user interface of the app. Its all the UI components such as labels, buttons, textfieds etc.

In the MVVM pattern, views are the "lightest" component. Their role is limited to :
    - Display Information & react to changes in that information.
    - Detect User interaction events (eg: Tap, Swipe etc) and pass that information to the view model for processing.

Does not have any logic, In a sense this is the dumbest component : Does 2 things :
   - 1. If a proeprty its bound mapped to changes, the view updates to reflect the new value.
   - 2. If user interacts with view the interaction is communicated with to the view model for interpretation.

Things to remmeber :
View binds or map to properties on the view model. i.e when the property they are bound to changes, that specific portion of the view reloads. i.e you need field level synchronisation.
Does not store state. i.e in our case, does not store the value of the counter.
iDont store state : For eg : In the counter app the cuurent value of the count i stored in the view, but we should not rely on it and state should be stored outside the view.

Testability : Keeping the view light weight and dumb, ensure the tested independentlyi

Views can be developer independently and swapped out. Use any UI framework, SwiftUI vs UIKit.

What are the Dependencies?
Depenedencies
- Binds to view to model to get values
- Passes user interaction back to the viewmodel for interpretation.i
- Each view should depend on only one view model.


<br>
<br>
--------

### View-Model
>>>>>>> mvvm-combine


### View-Model

The ViewModel plays a vital role in the MVVM pattern. Does the heavy lifting. Unlike the Model and View entities that are fiercely independent, the ViewModel is more affable; it interacts with both view and model entities.

The ViewModel does most of the heavy lifting in the MVVM pattern. Unlike the Model and View entities that are fiercely independent, the ViewModel is more affable; it interacts with both view and model entities.

The ViewModel is an alternate representation of the View and behavior of the UI.
By looking at the ViewModel we should be able to validate all the data independent of the View. This enables us to test the correctness of the data just by looking at the view model. 

> Represents the state and behavior of the view independently of the GUI controls used in the interface-  Martin Fowler

It is easier to consider Presentation Model as an abstract of the view that is not dependent on a specific GUI framework. While several views can utilize the same Presentation Model, each view should require only one Presentation Model. - Martin Fowler

In its simplest form, a ViewModel is a value converter. It simply converts values provided by the Model to a value that can be consumed by the view. 
For eg: In our price card example. The model publishes the price of type `Double`, and the view model converts the `Double` to a `String` which is consumed by the View.

⚠️  The view should not handle type conversion etc. The View model is the right place.

In most scenarios, ViewModel also stores states, updates the model, and handles user interaction. 
In its simplest form just a mapping. But will likely have logic for data transformation, persistence, transformation, can have all the logic

#### View Model acts as a value converter. 

Exposes a simplified unified view. for eg it can talk to more than one domain object... only those properties & behaviors are relevant to the view in question.

#### View Model stores the state 

Any information that we want to keep around is stored in the ViewModel.
Stores State: Only component in MVVM that has context/stores state.

In our example, the PriceModel publishes the latest price, and it's done. It does not keep track of the last published price.
However, we want to show the difference and direction of price movement. We use the ViewModel to store the last displayed price and when we get a new updated price we do a comparison in the ViewModel. 

#### View Model handles user interaction 

The view model is also the place to handle user-initiated action. i.e the View intercepts user interaction and communicates it to the ViewModel. 
The view model then decided how that action is to be interpreted.

When the user taps the refresh button, the view merely communicates to the ViewModel that the refresh button was tapped. i.e it calls the `refresh` method. 
The view model then decides if this request should be forwarded to the model and then further sets the view state when the data is being fetched.
<!-- Prashanth adds the debounce here? To show that the view model can reject handling a user interaction? -->

<!-- Show mapping between properties in the view and view model. -->

Exposes a simplified unified view. for eg it can talk to more than one domain object... only those properties & behaviors are relevant to the view in question.

#### Interactions with other componenets

#### Co-ordinates between view and view model

#### Interation between ViewModel and Model

#### Interaction between ViewModel and View
View binds to ViewModel.
Recives events .. and hanldes it.
Does not in anyway refer the view. This enables you to bind the same view model to different views.. which helps you swap out views easily? For eg: SwiftUI vs UIKit etc.

One view should have only one viewModel
One viewModel can work with multiple views 




<br>
<br>



### Summary: 

GUI Pattern Architecture: https://martinfowler.com/eaaDev/uiArchs.html#Model-view-presentermvp

References: GUI Pattern Architecture: https://martinfowler.com/eaaDev/uiArchs.html#Model-view-presentermvp
