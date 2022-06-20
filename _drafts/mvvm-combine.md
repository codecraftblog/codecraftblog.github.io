---
layout: post
title:  "MVVM using Combine Swift"
date:   2022-06-20 22:45:00 +0530
categories: iOS Swift MVVM Architecture 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

> _To improve is to change; to be perfect is to change often. - Winston Churchill_


Architecture Patterns : What are they and why should you care?

Our code is a living entity, we keep adding, moving, removing, rearranging it. It's always changing, and hopefully improving.

The ease & confidence with which we can change our code, depends a great deal on how the code is structured. Architectural patterns are time tested solutions that give us a out of the box solutions.. 

>An architectural pattern is a general, reusable solution to a commonly occurring problem in software architecture within a given context. - Wikipedia

In this post, we look at the Model-View-ViewModel design pattern.

We answer the following questions.
1. What is the MVVM?
2. What is the roles of Model View View-Models?
3. Example: MVVM in action.

In this post we look at the MVVM pattern in detail. We build a simple iOS app using Swift and Combine framework.

MVVM is a strutctural pattern that looks at our we organize our code.

<!--more-->

<br>
## MVVM Introduction

MVVM is a strutctural design pattern that looks at our we organize our code.

When it comes to building GUI based applications popular patterns are: 
- Model-View-Controller(MVC), 
- View-Interactor-Presenter-Entity-Router(VIPER), 
- Model-View-ViewModel(MVVM) etc.

The main goal of these patterns is *separation of concerns*. i.e they guide us on how to structure the different parts of our application. 
In return we build apps that are easy to reason about, safe to change and easy to test.

MVVM is a structural pattern splits our app to three main components based on their roles.
Model - Contains the business logic
View - User interface of the app
ViewModel - Entity that co-ordinates data flow and events between the Model and the View.

Prashanth : Image showing MVVM goes here. Maybe show only the boxes.. with labels for business rules etc.

MVVM pattern also defines rules of interaction between the components.
Boxes shows the components and lines shows the rules of engagement between them.
    As shown in the diagram the Model and view do not know of each other. They can change independent of each otheri

    Show that Onion diagram, with only view and domain layer.

Prashanth : Image showing MVVM goes here.
Static image above does not explani.. lets see it in action.


## OK.. fine.. So whats the benefit, Show me the benefit?

Separate presentation
    - Loosesly coupled : Can build and test UI separately
    - Single Resp Princliple
        -  Views : only change when there is achange to UI and user interaction
        -  Model : Only changes when some domain secific behavior changes.
        -  VM changes : When only some change in view affects the odel and some model affects views... other wise it need not change.


Dependencies :
    All Dependencies point inward.. <Prashanth> Show the onion diagram here again. with arrows.. 
    Clean Architecture : All lower level layers know nothing about higher level layers.
    "The goal, as always, is to have a codebase that is loosely coupled and high cohesive, so that changes are easy, fast and safe to make."


## MVVM Deep Dive : With an Example

<!-- Introduce the counter example -->
lets build a simple counter

1. Counter : that 
2. Add button that 
3. Minus button that decrements it. It has and additiona fucntion, disable when value is 0. No negative numbers.

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

Model Counter : Is an entity that encapsulates calculaation logic. i.e. the functions to add, subtract.
Takes in a number and returns the next number. (Pure function - that has no side effects)

Has a published property that publishes the latest value.
    - Passthrough Subject : It just publishes the latest value.

```swift

class ModelBoy {
 @Published somefellow: Int
}
```

Points to note : Knows nothing about the outside, just specializes in add and subtract. and publishes a value. Ensures it s loosely coupled and therefore easy to isolate and test.
So model matches our goal, easy to test and islolated .. etc.


### View

User interface of our app, everything that is presented to the User... note that is need not be a visible. thereore presentation layer but be better option.
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

### View-Model
Central component in MVVM Pattern
 - While the view and Model are fiercely independent, the view model is more affable, it acts as the interface between the view and model.

In its simplest from a View Model is a value converter, i.e it converts values to a value that can be consumed by the view.  

Represents the state and behaviour of the view  independently of the GUI controls used in the interface-  Martin Fowler

Its an alternate representation of the view and behaviour of the UI. - prashanth Moorthy :)
This enables you to bind the same view model to differnet views.. that helsp you swap out vies easily? For eg: SwiftUI vs UIKit etc.

<!-- Show mapping between properties in the view and view model. -->

Exposes a simplified unified view. for eg it can talk to more than one domain object... only those properties & behaviours at are relevant to the view in question.
Interprets user interactions :  View just sends it all the user interactions, this guy reads it and decides how it needs to be handled.

it is easier to consider Presentation Model as an abstract of the view that is not dependent on a specific GUI framework. While several views can utilize the same Presentation Model, each view should require only one Presentation Model. - Martin Fowler

Stores State : Only component in MVVM that has context / stores state.
Updates the model

Co-ordinates between view and view model

In its simplest form just a mapping. But will likely have logic for data transformation, persistence, transformation, can have all the logic

Owns the model and updates it.
Views Binds to the View <!-- Prashanth needs more elaboration -->



Summary: 

GUI Pattern Architecture : https://martinfowler.com/eaaDev/uiArchs.html#Model-view-presentermvp

References : GUI Pattern Architecture : https://martinfowler.com/eaaDev/uiArchs.html#Model-view-presentermvp



#### View 





#### Model



## SubTopic 1

## SubTopic 1
