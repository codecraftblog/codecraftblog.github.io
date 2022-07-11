---
layout: post
title: "MVVM using Combine Swift"
date: 2022-06-20 22:45:00 +0530
categories: iOS Swift MVVM Architecture
author:
    name: Prashanth
    twitter: codecraftblog
---

Our code is a living entity. We constantly tinker with it. We add, move, delete and rearrange parts of our code in the hope of making it better.

The ease & confidence with which we can change our code, depends a great deal on how it is orgnized.
Architectural patterns are time tested solutions help us and anyone working our code understand it better.

Patterns do impose some restrictions on what we can and cannot do, but in return our code is easy to reason about, safe to change and easy to test.

> An architectural pattern is a general, reusable solution to a commonly occurring problem in software architecture within a given context. - Wikipedia

In this post, we look at a structural design pattern called _MVVM_ or _Model-View-ViewModel_.

We answer the following questions.

1. What is the MVVM?
2. What is the roles of Model View View-Models?
3. Example: MVVM in action.

To help understand the pattern better we build a simple iOS app using Swift and Apple's Combine framework.

<!--more-->

<br>

## MVVM Introduction

When it comes to building GUI based applications there are many popular patterns. A few examples are:

-   **MVC** - Model-View-Controller
-   **VIPER** - View-Interactor-Presenter-Entity-Router
-   **MVVM** - Model-View-ViewModel

This post looks at the MVVM pattern.

The Model-View-ViewModel (MVVM) is a structural design pattern. Structural design patterns guide us on how to organize different entities in our app and how different entities interact with each other.

The main goal of MVVM is _separation of concerns_. i.e it helps organize our code in a way that each part of our code deals with a coherent & well defined set of functions.

MVVM splits our application to three parts

-   **Model** - Contains the Data & business logic of our app.
-   **View** - User interface of the app
-   **ViewModel** - Entity that co-ordinates data flow and events between the Model and the View.

MVVM pattern also defines the rules of interactions between the three parts of the app. i.e. which component can interact with which other component.

<figure>
<img src="/images/mvvm_combine/MVVM_Image1.png" width="400">
  <figcaption>Fig 1. Components of MVVM pattern</figcaption>
</figure>

fig. 1 shows how the different parts are related to each other.
In fig. 1 the arrows indicate how the the different components talk to each other.

Boxes shows the components and lines shows the rules of engagement between them.
Direction of the arrow - Indicates the direction of the dependency. Direction of communication.
Type of arrow : - - -> Loosely coupled.
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

Separate presentation - Loosesly coupled : Can build and test UI separately - Single Resp Principle - Views : only change when there is achange to UI and user interaction - Model : Only changes when some domain secific behavior changes. - VM changes : When only some change in view affects the odel and some model affects views... other wise it need not change.

Dependencies :
All Dependencies point inward.. <Prashanth> Show the onion diagram here again. with arrows..
Clean Architecture : All lower level layers know nothing about higher level layers.
"The goal, as always, is to have a codebase that is loosely coupled and high cohesive, so that changes are easy, fast and safe to make."

## MVVM Deep Dive : Example

<!-- Introduce the counter example -->

Lets build a simple app that displays the price of our currency Dodgy Coin vs the US Dollar.

<figure>
<img src="/images/mvvm_combine/PriceCard.gif" width="400">
  <figcaption>Fig 3. Sample app MVVM pattern</figcaption>
</figure>

<!-- Prashanth give a gentle introduction to the UI without much technical details -->

<br>
<br>

### Model

Important : Does not store state or context. In our counter app, the model layer does not keep a tab of the current value of the counter.

The Model layer, deals encapsulates all the business logic.

Relation to other components in MVVM : Model layer not know about any of the other components.

However, the model still needs a way to notify anyone who is interested. i.e Needs a way to return the result of a calculation : Observable, ie other objects can watch for changes.

