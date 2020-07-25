---
layout: post
title:  "Draft - Swift interview questions (Array)"
date:   2020-06-28 21:16:00 +0530
categories: iOS 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

Array is probably the most widely used data structure. Every programming language has a data structure has some form of  
Not surprisingly a lot of questions in inteviews revolve around arrays. 

This post includes some frequenlty asked questions about Swift Arrays and also explains why things work they way they do. 

<br/>

<h3 style="color:#29c7ac;">Q. How can you safely fetch the first element of a Swift Array?</h3>

> Use the array `first` property. The first property returns an optonal value. The return value is nil if the array is empty. 
> A common mistake is to use array[0], this will throw an error and crash at runtime if the array is empty 

```swift
let fruits = ["🍎","🥭","🍏","🥝","🍍"]
fruits.first  //🍎 

// Avoid this
fruits[0]    //🍎 

let emptyFruits = [String]()
emptyFruits[0]  // error❗️ 
```
<br/><br/>

<h3 style="color:#29c7ac;">Q. How can you safely fetch the last element of a Swift Array?</h3>
> Use the array `last` property. The last property returns an optional. The return value is nil if the array is empty. 

```swift
let fruits = ["🍎","🥭","🍏","🥝","🍍"]
fruits.last //🍍 
```

<br/><br/>

<h3 style="color:#29c7ac;">Q. How can you safely fetch the first n element of a Swift Array?</h3>

```swift 
let fruits = ["🍎","🥭","🍏","🥝","🍍"]

fruits.prefix(3)
// ["🍎", "🥭", "🍏"]

// Avoid this. Potenial for index out of bounds error. 
fruits[0...2] 
// ["🍎", "🥭", "🍏"] 

fruits.prefix(10)
//["🍎","🥭","🍏","🥝","🍍"]  

fruits[0...10]
// error❗️ - Array has only 4 valid indices.

```
<p class="center">───── ⋆⋅☆⋅⋆ ─────</p>

<h3 style="color:#29c7ac;">Q. How do you check if an Swift Array is empty?</h3>

> Swift Array has an instance property named `isEmpty` which can be used to check if array has no elements. <br>
> The `isEmpty` property returns a boolean value which is `true` if the arrray is empty.

```swift
let emptyArr = [String]()

emptyArr.isEmpty        // true 
emptyArr.count == 0     // true 

let nonEmptyArr = ["foo","bar"]

nonEmptyArr.isEmpty     // false
```

The `isEmpty` property is not limited to Array. It is available on all types that conform to the Collection protocol 
Some commonly used types that conform to the Collection protocol are Array, Set, Dictionary, String (conforms BidirectionalCollection)  

#### Performance : 
The `isEmpty` property has a complexity of O(1) for collections. 

#### Using isEmpty vs count == 0
In other languages for eg: JavaScript, we check the if the lenght of a array is 0 to determine if its empty. 

For most most commonly used collections such as Array, Set, Dictionary both isEmpty an count == 0 is constant time. However, its not true for all Collection types. 

For eg: The default view of a Swift string is a collection of characters. In the case of String the complextion of `count` is O(n). 
i.e the time taken to compute the count depends on the number of characters. To get the count we need to traverse the entire string.
Using count == 0 is inefficient way to check if a `String` is empty.

To keep things consistent. Its advisable to use isEmpty for any type of collection.

<br/><br/>

<h3 style="color:#29c7ac;">Q. How can you reverse the contents of an array? </h3>

> The `reverse` method reverses the order of the elements in place. i.e The source array will be mutated. 
> Another method `reversed()` return a view of the original array where the elements are in the reverse order. The reversed method does not allocate new space.

```swift 
var sampleArr1 = ["🐶","🐱","🐭"]
sampleArr1.reverse()
//output :  ["🐭", "🐱", "🐶"]
```
Here the sampleArr1 itself is mutated. Further more, because the array is mutated, we have to declare it as a `var` 

```swift 
let sampleArr2 = ["🐶","🐱","🐭"]
sampleArr2.reversed()           
//output :  ReversedCollection<Array<String>>
```
The reversed method simply wraps the original array in a `ReversedCollection`. It not revers the array as yet.
The array we started with is unchanged. 
ReversedCollection is simply an entity that promises to behave as if the array is reversed. 

In th code below if we can confirm the order is reversed by looking at the first element.
```swift 
sampleArr2.reversed().first  
// 🐭
```

Performance :  
The `reverse()` method has a complexity of O(n). i.e the time it takes depends directly on the number of elements in the array.
The `reversed()` method has a complexity of O(1). Since the reversed method does not move the elements of the array, it performs constant. 

Here is an exmaple of how the reverse method can be useful. 
Which searching large arrays if we have an inlking of which half of the array an element might lie, we can iterate over the array in revers order. 
Here we are looking for the '🍏', note how using the reversed method and then iterating over the array speeds up the look up time. 

