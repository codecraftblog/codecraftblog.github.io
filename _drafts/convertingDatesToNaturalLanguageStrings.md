---
layout: post
title:  "Draft - Converting Dates"
date:   2020-08-22 21:16:00 +0530
categories: iOS 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

# Converting Dates to Natural Language Strings like Today, Tomorrow, Yesterday etc.

## Introduction/Overview :

Mention Foundation Frammework man.

Dates are respresented in Swift code using the `Date` value type. 
In Swift Date structure makes it easy to work with dates and, there are a methods that can be used to compare two dates, create a date from a time interval and more.

While `Date` is the great for storing and workign with Dates, they are not ideal to dispalay in a user inteface.

Often we also want to display dates in the User Inteface. It makes sense to dispaly dates in a more human friendly way, space constarains ithe UI, Localization conceners etc. For this we want to format the date before display and again we use a class called DateFormatter to convert dates to a string. 

tke a look at this wikipedia post : https://en.wikipedia.org/wiki/Date_format_by_country

<!--more-->

Converting dates to into not a trivial task. You have to take into accoutn different formatting rules based on the User's region, Locale etc.
Localization. 
- For eg : USA useses moth first.
- Some countires use the .

Given how ubiqueiots Dates are an how often we have to manipulate them, its always wise to abstract away all these nuances and let apple framework handle it for us.

In this post, we look at how to convert date to string. Especially we focus on how ot convert date to strings that appear more natural for eg: 
Strings like "Today", "Yesteray"

## Converting Date objects to String  

`Date` are great when you want to store or manipulate dates. To display dates to the user using an UI element for eg a UILabel. We need to convert the Date to a String. , we need to convert it to a String. 

Lets first look at how to convert a Date to String. .This is not meant to be exxtensive.

```swift
let today = Date() // 2020-08-22 10:50:48 +0000

let strDate = "\(today)"
// 2020-08-22 10:53:05 +0000
```

This default datea string has all the ifnormaiton but its not a good candidate to display to the user.

The `DateFormatter` class helps convert Dates to string and vice verca.

Here we only look at how to convert Dates to different string representations String.
The date formatter come with a wide set of presets. lets look at afew. 

```swift
// Convert string to Date
let dateF = DateFormatter()

dateF.dateStyle = .short
dateF.string(from: today)
// "8/23/20"

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
## Convert Date to natural strings "Today", "Tomorrow", "Yesterday"

The dateStyle property works well for most cases but they tend to be more formal. In some situations we might want a "natural" or friendly string representation. 
For eg: If you plan to meet your friend the next day. You might say, *"I will meet you tomorrow"* rather than *"I will meet your on Aug 24 2020"*

Date formatter comes to the resuce again. We can set the property `doesRelativeDateFormatting = true`.

```swift
dateF.doesRelativeDateFormatting = true

dateF.string(from: today)// "Today" 

let tomorrow = Calendar.current.date(byAdding: .day, value: 1, to: today)!
dateF.string(from: tomorrow)  // "Tomorrow" 

let yesterday = Calendar.current.date(byAdding: .day, value: -1, to: today)!
dateF.string(from: yesterday)  // "Yesterday" 
```

## What about localization? 
## How Locale affects the date format..   

All the code samples have seen till now return the output in English. That is because my current locale is set to `en_US` 

Based on your Locale your results by might be different. You can check your current locale by printing `Locale.current`

Dates are respresented in different ways in different parts of the world. The date formats even vary based on the context in which they are used. 
For eg : The date 01/10/20 will be 10 Januaray 2020 in the USA, but the same date in India would be read as 01 January  2020 in India.
In the US the month is written first follow by the date and whereas in India, the day come s first.

To get a sense of how complex this can get, take a look at this wikipedia page. There are a ton of different formats and sometimes the formats vary even within the same region.

As this wikipedia article metions.
Wikipeida : 
The legal and cultural expectations for date and time representation vary between countries, and it is important to be aware of the forms of all-numeric calendar dates used in a particular country to know what date is intended.

Solution? DateFormatter again. At the time of this writing i see that the DateFormatter supports almost 900+  

The date formatter automatically picks up the user's current locale and formats the output based on the converentsion relevant to that Locale. 

Normally, you would expect the formatter to not set the Locale, you want to the user to have control over the Locale. 
If for some reaseon you want to set the local you can.

An importnat takeaway is that handing date formats is not easy and its best to let the dateformatter handle it for you.


Table below lists example of how the output might shoe for different Locaes.

By default your Locale is picked up automtically. 

Locale IDs

A locale ID identifies a specific region and its cultural conventions—such as the formatting of dates, times, and numbers. To specify a locale, use an underscore character to combine a language ID with a region designator, as shown in Table B-5. For example, the locale ID for English-language speakers in the United Kingdom is en_GB, while the locale for English-speaking residents of the United States is en_US.

https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/LanguageandLocaleIDs/LanguageandLocaleIDs.html

You can change the display by setting the locacle.

```swift
dateF.locale = Locale(identifier: "de")
dateF.string(from: today)

dateF.locale = Locale(identifier: "fr")
dateF.string(from: Date())

dateF.locale = Locale(identifier: "ja_JP")
dateF.string(from: Date())

```

| | en_US| hi_IN| de_DE
| ------------- |:-------------|:-----|:-----|
| Today - 1 day | right-aligned |परसों | Vorgestern|
| Today - 2 days | centered      | कल | Gestern|
| Today | centered      | आज | Heute|
| Today + 1 day  | centered | आने वाला कल | Morgen | 
| Today + 2 days | centered | आने वाला परसों | Übermorgen |


## Formatting context
Prashanth : We could ditch this... 

Based on where we migth want ouse this.. we have to 

NSFormattingContextUnknown An unknown formatting context.
NSFormattingContextDynamic A formatting context determined automatically at runtime.
NSFormattingContextStandalone The formatting context for stand-alone usage.
NSFormattingContextListItem The formatting context for a list or menu item.
NSFormattingContextBeginningOfSentence The formatting context for the beginning of a sentence.
NSFormattingContextMiddleOfSentence The formatting context for the middle of a sentence.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| .unknown      | right-aligned | $1600 |
| .dynamic | centered      |   $12 |
| .standalone | are neat      |    $1 |
| .listItem | are neat      |    $1 |
| .beginningOfSentence | are neat      |    $1 |
| .middleOfSentence | are neat      |    $1 |


It here to too, based on the locale appropriate resprestions. The table below lists the these relative date representations in a few languages.

For eg: For English Locales, you the represent "Yesterday","Today" and Tomorrow
For other Locales like Germany, you can represent dates like Day aftere tommorrow. 

##Conclusion :
We have seen how using the 
Abstract away the complexity.
Context, Localiztion for free..

When you consider all the heavy lifting thats done for you.. its an offer you cannot and should not refuse. 

----
<br>
## References:
Apple Developer Documentation : Date - https://developer.apple.com/documentation/foundation/date
Apple Developer Documentation : DateFormatter - https://developer.apple.com/documentation/foundation/dateformatter
Unicode Common Locale Data Respository : http://cldr.unicode.org/translation/date-time-1/date-time-patterns