Tells other ugys.. hey ia m observable .. ie. you can watch me I dont care.. i will publish values. you can listen to if you want.
The Model layer contains all the domain or business rules.
All code that deals with things like data access, networking etc. would all be considered model.

In the counter app, the domain/business rules is code that increments or decrements the counter value.

```swift
import Foundation
import Combine

class PriceProvider: ObservableObject {

    let currencyName = "Dodgy Coin"          // 1️⃣
    @Published var price: Double = 394.05    // 2️⃣

    init() {
        startPublishingLatestPrice()
    }

    func fetchCurrentPrice() {              // 3️⃣
        // Fetches the price on demand.
    }

    private func startPublishingLatestPrice() {
        // Code that publishes the price at regular intervals.
    }
}

```

Important

1. Does not know anything about the counter app.
2. Does not store State or have any context

In MVVM model layer is completely unaware of the view or the view model.

However, the model still needs a way to notify anyone who is interested. i.e Needs a way to return the result of a calculation : Observable, ie other objects can watch for changes.

As show in fig1. the arrow communicates to the view model indirectly. Observable, pub sub.. that pattern.

Points to note : Knows nothing about the outside, just specializes in add and subtract. and publishes a value. Ensures it s loosely coupled and therefore easy to isolate and test.
So model matches our goal, easy to test and islolated .. etc.

<br>

### View

View - Its the user interface of the app. Usually a View is something the user can see and possibly interact with. 

In the MVVM pattern a different parts of the View binds to properties exposed by the ViewModel. 
i.e parts of the View bind or link themselves to specific properties on the ViewModel. When the ViewModel changes a properity the view that is bound to that property reloads to reflect the change.

Some View components also allow user interaction. When user action is detected (tap, swipe etc), the View informs the view model about interaction.

<figure>
<img src="/images/mvvm_combine/focus_view.png" width="600">
  <figcaption>Fig ?. View Model and its interactions</figcaption>
</figure>

Views should avoid having any logic in them. Avoid doing any type of type conversion etc in the View. Views rely on the ViewModel to provide the data they need via bindings.

In most apps, the views tend to change most often. Adopting the MVVM pattern allows us to develop and test views independently. The Views in our app can be easily be modified or even completely replaced without affecting the rest of the app. 

Lets say we decide to migrate our UIKit based app to SwiftUI, we can develop and test the SwiftUI view independently with a mock ViewModel and replace our UIKit view when our new view is ready.

#### Relationship View in MVVM

In the MVVM pattern, the View only interacts with the ViewModel.
A view only works with a single ViewModel, however the ViewModel can serve mulitple views.

<figure>
<img src="/images/mvvm_combine/interactions_view.png" width="600">
  <figcaption>Fig ?. View Interactions</figcaption>
</figure>

#### Relationship between View & Model in MVVM 

In the MVVM pattern, View and Model entites do not interact with each other at any point.
This means the View and Model are free to change without worrying about the other.

#### Relationship between View & ViewModel in MVVM 

As we discussed above, the View needs to bind to ViewModel. This enables to View to watch for cchanges it is interested in update itself.

The binding can be one-way or a two-way binding.
A one-way binding is where a View element only reads a value from the ViewModel. For eg: a Label.
In a two-way or bi-directional binding, the view can also updated the property its bound to in the ViewModel. For eg: A TextField that be both display a value and can be edited by the user. 

There are many different techniques to bind a view to a ViewModel.
Few popular ways are 
- Using Observable libraries such as Bond or writing a custom observable. 
- Key Value Observing (KVO), Notification 
- Using Reactive frameworks RxSwift, RxCocoa, Combine

In this post we use Apple's Combine framework for binding.

#### How to bind View to a ViewModel (using Combine)

Apple's Combine framework allows us to declare Publishers and Subscribers.
Publishers transmit a sequence of values over time and Subscribers register with a Publishers and get notifies each time a value changes.

In our MVVM, the ViewModel is a Publisher that transmits change in values and one or Views(Subsribers) can register with the ViewModel and get notified when there is change in value.  