```swift 
let largeArr = Array(repeating: "🍎", count: 999999) + ["🍏"]
// ["🍎", "🍎", "🍎", .......,"🍎", "🍎", "🍎", "🍏"]

largeArr.contains("🍏")
// Execution time :  0.38 seconds

largeArr.reversed().contains("🍏")
// Execution time : 0.00008 seconds
```


popular uses 
- Like they showed in the wwdc videw.. when removing elements from an array if you traverse in revers order . its cool.
- Palindrome?

value is nil if the array is empty.

Same things for last.

Note : we might be tempted to fetch the index at element zero. but this is not always guaranteed to gire th correc tresut.
For eg: You will get an execpton if the array is empty.

Complexity : 

<h3 style="color:#29c7ac;">Q. How can If a string isEmpty?</h3>
use isEmpty.. do not use count Property.. 
For eg : if it does not conform to the Randomaccess colleciton.. it will iterate over the collection.

Strings for eg: Do not conform to the RandomAccessColleciton protocol.

why?

// Default implementations of core requirements
extension Collection {
  // A Boolean value indicating whether the collection is empty.
  ///
  /// When you need to check whether your collection is empty, use the
  /// `isEmpty` property instead of checking that the `count` property is
  /// equal to zero. For collections that don't conform to
  /// `RandomAccessCollection`, accessing the `count` property iterates
  /// through the elements of the collection.
  ///
  ///     let horseName = "Silver"
  ///     if horseName.isEmpty {
  ///         print("My horse has no name.")
  ///     } else {
  ///         print("Hi ho, \(horseName)!")
  ///     }
  ///     // Prints "Hi ho, Silver!")
  ///
  /// - Complexity: O(1)
  @inlinable
  public var isEmpty: Bool {
    return startIndex == endIndex
  }

  If its a random access collection.. you have access to the startIndex and endIndex
  for otheres to find the end index.. you neeed to iterate and find it. 


<h3 style="color:#29c7ac;"><span style="font-size:32px">Q.</span> How can you safely fetch the first element of a Swift Array?</h3>

Given a very large arra, we an fetch it in constant time.

Complexity: O(1)

Array conforms to random access cllection. : menaing..
BidirectionalCollection only have `last` function 

// Swift docs say the following "An array stores values of the same type in an ordered list."
// Is it possbile to create an array that contains only Int and Strings? For eg: [1,2,"fizz",4,"buzz"] ?
<h3 style="color:#29c7ac;"><span style="font-size:32px">Q.</span> How can you safely fetch the first element of a Swift Array?</h3>


<h3 style="color:#29c7ac;"><span style="font-size:32px">Q.</span> How can you get the first index of a swfit commection ?</h3>

> Answer :<br>
> Noote the word collection. Not that there are collectioons does not imply a Array.  Arrays are inexed by INt so syou can do [0]
> Only random access collections can be indexed by Int
> Swift Array type defines the a method called `contains` that can be used to check if an Array of strings contains a given string. 


<h3 style="color:#29c7ac;"><span style="font-size:32px">Q.</span> How do you get the first character of a string?</h3>

> Answer :<br>
String is a collection but you can do .. get [0]

<h3 style="color:#29c7ac;"><span style="font-size:32px">Q.</span> How do you check if an array of strings contains a given string?*</h3>

> Answer :<br>
> Swift Array type defines the a method called `contains` that can be used to check if an Array of strings contains a given string. 

```swift
let students = ["Alice", "Bob", "Chad"]

students.contains("Bob")
// true

students.contains("Dan")
// false 

students.contains("BOB")
// false 
```

#### Closer look 

![Drag Racing](/images/trial.gif)

Refer to this post for a detailed discussion on how the `contains` method works in Swift. 

In this post we look at at different wasy in which elemnts in an array.

#### Deep Dive 

---
<br>

<h3 style="color:#29c7ac;">How do you check if an array of custom elements contains a given element?</h3>

A. Swift Array type defines the a method called `contains(where : )` that can be used to check if the element that matches a predicate.
For eg : Consider an Array of Users. Where `User` is a type we have defined(struct) 

```swift
struct User {
    var firstName : String
}

let userA = User(firstName: "Alice")
let userB = User(firstName: "Bob")
let userC = User(firstName: "Chad")

let users = [userA,userB,userC]

```

```swift
let userX = User(firstName: "Bob")
//users.contains(userX)
// Referencing instance method 'contains' on 'Sequence' requires that 'User' conform to 'Equatable'

```

```swift
let userX = User(firstName: "Bob")

users.contains { (userX) -> Bool in
    return userX.firstName == "Bob"
}

```

#### Under the hood :

Method defintion
Time complexity


| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

