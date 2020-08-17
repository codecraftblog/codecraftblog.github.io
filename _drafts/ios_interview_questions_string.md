---
layout: post
title:  "Draft - iOS interview questions (Strings)"
date:   2020-06-28 21:16:00 +0530
categories: iOS 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

<br/>

<h3 style="color:#29c7ac;">Q. How do you create an empty string in Swift? </h3>

> Empty strings can be created using the String's initalizer or by a string literal. 
> String literals is the preferred way.

```swift
let emptyStr1 = String()     // Using String intializer
let emptyStr2 = ""           // Using string literal

```
Both the methods described above yield the same result. 

```swift
emptyStr1 == emptyStr2      // true
```

<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />

<h3 style="color:#29c7ac;">Q. State a few different ways you can create a string in Swift? </h3>

Here are a few common patterns to create a String . Swift Standard Library has a rich set of methods to create strings
the most popular method is using string literal.

```swift
//: How do you create a string in Swift?
let str1 = "foo"
let str2 = "This is a really long string"

 // From Sequence of Characters.
let str3 = String(["F","O","O"])

var emojiStr = "Hello 😈"

let multiLineStr =
"""
Line 1 : The first line
Line 2 : The Second line
Line 3 : The Third line
"""

```

<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />


<h3 style="color:#29c7ac;">Q. How do you find the length of a Swift String ? </h3>

> By "length" here refers the the number of characters in the String. We can get the number of characters using the `count` property 

```swift
var str = "Hello 😈"
str.count // 7 
```

<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />



<h3 style="color:#29c7ac;">Q. How can we check if a string is empty in Swift? </h3>

> Use the `isEmpty` method to check if a string is empty. 

```swift
var str = "Hello 😈"
str.isEmtpy  // false

var emptyStr  = ""
emptyStr.isEmtpy  // true 

```

Note : Avoid using `string.count == 0` to check if a string is empty.

If the string in question is not empty, the `count` method has to first iterate over all the characters of the string. This is an O(n) operation.
Once the the count is determine, we then check if the count is zero. This method in inefficient and should be avoided.

<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />


<h3 style="color:#29c7ac;">Q. How do you get the first & last character of a string? </h3>

> Swift String type provides the `first` and `last` property to extract the first and last character of a string respectively. 

```swift

var str = "Hello 😈"
str.first // H
str.last  // 😈

```

<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />



<h3 style="color:#29c7ac;">Q. How do you concatenate two strings in Swift ? </h3>

> Swift stirngs can be concatenated using the `+` operator. 

```swift

let str1 = "foo"
let str2 = "bar"

let joined = part1 + part2
// foobar

```
Note the `+` opertor returns a new string.

We can also `append` to an existing string. 

```swift
var variableStr = "foo"

variableStr.append("bar")
variableStr += "bar"

variableStr  // foobarbar

```
Here we mutate the variableStr itself, no new string is created.

<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />

<h3 style="color:#29c7ac;">Q. How do you change the case of a Swift String ? </h3>

> String has provides the following methods & properity to change the case of a string 
> * `uppercased`  - returns an upper cased copy of a string  
> * `lowercased`  - returns an lower cased copy of a string  
> * `capitalized` - returns a capitalized string 

```swift

let testStr = "Lorem Ipsum is simply dummy text of the printing and typesetting industry."

let upperCaseString = testStr.uppercased()
// "LOREM IPSUM IS SIMPLY DUMMY TEXT OF THE PRINTING AND TYPESETTING INDUSTRY."

let lowerCaseString = testStr.lowercased()
// "lorem ipsum is simply dummy text of the printing and typesetting industry."

// Capitalize the first character of every word
let capitalizedString = testStr.capitalized
// "Lorem Ipsum Is Simply Dummy Text Of The Printing And Typesetting Industry." 

```

All of the operations to change the case of a string have a complexity of O(n).
i.e. The time taken to change the case of the string increases with the length of the string.

<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />

<h3 style="color:#29c7ac;">Q. How do you check if two strings are equal ? </h3>

> Swift  
== operator
> 

```swift

```
Using the `==` operator, does an exact match. i.e if the case changes etc. it would be a no match. 

String also aprovides a way to do a caseInsensitiveCompare


// Prashanth check out folding also : https://dev.to/nemecek_f/advanced-string-comparison-and-sorting-in-swift-3n14
```swift

```

<br />
<p style="text-align:center;">───── ⋆⋅☆⋅⋆ ─────</p>
<br />





//: How do you compare if two strings match (ignoring the case)?
str.caseInsensitiveCompare("hello 😈") == ComparisonResult.orderedSame