<blockquote class="error">
Things to Avoid.
Avoid storing state in the view. In some cases people read the value from a label to fetch the prevous value etc. Avoid this..
</blockquote>

Note if you are familiar with MVC, the ViewController is also considered part of the View in MVVM.

#### View - Example PriceCard

Lets breakdown the View

<figure>
<img src="/images/mvvm_combine/PriceCardUIBreakDown.png" width="600">
  <figcaption>Fig 3. Sample app MVVM pattern</figcaption>
</figure>

1. We have a card title, that does not change.
2. One label that shows the price and changes everytime the card gets a new price.
3. A box at the that displays the change in price. It also shows a background box, that is either red or green or yellow
   depeding on the direction of price change.
4. Refresh button, that when tapped fetches the latest price.

```swift
struct PriceCard: View {
    @ObservedObject var cardViewModel: PriceCardViewModel                           //1️⃣
    
    var body: some View {
        ZStack {
            CardBackgroundView()
            
            HStack(alignment: .firstTextBaseline) {
                
                VStack {
                    CardTitleLabel(cardTitle: cardViewModel.cardTitle)               //2️⃣
                    RefreshButton(onClickAction: cardViewModel.refreshCardRequested) //3️⃣
                }
                
                Spacer()
                
                VStack {
                    Text(cardViewModel.currentPriceString).heavyFont()                //4️⃣
                    DiffView(direction: cardViewModel.priceDirection,
                             value: cardViewModel.priceDifferenceString)
                }
            }
            .paddedBox()
        }
    }
}
```


<br>
<br>

### View-Model

The ViewModel does most of the heavy lifting in the MVVM pattern. Unlike the Model and View entities that/which are fiercely independent, the ViewModel is more affable; it interacts with both the view and the model entities.

<figure>
<img src="/images/mvvm_combine/focus_viewmodel.png" width="600">
  <figcaption>Fig ?. View Model and its interactions</figcaption>
</figure>

The ViewModel is an alternate representation of the state and behavior of the View.
In other words, by inpsecting the ViewModel we should be able to validate the data and behaviour independent of the View.
Which also means that we can test the correctness of the data just by inspecting at the ViewModel.

> _Represents the state and behavior of the view independently of the GUI controls used in the interface_ - **Martin Fowler**

#### View Model acts as a value converter

In its simplest form, a ViewModel is a value converter. It converts data provided by **one or more** model entities to a value that can be consumed by the view.
In our price card example, the Model `PriceCardPublisher` publishes the price of type `Double`, and the ViewModel converts the `Double` to `String` type which is displayed by the View.

<blockquote class="warning">
<h6>⚠️  Note </h6>
The view should not handle type conversion etc. The View model is the right place.
</blockquote>

In real world scenarios, in addition to converting values, a ViewModel also triggers request for data, stores state, handles user interaction and updates the model.

#### View Model stores the state/context

Any information that we want to keep around is stored in the ViewModel. In the MVVM pattern the ViewModel is the component that has context/stores state.

In our example, the Model `PriceModel` publishes the latest price, and it's done. It does not keep track of the last published price or historical prices.
However, our app wants to show the difference and direction of price movement. We use the ViewModel to store the last displayed price and when we get a new updated price we do a comparison in the ViewModel.

#### View Model handles user interaction

The ViewModel also handles user-initiated action. When the user interacts with View, the View intercepts user interaction and communicates it to the ViewModel. The ViewModel then decides how that action should be interpreted.

In our example, when the user taps the refresh button, the view merely communicates to the ViewModel that the refresh button was tapped. i.e it calls the `refreshCard()` method.
The view model then decides if this request should be forwarded to the model and then further sets the view state when the data is being fetched.

<!-- Prashanth Show mapping between properties in the view and view model. -->

<figure>
<img src="/images/mvvm_combine/interactions_viewmodel.png" width="600">
  <figcaption>Fig 3. Sample app MVVM pattern</figcaption>
