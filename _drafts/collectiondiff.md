---
layout: post
title:  "Draft - Working with CollectionDiff"
date:   2020-06-28 21:16:00 +0530
categories: iOS 
author:
  name: Prashanth 
  twitter: johndoetwitter
  picture: /images/profilePhoto.jpg
---

How to diff collections in Swift 
diffing in swift
Working with Collection Difference
How to implement a rudiementary spell checker in swift

Xcode 11.0+
Swift Standard Library

In Xcode 11, the Swift Standard Library added the ability to find the difference between two collections. Given two collections the `difference` method returns the set of operations required to arrive at a target array from a source. 

The logic to find the diff between two arrays is not trivial. This wikipedia article https://en.wikipedia.org/wiki/Diff for more information. 
The addition of `difference` eliminates the need to wrangle with complex and elrror prone. 

Lets look at how the difference method work by looking at a simple example. 
In the code below we want to find the diff between `sourceArr` to `targetArr`

```swift
let sourceArr = ["One","Two","Three"]

let targetArr = ["Two", "Three", "One", "Four"]

```
Here the sourceArray has two changes 
1. The order of the elements in the original array has changed. "One" has moved from index 0 to index 3. 
2. A new element "Four" has been added to the end of the Array.

```swift
let diffResult = targetArr.difference(from : sourceArr) 
print(diffResult)
```
Calling `difference` method on the targetArr returns a CollectionDifference.

```swift
CollectionDifference<String>(
            insertions: [
                    Swift.CollectionDifference<Swift.String>.Change.insert(offset: 2, element: "One", associatedWith: nil),
                    Swift.CollectionDifference<Swift.String>.Change.insert(offset: 3, element: "Four", associatedWith: nil)
            ],
            removals: [
                    Swift.CollectionDifference<Swift.String>.Change.remove(offset: 0, element: "One", associatedWith: nil)
            ]
)
```
A `CollectionDifference` is a struct that lists the following :
* removals : Array of changes that reprsent removals from the sourceArr 
* insertions : Array of changes that represent insertions into the sourceArr.


### Note on element move operations 
are combination of removal and insertion operations.
For example : The element "One" moved from index 0 to index 2.
The Collection difference treats this a removal from index 0 and insertion at index 2.
From looking at the result of the diff, we dont see any indication of moves. We can however use the `inferringMoves` method, which will then indicate wich element was effectively a move.



  ..For 
 The outpt of difference is a CollectionDifference.
 A colleciton differece is a consists of insertions, and deletions required to dervie the target array from the input array.
 
 *A collection of insertions and removals that describe the difference between two ordered collection states.*

One use case is if we have really large array we can choose to remove and insert only lements that have c anged.
Thinsk of a scenairo where get a some sort of dat aform an extgernal api. The data may or maynot have changed. Using diff. we can choose to apply a change only if ther eis a change and also, and also only apply changes at the desired indices.

Now, for scenarios where migth use this could be UI udpates. For ex you cwna to animait the uin chages. 
i.e show how the changes are applied.. with soem ainimation.We dont really need to udnestand the innnards of CollectionDifference.

Who? Swift elements do provide another API called `applying` that take an array and a collecciton diff and apply it to the array.

So lets look at the applying method.

```swift
Apply the changes
func applying(_ difference: CollectionDifference<Element>) -> Array<Element>?
```
Notice that this return type is optional. If you pass in an incompatible colled diff. they it will return nil.

-------

## Practical applications

Updating tableviews. Check out diffable datasource.
Updating your custom UI.

### Implement a simple spell check :
As we discussed above, the diff works on any colleciton that conforms to the BidirectionalCollection protocol.
Swift `String` conforms to BidirectionalCollection and that makes it easy to implement a rudiementary spell checker using diff.

```swift
let inputString = "Karnatak"

let stateNames = ["Karnataka", "Kerala", "Tamil Nadu"]

let possibleMatches = stateNames.filter {
    if stateNames.contains(inputString) {
        return true
    }

    let diff = $0.difference(from: inputString)
    return diff.count < 3
}

print("Possbile Matches : " \(possibleMatches))

// Output 
Possbile Matches : ["Karnataka"]
```

Here is an example of a search UI thats fault tolerant. It definitley better 

Diff between strings? Levenstien.. 

Beofre we look at what a colleciotn diff is .. we can 

Practical applications

Updating tableviews. Check out diffable datasource.
Updating your custom UI.
![yay](/images/tableview_animation.gif)


The `difference` method returns a collectionDifference.
A collecitonDiff is an other collection that lists the elemtns that needs to removed and insterd inorder to go from the sourceArr to the targetArr.

In this case the returned value is 

Prashanth Add the outptu of diff here..

for most applications we would not want to do deal with this fellow. we are just going to consume this.

### Applying a diff

Collections also prrovide a wya to apply a diff. Here we just call

```swift
array.apply(diff)
```
----------

### Practical uses :

Simple Spell check


There are multiple ways we can do this. One approach is to delete the  
Step 1 : Remove element at index 0 "One"
Step 2 : Insert element at index 2 "Four"

Like any other collection the CollecitonDiff can be interated.
 The outpt of difference is a CollectionDifference.
 A colleciton differece is a consists of insertions, and deletions required to dervie the target array from the input array.
 
*A collection of insertions and removals that describe the difference between two ordered collection states.*

```swift

```

Prashanth : Why would you want to convert arrays.


for complex errorDiff (short for Difference) In simple terms a diff tells you what oerations you need to do to convert one collectio into aothe. 

Collections this can be descried with two operatios removals and insertsions. 
Moves just removal from one index and insert at another index.

-------------

Examine the difference method in more detail.

```swift
func difference<C>(from other: C) -> CollectionDifference<Element> where C : BidirectionalCollection, Self.Element == C.Element
```

We already saw.. 
  1. Works with collection C, where C should conform to a `BidirectionalCollection`
  2. Elemnts in C should be fo the same type. In this case.. both were arrays of strings. 








Collection Difference provides an efficient and easy to use api to convert collections 

For a collection the diff is the A diff is a lay mans terms the 

```swift
let sourceArr = ["One","Two","Three"]

let targetArr = ["Two", "Three", "Four"]

```


Collection Difference provides an efficient and easy to use api to convert collections 

For a collection the diff is the A diff is a lay mans terms the 

## Defintion of difference
Works on any BidirectionalCollection. And the elements in the two collections should be of the same type.

```swift
func difference<C>(from other: C) -> CollectionDifference<Element> where C : BidirectionalCollection, Self.Element == C.Element
```


