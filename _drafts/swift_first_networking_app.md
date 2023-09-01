---
layout: post
title: "Swift Networking : Build first"
date: 2022-06-20 22:45:00 +0530
categories: iOS Swift MVVM Architecture
author:
    name: Prashanth
    twitter: codecraftblog
---

Intro the app: What are we trying to build.
Show the UI

Title suggestions for a series of blog posts on building an iOS app from scratch with MVVM, SwiftUI, and networking:

1. "Building Your First iOS App: Getting Started with Swift and MVVM"
2. "Creating a SwiftUI User Interface for Your iOS App: A Step-by-Step Guide"
3. "Building the Model Layer: Managing Data and Networking in Your iOS App"
4. "Developing the ViewModel Layer: Connecting the Model and View in MVVM"
5. "Implementing Data Models and Bindings: A Deep Dive into the MVVM Pattern"
6. "Handling User Input and Navigation in SwiftUI: Building Interactivity into Your App"
7. "Testing and Debugging Your iOS App: Best Practices and Troubleshooting Tips"

The suggested series of blog posts will begin with the basic steps of iOS app development, gradually introducing the MVVM architecture and SwiftUI for building the user interface. It will then focus on key aspects such as building the Model layer for managing data and networking, followed by the ViewModel layer to serve as the mediator between the Model and View layers. Finally, the series will cover topics like data models and bindings, handling user input, and testing and debugging techniques to ensure a comprehensive understanding of building an iOS app from scratch.


Part 1: Structure the app.
Part 2: Building the Model 
Part 3: Building the ViewModel
Part 4: Building the View 
Part 5 : Bringing it all together 

Our code is a living entity. We constantly tinker with it. We add, move, delete and rearrange parts of our code in the hope of making it better.

Build your first app

Topics you will learn
- App Structure - 
- Networking
- Building your UI

### Structure of the App
Why do we need a strucuture.
Need many things. 
User Interface, Networking, Business Logic and intereactions between all these things.
Which component makes a call ? Where should the neworking code go? Should it be in the UI?

This is where design patterns come in. Seperation of concerns & communication challnesl.. Which components can talk to each other. Eye on maintainability etc.
Rules of communiation using MVVM.

Why do we need a strucuture.

We use the MVVM pattern. To understand more about MVVM check out this post.
We bascially split the app into three may parts
- Model - API and the Data Model 
    - Petstore API
- View - Views
- View Model - View Model


The MVVM (Model-View-ViewModel) pattern is widely used in iOS app development to ensure a clear separation of concerns and to promote maintainability and testability. In the MVVM pattern, the model represents the data and business logic, the view displays the user interface, and the view model acts as a bridge between the model and the view. The view model exposes properties and commands that the view can bind to, facilitating data updates and user interactions. This pattern allows for a decoupled architecture, where the logic and data can be easily tested and modified without affecting the user interface. Additionally, it enables code reusability and provides a clean structure to organize and manage complex application logic.

## How to build the Model layer in a iOS app

## How to build the ViewModel layer in a iOS app
- Assume some UI element will need data.. this guy is the coordinator.
- We should be able to test this ViewModel layer indpenedently. i.e without the need for a UI.

## How to build the View layer in a iOS app
- Assume some UI element will need data.. this guy is the coordinator.
- maybe swiftUI, maybe UIKit or Mac OS app. 
- It should not matter to the ViewModel or View  

## New Post : 
### Building the Model layer

Typically an app will work with a backend. and the backend will to some extent decide what entieis ad their shpae in our app. 
Our app will have the following entities.
Add a link to the Swagger PetStoreAPI here.
- Pet
- Tag
- Status

Our model layer has two main repsonsilbity. Making a network calls can creating model objects that our App understands.

At the end, i.e when are done building the model layer, we should be able to make network rqequts and fetch data and convert the Data into entities that our app understands.

<!-- Chat GPT -->

In the MVVM design pattern, the networking code is typically implemented within the model layer. The model layer is responsible for handling data and business logic. This includes tasks such as retrieving data from remote APIs, parsing and processing the received data, and notifying the view model or view about the updated data. By placing the networking code within the model layer, it ensures that the view model remains focused on managing the presentation logic and UI state rather than dealing with the intricacies of networking. This separation of concerns allows for better code maintainability, testability, and modularity.



### View Model
Pet Listing View Model

### Netowrking Layer 

### Building the View

### 

