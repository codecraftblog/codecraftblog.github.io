---
layout: post
title:  "Converting Dates to String using Swift"
date:   2020-08-22 21:16:00 +0530
categories: iOS Swift DateFormatter 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

Dates are represented in Swift code using the `Date` structure. 

The `Date` type makes it easy to work with dates. You can easily perform operations like persisting `Date` instances, comparing two Dates, checking if the given `Date` falls on a weekend, etc.

While the `Date` type is great for working with dates in code, they often need to be converted to their corresponding `String` representation before it is displayed to the user.

Converting types like Int, Float, etc to their string representation is easy; however, converting `Date` instances to their `String` representations is not a trivial task.

You have to account for the different date formats used in different geographic regions, handle localization, and consider the context in which the date is used.

Given how ubiquitous `Date` is and how often we have to manipulate them, it is wise to isolate `Date` handling code.

In this post, we look at how to convert date to string. In general, we look at how we can abstract away all the nuances of processing dates using the DateFormatter class. 

We also see how to convert Date to String that appears more natural for eg: strings like Today, Yesterday, etc.

<!--more-->

<br>
## Converting Date to String  

We first create Date instance and then convert it to a String. 

```swift

let today = Date() 
// 2020-08-22 10:50:48 +0000

let strDate = "\(today)"
// 2020-08-22 10:53:05 +0000

```

The generated date string has all the information, but it is not easy to read and it is not something you would display to the user.

Lets look at how this can be improved.

<br>
## Introducting the DateFormatter 

The `DateFormatter` class helps convert `Date` objects to their corresponding `String` representations and back.

The code shows the 3 steps required to convert a `Date` to a `String` in Swift.

```swift

// Convert string to Date
let dateF = DateFormatter()  // 1

dateF.dateStyle = .short     // 2 

dateF.string(from: today)    // 3 
// "8/23/20"

```

1. Create a `DateFormatter` instance.
2. Select a dateStyle. Here we choose the .short style. 
3. Call `string(from:)` method on the `DateFormatter` instance.

The other dateStyles we can choose from are .medium, .long, .full, and .none. 

```swift

dateF.dateStyle = .medium
dateF.string(from: today)
// "Aug 23, 2020"

dateF.dateStyle = .long
dateF.string(from: today)
// "August 23, 2020"

dateF.dateStyle = .full
dateF.string(from: today)
// "Sunday, August 23, 2020"

```

<br>
## Convert Date to "natural" strings : "Today", "Tomorrow", "Yesterday" etc

The different dateStyle properties works well for most cases but they tend to be formal. In some situations, we might want a "natural" or Human friendly string representation. i.e Words we would use in everyday conversations.
For eg: If you plan to meet your friend the next day. You might say, *"I will meet you tomorrow"* rather than *"I will meet your on Aug 24, 2020"*

DateFormatter comes to the rescue again. We can set the property `doesRelativeDateFormatting = true` and viola.

```swift

dateF.doesRelativeDateFormatting = true

dateF.string(from: today)
// "Today" 

let tomorrow = Calendar.current.date(byAdding: .day, value: 1, to: today)!
dateF.string(from: tomorrow)  
// "Tomorrow" 

let yesterday = Calendar.current.date(byAdding: .day, value: -1, to: today)!
dateF.string(from: yesterday)  
// "Yesterday" 

```

<br>
## Effect of Locale on Date format

The code samples have seen so far return strings in English. That is because the current locale on my Mac is set to `en_US` 

Based on your Locale your results by might be different. You can check your current locale by inspecting `Locale.current` property.

Dates are represented in different ways in different parts of the world. Not to mention, even within the same region the date format even vary based on the context in which the date is used. 

This excerpt for Wikipedia summarizes how Locales and Dates are related. 

>The legal and cultural expectations for date and time representation vary between countries, and it is important to be aware of the forms of all-numeric calendar dates used in a particular country to know what date is intended.
> Wikipedia 

For instance, the date **01/10/20** will be interpreted as **10 January 2020** in the USA, but the same date in India would be **01 October 2020**.

To get a sense of how complex this can get, take a look at this Wikipedia page. https://en.wikipedia.org/wiki/Date_format_by_country

Solution? DateFormatter again. Currently, the DateFormatter supports almost 900+ Locales. 
For a complete list of supported Locale print the `Locale.availableIdentifiers` property.

The DateFormatter automatically picks up the user's current locale and formats the output appropriately. 

In most cases, the default behavior is what the user would expect and it should not be changed. 
If changing the Dateformatter's locale is appropriate for your app, you can set the locale as shown below. 

```swift

dateF.locale = Locale(identifier: "de")
dateF.string(from: today)

dateF.locale = Locale(identifier: "fr")
dateF.string(from: Date())

dateF.locale = Locale(identifier: "ja_JP")
dateF.string(from: Date())

```

**Table : Date to String conversion based on Locale**

| | en_US| hi_IN| de_DE
| ------------- |:-------------|:-----|:-----|
| Today - 2 days | -- |परसों | Vorgestern|
| Today - 1 day | Yesterday | कल | Gestern|
| Today | Today | आज | Heute|
| Today + 1 day  | Tomorrow | आने वाला कल | Morgen | 
| Today + 2 days | -- | आने वाला परसों | Übermorgen |

<br>

In this post, we looked at how a seemingly simple task - converting a date to a string - can quickly become complex.
The Date to String conversion depends on the region, language, context, etc.

Given all the complexity involved with formatting dates it is best to let Dateformatter do all the heavy lifting.
It's an offer you cannot and should not refuse. 

----
<br>
## References:
Apple Developer Documentation: [Date](https://developer.apple.com/documentation/foundation/date)

Apple Developer Documentation : [DateFormatter](https://developer.apple.com/documentation/foundation/dateformatter)

[Unicode Common Locale Data Repository](http://cldr.unicode.org/translation/date-time-1/date-time-patterns)

