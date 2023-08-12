---
layout: post
title:  "Swift Interview Questions - Array - Updated 2020"
date:   2020-08-16 21:16:00 +0530
categories: iOS Interview
permalink: /swift_interview_array_1
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

The Array is probably the most widely used data structure in programming. The array is a versatile data structure that used to solve a large number of problems.
It is easy to learn and work with an array however, most programmers don't use all of Array's features. 

It is also important to understand how Swift array works under the hood to determine if an array is the best data structure for a given task. Knowledge of how the different array methods and properties are implemented also helps avoid common pitfalls. 

Not surprisingly a lot of interview questions revolve around the array. 

This post includes some of the most frequent interview questions. 
<!--more-->

<br/>

<h3 style="color:#29c7ac;">Q. How can you safely fetch the first element a Swift Array?</h3>

>Swift Array provides the `first` property to safely fetch the first element of the array if it exists. 
>
>The `first` property returns an optional value. The return value is nil if the array is empty. 
>
>Avoid using the array[0], this will throw an exception and crash at runtime if the array is empty. 

```swift
let fruits = ["🍎","🥭","🍏","🥝","🍍"]
fruits.first  //🍎 

// Avoid this
fruits[0]    //🍎 

let emptyFruits = [String]()
emptyFruits[0]  // error❗️ 
```
<br />
<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />

<h3 style="color:#29c7ac;">Q. How can you safely fetch the last element of a Swift Array?</h3>
> Use the array `last` property. The last property returns an optional. The return value is `nil` if the array is empty. 

```swift
let fruits = ["🍎","🥭","🍏","🥝","🍍"]
fruits.last //🍍 
```

<br />
<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />

<h3 style="color:#29c7ac;">Q. How can you safely fetch the first n element of a Swift Array?</h3>

> Use Swift Array's `prefix(_:)` method to extract the first 'n' elements. 
>
> The prefix method safeguards against fetching elements that are beyond the elements in the array. 
> `prefix(_:)` returns all the elements of the array if n exceeds the number of elements in the array. 

```swift 
let fruits = ["🍎","🥭","🍏","🥝","🍍"]

fruits.prefix(3)
// ["🍎", "🥭", "🍏"]

// Avoid this. Potential for index out of bounds error. 
fruits[0...2] 
// ["🍎", "🥭", "🍏"] 

fruits.prefix(10)
//["🍎","🥭","🍏","🥝","🍍"]  

fruits[0...10]
// error❗️ - Array has only 4 valid indices.
```
Note: The prefix method returns an `ArraySlice` and not a new array. i.e it does not allocate any new storage.
An `ArraySlice` can be thought of as a view of the existing array. 

```swift
func prefix(_ maxLength: Int) -> ArraySlice<Element>
```

Another point to note is that maxLength has to be positive. Passing a negative value will result in an error. 

```swift
func prefix(_ maxLength: Int) -> ArraySlice<Element>
```

```swift
   // maxLength cannot be negative  
    arr.prefix(-1) // error❗️  
```

Array also has the `suffix(:_)` method to retrive the last n elements.
<br />
<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />

<h3 style="color:#29c7ac;">Q. How do you check if an Swift Array is empty?</h3>

> Swift Array has a property named `isEmpty` that can be used to check if the array has no elements. <br>
>
> The `isEmpty` property returns a boolean value which is `true` if the array is empty.

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
In other languages for eg: JavaScript, we check if the length of an array is 0 to determine if it is empty. 

For most commonly used collections such as Array, Set, Dictionary both isEmpty and count == 0 is O(1) time (constant). However, it is not true for all Collection types. 

For eg: The default view of a Swift string is a collection of characters. In the case of String, the time complexity of `count` is O(n). 
i.e the time taken to compute the count depends on the number of characters. To get the count we need to traverse the entire string.
Using count == 0 is an inefficient way to check if a `String` is empty.

To keep things consistent. It is advisable to use isEmpty for any type of collection.

<br />
<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />

<h3 style="color:#29c7ac;">Q. How can you reverse the contents of an array? </h3>

> The `reverse` method reverses the order of the elements in place. i.e The source array will be mutated. 
> Another method `reversed()` returns a view of the original array where the elements are in the reverse order. The reversed method does not allocate new storage.

```swift 
var sampleArr1 = ["🐶","🐱","🐭"]
sampleArr1.reverse()
//output :  ["🐭", "🐱", "🐶"]
```
Here the sampleArr1 itself is mutated. Furthermore, because the array is mutated, we have to declare it as a `var` 

```swift 
let sampleArr2 = ["🐶","🐱","🐭"]
sampleArr2.reversed()           
//output :  ReversedCollection<Array<String>>
```
The reversed method simply wraps the original array in a `ReversedCollection`. It does not reverse the array as yet.
The array we started with is unchanged. 
ReversedCollection is simply an entity that promises to behave as if the array is reversed. 

In the code below if we can confirm the order is reversed by looking at the first element.

```swift 
sampleArr2.reversed().first  
// 🐭
```

Performance :  
The `reverse()` method has a time complexity of O(n). i.e the time it takes depends directly on the number of elements in the array.
The `reversed()` method has a complexity of O(1). Since the reversed method does not move the elements of the array, it performs better. 

<br />
<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />


<h3 style="color:#29c7ac;">Q. How do you check if an array of strings contains a given string? </h3>

> Swift Array type defines a method called `contains` that can be used to check if an Array of strings contains a given string. 

```swift
let students = ["Alice", "Bob", "Chad"]

students.contains("Bob")    // true

students.contains("Dan")    // false 

students.contains("BOB")    // false  (compare is case-sensitive) 
```
<br>
#### How contains works 

![Contains Animation](/images/trial.gif)

<!-- 
Refer to this post for a detailed discussion on how the `contains` method works in Swift. 

In this post, we look at different ways in which elements in an array.
-->

<br />
<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br /> 
<h3 style="color:#29c7ac;">Q. How do you check if an array of custom elements contains a given element? </h3>

> Swift Array type defines the method called `contains(where : )` that can be used to check if the element that matches a predicate.

For eg: Consider an Array of Users. Where `User` is a type we have defined(struct) 

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
// Error : Referencing instance method 'contains' on 'Sequence' requires that 'User' conform to 'Equatable'

```

```swift
let userX = User(firstName: "Bob")

users.contains { (userX) -> Bool in
    return userX.firstName == "Bob"
}
```
<br />
<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />

<!--
IS it possible to create an Heterogesoud array in Swift?
// Swift docs say the following "An array stores values of the same type in an ordered list."
// Is it possible to create an array that contains only Int and Strings? For eg: [1,2,"fizz",4,"buzz"] ?
-->