</figure>

#### Interation between ViewModel and Model

-   ViewModel can work with one or more model entities..
-   ViewModel holds on to a referece to the model entities. Conversely, Model/s are unware of the ViewModels.
-   ViewModel listens for changes in the model entities and reacts to any changes.

#### Interaction between ViewModel and View

-   View binds to ViewModel. Does not hold a reference. Does not in anyway refer the view. This enables you to bind the same view model to different views.. which helps you swap out views easily? For eg: SwiftUI vs UIKit etc.
-   ViewModel receives events from the view and then reacts.
-   ViewModel One view should have only one viewModel
-   One ViewModel can work with multiple views

Back to our code example :

Code Snippet 3.0

```swift
import Foundation
import Combine

class PriceCardViewModel: ObservableObject {

    private let priceModel = PriceProvider()                            // 1️⃣

    // Properties that view entites can read or bind to.                // 2️⃣
    var cardTitle: String
    @Published var currentPriceString : String = "Start Value"
    @Published var priceDifferenceString : String = ""
    @Published var priceDirection: PriceDirection = .unchanged

    private var currentPrice: Double = 0.0                              // 3️⃣
    private var store = Set<AnyCancellable>()

    init() {
        cardTitle = priceModel.currencyName
        bindToModel()                                                   // 4️⃣
    }

    private func refeshCard() {                                         // 5️⃣
        priceModel.fetchCurrentPrice()
    }
}

```

In the above code snippet

-   1️⃣ - ViewModel creates a Model instance and holds on to it.

-   2️⃣ - The View Model has four properties that can be read by the view.
    cardTitle : Does not change. Views direclty reference this.
    Published properties : View Modesl can bind to this value and watches for changes .

-   3️⃣ - `currentPrice` is where the VM stores the state. This is the only place where the state is sotred. state currentPrice : Holds on to state. This is what is used to compare prices when a new price is received.

-   4️⃣ - Here the ViewModel binds itself to the Model. i.e it subscribes to change in the Model.

-   5️⃣ - `refreshCard()` is a public method that views can call when they want the model to fetch a new value.
    Note that when a view calls the refreshCard method, there is no guarantee that the view model will refresh the card. For eg: there could be minimum duration the ViewModel watits before it preas reqeust.

<blockquote class="tip">
Note: The PriceCardViewModel has no reference to any View objects. 
</blockquote>

{::options parse_block_html="true" /}

<details>
<summary markdown="span">PriceCardViewModel Extension : Contains helper functions</summary>

```swift
extension PriceCardViewModel {

    func bindToModel() {
        priceModel.$price.sink { latestPrice in
            self.priceDirection = self.calcPriceDirection(lastPrice: self.currentPrice, latestPrice: latestPrice)
            self.priceDifferenceString = self.calcPercentageDiff(priceDifference: self.currentPrice - latestPrice)
            self.currentPrice = latestPrice
            self.currentPriceString = String(format: "$%.2f", self.currentPrice)
        }.store(in: &store)
    }

    func calcPriceDirection(lastPrice: Double, latestPrice: Double) -> PriceDirection {
        let priceDifference = latestPrice - self.currentPrice

        if priceDifference == 0 {
            return .unchanged
        }

        let priceDirection = priceDifference > 0 ? PriceDirection.positive : PriceDirection.negative
        return priceDirection
    }

    func calcPercentageDiff(priceDifference: Double) -> String {
        let formatter = NumberFormatter()
        formatter.numberStyle = .currency
        formatter.maximumFractionDigits = 2

        return formatter.string(for: priceDifference) ?? ""
    }
}

```

</details>

{::options parse_block_html="false" /}

<br>
<br>

### Summary:

GUI Pattern Architecture: https://martinfowler.com/eaaDev/uiArchs.html#Model-view-presentermvp

References: GUI Pattern Architecture: https://martinfowler.com/eaaDev/uiArchs.html#Model-view-presentermvp
