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
*Work in progress* : Need to figure out an appropriate title and subtitle.
How to diff collections in Swift 
diffing in swift
Working with Collection Difference
How to implement a rudiementary spell checker in swift

In Xcode 11, the Swift Standard Library added the ability to find the difference between two collections. The `difference(from:)` method returns the set of operations required to arrive at a target collection from a source collection. 

The logic to find the diff between two collections is not trivial. This wikipedia [post][wikipedia-diff] for more information. 
The addition of the `difference(from:)` method to the Swift Standard Library eliminates the need to wrangle with custom code that can quickly become complex and error prone. 

In this post we look at how to use the `difference(from:)` method and some use cases.

Lets start with a simple example. We will use a popular collection the Array.

In the code below we want to find the diff between two arrays `sourceArr` and `targetArr`

```swift
let sourceArr = ["One","Two","Three"]

let targetArr = ["Two", "Three", "One", "Four"]
```

In this example, its easy work out that there are two changes.
1. The order of the elements in the original array has changed. i.e element "One" has moved from index 0 to index 2. 
2. A new element "Four" has been added at index 4. 

Prashanth --- Add that image here.. animation of the changes required.

This task of identifying the changes however can quicky get challenging as soon as the number of changes increases. 

In the code below, we apply the diff method to define operations for us. 

```swift
let diffResult = targetArr.difference(from : sourceArr) 

print(diffResult)
```
Calling `difference(from:)` method on the targetArr returns a struct called a `CollectionDifference`

> A collection of insertions and removals that describe the difference between two ordered collection states. - Apple Developer Docs

At this point, we will skip over the innnards of the `CollectionDifference` and see how we can apply the diff. 
We will come back and look at the collectiondiff in the section "Prashanth add link to section here" 

## Applying the `CollectionDifference`

Collections have a handy method called `applying(_:)` that can by used to apply a diff to a collection.
The `applying(_:)` method takes the source collection and a diff and optionally returns a new collection. If the diff and the source Array are incompatible the `applying(_:)` method returns nil.

```
let finalArr = startArr.applying(diff)
```

Note on colletions : 
There are few important considerations we glossed over when we looked at. We used the term colleciont a lot.
TO be precise the difference: method only works on collectiosn that conform to BidirectionalCollection

Common types that conform for BidirectionalCollection are : Array, String etc.

For a full list of look here : Prashanth Link to apple site here...

```
func difference<C>(from other: C) -> CollectionDifference<Element> where C : BidirectionalCollection, Self.Element == C.Element
```
Works with any Collection (artibrarily called C here). The Collection should satisfy two conditions.
1. C Must conform to the BidirectionalCollection progocolji
2. The source array and trager array should have same type.
    i.e you can find a diff between and array of strings and an array of Ints




Differnce from : 


https://hacker-news.firebaseio.com/v0/updates.json?print=pretty


#j Practical Applicaitons

What are some of the practial applications of this? 
1) Updating large arrays. For eg; IF you data comes in from a removoe if your data comes from some external datasource. 
Diff can be used to efficiently updated your data store.






The apply function performs much better than iteration over the collection diff


If we examine the output of the difference we can see that it has two arrays
1. An array with 1 removal operation. Remove element "One" at index 0.
2. An array with 2 insertion operation. Insert element "One" at index 2 and insert element "Four" at index 3.

```swift
// Output
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
### Processing the CollectionDifference

What we have so far is the set of operations. In order to convert the sourcearr to the targetArr, we have to apply the diff.

The collectionDifference returned is also a collection, which means we can interate over it. (like a array.) 
For eg: we can start with the sourceArr and then iterate over the diff and perform one ooperataio aat a time. 

```swift
var tempArr = []

diff.forEach { (change) in
    switch change {
    case .insert(offset: let offset, element: let element, associatedWith: _):
        tempArr.insert(element, at: offset)
        print("Insert at offset : \(offset)  element \(element)  stateAfter : \(tempArr)")
    case .remove(offset: let offset, element: let element, associatedWith: _):
        tempArr.remove(at: offset)
        print("Remove at offset : \(offset)  element \(element) stateAfter : \(tempArr)")
    }
}

/* Output
Remove at offset : 0  element One stateAfter : ["Two", "Three"]
Insert at offset : 2  element One  stateAfter : ["Two", "Three", "One"]
Insert at offset : 3  element Four  stateAfter : ["Two", "Three", "One", "Four"]
*/
```

Collections also have another handy method called applying(_:) that can by used to apply a diff to a collection.
The apply function performs much better than iteration over the collection diff

```
let finalArr = startArr.applying(diff)
```
The iteration is useful when we want to apply the echanges in stages. For eg: If you are using he perofrmn batchUpdates in a table view. HThen we can use 

Two step process

## Practical Applicaitons
What are some of the practial applications of this? 
1) Updating large arrays. For eg; IF you data comes in from a removoe if your data comes from some external datasource. 
Diff can be used to efficiently updated your data store.

2) Works great with UI updates. When you want to animate the hcanges.
Think table bivew. althought with the introduction of the Diffable dataroouse.. 


The method we choose will depend on the  

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
 
> A collection of insertions and removals that describe the difference between two ordered collection states.

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
<img align="right" src="/images/tableview_animation.gif">

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


### talk bout the toher variant 
```swift
difference(from:by:)
```

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


[wikipedia-diff]: https://en.wikipedia.org/wiki/Diff
