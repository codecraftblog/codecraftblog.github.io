---
layout: post
title:  "Data Flow in Swift UI"
date:   2021-07-04 00:16:00 +0530
categories: iOS Swift DateFormatter 
author:
  name: Prashanth 
  twitter: codecraftblog 
---

### @State Property Wrapper :

@State is used to manage *transient UI state* **locally** within a view. You do this by wrapping value types with the @State property.

@State Property Wrapper type that can read and write a value managed by SwiftUI.
 - SwiftUI manages the storage of any property declared as state.
 - When the state value changes, the view invalidates its appearance and recomputes the `body`.
 - Use State as the single source of truth for a given view.

- You should only access state properties from view's body or some method called from the view's body.
- So, declare @State properties as *private* to prevent clients of your view from accessing them.
- 
- To pass the state to another view, use the projected value `$propertyName` 

Prashanth this is GOLD.
>SwiftUI state and data flow property wrappers **always** project a `Binding`, which is a two-way connection to the wrapped value, allowing another view to access and mutate a single source of truth


What are property Wrappers ?

- projectedValue
- wrappedValue

### Creating a Game Tile

In this post we will at how to create as simple SwiftUI component, a Game Tile. We will create a tile that will be used in a memory game.
Here is a sample of what the tile will look like.

The tile will also have a two states.
The tile will toggle between a blank tile and show an ICOn when it tapped.

!["Game Tile"](/images/GameTile_1.png)

Behaviour : 

!["Game Tile"](/images/Tile_UserInteraction.png)

# Structure of the game tile. 
The Tile has a simple layout, it has two layers, 1. background layer (light Gray) and an icon.

Stacks : We use a Zstack to place objects one in front of the other.
Other types of stacks are HStack and VStack.

!["Game Tile"](/images/ZStack.png)


```swift
import SwiftUI

let lightGray = Color.init(white: 0.92)

 struct Tile: View {
        
    var body: some View {
    // 1. ZStack adds two layers.
    ZStack {                                                    // 1
        // Layer 1 : Background Layer                              
        RoundedRectangle(cornerRadius: 10.0)                    // 2
            .frame(width: 75, height: 75, alignment: .center)
            .foregroundColor(lightGray)
            
        // Layer 2 : Icon Layer     
        Text("🦁")
            .font(.system(size: 56))
        }
    }
}   

```
Output :
!["Game Tile"](/images/GameTile_1.png)

Code Explanation :

1. We create a ZStack to hold the other view we will create.
2. Create a Rounded rectangle.
3. Create an Text view, the contents of the view is an icon.

Note that the order in which we add elements to the Stack matters.

todo: segue to ZStack post.

### Making the Tile Interactive

In its default state the tile will not show an icon and when its is tapped it should reveal the icon.
When tapped again, it shold hide the icon.
We want the tile to show no icon, but when user taps on it the tile should reveal the icon.

todo: add image here .. showing user tap. .gesture.

When the user taps on the tile, we want to show or hide the icon.

Steps invovled :
    1. Detect taps on the tile.
    2. Tile detects the tap.
    3. Set the state to isTapped.
    4. Re-render the tile.


We need some way for the tile to store its current state. i.e the Tile should know if it has been tapped. 
we can do this by adding a varibale that tracks the status.

However, when the varible changes, we also want to show or hide the icon.

SwiftUI helps us track local state @SwiftUI @State property wrapper type???. 

The @State property wrapper as the name implies "wraps" a property. i.e. it add some additional functinanliy to our property.

In the case of the @State property wrapper, it enables swift UI will watch this property wrapper for changes and if any views are impacted by the change those views will be reloaded.

If any view uses the property wrapper it will be reloaded.


SwifUI provides a property wrapper type that .. doe sthis for us.

SwiftUI manages the storage.. 
When the value changes, view is invalidated and body is ...

Few rules :
Access state from only inside the view's body. or methods called by it.
To be safe mark it as private.



Lets add a @State variable.
The `isTapped` here is a property wrapper, that tracks if the card has been tapped.

```swift
struct Tile: View {

    @State private var isTapped : Bool = false  // 👈🏽

    var body: some View {
    // .....  
    }
}
```

State is a property wrapper, that tracks if the card has been tapped.

## Reacting to user tap.
Next the tile needs to keep track of whether it has been tapped.
We can do this with a boolean property do store the current state of the card.

On the ZStack, we add a tap gesture recognizer. and each time the ZStack is tapped, we toggle the isTapped property.

```swift
struct Tile: View {
    
    var isTapped : Bool = false
        
    var body: some View {
        ZStack {
            RoundedRectangle(cornerRadius: 10.0)
                .frame(width: 75, height: 75, alignment: .center)
                .foregroundColor(lightGray)
            
            Text(isTapped ? "🦁" : "")        // 1
                .font(.system(size: 56))
        }.onTapGesture {                      // 2
            isTapped.toggle()                 // 3 
        }
    }
}

```


## Update to UI based on state changes.

So far we are able to detect when a user taps on a tile.
We have also updated our variable that determines the current state of the card.

However there is one thing missing. We still need to update the UI to show and hide the icon. 

Introduction @State property wrapper.
SwiftUI provides the @State property wrapper that does this for us.
The @State property wrapper, wraps a property and then watches for changes to that property. wWhen the value changes, any views affected by that change is re-rendered. 

To make our isTapped boolen an @State property wrapper, we just need to prefix our property withe @State attribute ??.

```swift
    @State private var isTapped : Bool = false
```

SwiftUI does all the work behind the scene. When the value of isTapped changes, SwiftUI notices that our Text View uses this value. So SwiftUI then re-renders the body of the Tile View.

```swift
struct Tile: View {
    
    @State private var isTapped : Bool = false
        
    var body: some View {
        ZStack {
            RoundedRectangle(cornerRadius: 10.0)
                .frame(width: 75, height: 75, alignment: .center)
                .foregroundColor(lightGray)
            
            Text(isTapped ? "🦁" : "")        // 1
                .font(.system(size: 56))
        }.onTapGesture {                      // 2
            isTapped.toggle()                 // 3 
        }
    }
}

```

A few important things to notice :
- When you mark as a @State .. SwiftUI manages the storage.
- Do modify the value from outside the view. So mark it as as private.
- If you do need to pass to other view.. bidining .. $isTapped.


We have modified the text View to show the icon only if isTapped is true.

We have added a onTapGesture on the ZStack. So if you tap anywhere on the ZStack the isTapped property is toggled.

Under the hood SwiftUI recreates the view each time the isTapped changes. Thats the @State at work...

Final code looks like this.

```swift
import SwiftUI

let lightGray = Color.init(white: 0.92)

struct Tile: View {
    
    @State private var isTapped: Bool = false
        
    var body: some View {
        ZStack {
            RoundedRectangle(cornerRadius: 10.0)      (1)
                .frame(width: 75, height: 75, alignment: .center)
                .foregroundColor(lightGray)
            
            Text(isTapped ? "🦁" : "")
                .font(.system(size: 56))
        }.onTapGesture {
            isTapped.toggle()
        }
    }
}
```




To Summarize
 - Only card and 

todo: Add animation... showing tap and state change
