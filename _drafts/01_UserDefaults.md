---
layout: post
title: UserDefaults in iOS 
categories:
  - iOS 
---

Initial excerpt and TOC goes here.

- Introduction :
    - What is UserDefaults? Why do we need it?

- User Defaults basics
    -  Its a key value pair.
    -  We work with standard userD

- Saving data to userDefaults 
    - Saving commonly used datatypes Int, 
        - How to save Int, Float to userDefaults
        - How to save Swift Strings in UD
        - How to save Boolean 

    - Saving other types in User Defaults
        - How to save Swift date in UserDefault
        - How to save URL in UserDefault
        - Bascially any propertylist type can be saved to user defaults.
    - Saving collections to user defaults.

    - Saving anything. Images? (Prashanth Save this for another post?)
        - Possible example, Avatar image.

    - Setting Default values for user Defaults.

- Reading from UserDefaults
    - View all Key values pairs.

- Removing items from UserDefaults

- Check if an item exists in UserDefaults
- Removing items from UserDefaults

All apps use some kind of state.
i.e we want to save stuff. User defaults are probably the eaise options.
no setup. and simple api. best for quick and dirty.

The UserDefaults also server as a quick and dirty persistence mechanism for sampple projects, Proof of concpets, where performance and is not the focus.

In this post, we look at how

<!-- toc -->

- [Persisting commonly used data types in UserDefaults](#persisting-commonly-used-data-types-in-userdefaults)
    * [How to save Swift Int, float to UserDefaults](#how-to-save-swift-int-float-to-userdefaults)
- [Persisting commonly used data types in UserDefaults](#persisting-commonly-used-data-types-in-userdefaults)
- [How to save URL to UserDefaults](#how-to-save-url-to-userdefaults)
- [Check if a UserDefault already exists](#check-if-a-userdefault-already-exists)

    How to delete keyValues pairs from UserDefaults

<!-- tocstop -->

Going beyod the standard user defaults.

### Steps to add custom fonts to an iOS project

<!-- toc -->
1. [Review custom font files](#review-custom-font-files)
2. [Add the custom font to your Xcode project](#add-the-custom-font-to-your-xcode-project)
3. [Update the project info.plist](#update-the-project-info-plist)
4. [Access the custom font](#access-the-custom-font)
<!-- tocstop -->

<!--more-->
<br>

---




Most apps give users the option to customize the appearance or the behavior. 

For example :  A news reader app might allow users customize the font size and font family. Or we might allow the user to select between a dark theme and a light theme. 

Once the user has chooses a preference, the app should save the users perference so that it can be applied when the app is launched the next time.

In an iOS we save the user preferences to the user's defaults database. iOS provides the `UserDefaults` class that is used to persist and fetch data from the defaults database.

The data persisted using `UserDefaults` is written to a plist file under the hood. This plist file is stored in the application sandbox. Since the defaults are stored in the apps sandbox, it means other applications on the cannot access the preferences of another app. Also, if the user deletes and app from the device the user preferences will be lost.

To ensure the the preferences stick around even after the app is deleted from your device we can use other options such as storing it in the Keychain or saving it to a key-value store in the user's iCloud account.

 

To learn how to store data in Keychain checkout this post. <Prashanth Link here.>

To learn how to store data in users iCloud account checkout this post. <Prashanth Link here.>

In this post we look at how to save and fetch commonly used data types such as Strings, Numbers, boolean values to the user defaults. We also look at related operations like deleting a value, checking if a default value already exists etc.

Before we dive in, just keep in mind that UserDefaults is not the only place we can use to store the user's preferences. We can also save preferences in the settings bundle.

Typically, in an iOS app, we would display the preferences stored in UserDefaults in a custom screen within the app. The preferences stored in the Settings bundle will be displayed in the Settings app.

See the this post on How to store your application's settings in the settings bundle. <Prashanth Link here.>

The decision on where to store a particular default is a tricky one. Apple provides some guidelines here <Prashanth Link here.>, they recommend option a user would access frequently within the app and preferences the user is not likely to change in the settings bundle. We can access these app setting using the settings app.

To learn how to save data to the settings bundle, check out this post <Prashanth : Link to Settings bundle post here>

In this post was assume you have already make the decision to store something in the user defaults.


## Persisting commonly used data types in UserDefaults

To save data in a the UserDefaults, we associate our data with a key. We can then retrieve it at some later point using the same key.

### How to save Swift Int, float to UserDefaults

```swift
// Saving a Int in the user defaults
UserDefaults.standard.set(100, forKey: "highScore")

UserDefaults.standard.integer(forKey: "highScore")
// Returns 100
```

In the code above, save an integer value 100 using the key "highScore" to the user defaults and then fetch the integer later.

Note how we specify the type (integer) when we are trying to fetch the value back from the UserDefaults.

It is important to specify the correct type. For example in the code below, when we save a floating point value (100.5). When we try to get the value from the UserDefaults. When incorrectly  specify the type of the value as `Int`, we get back 100 instead of 100.5

```swift
UserDefaults.standard.set(100.5, forKey: "temperature")
UserDefaults.standard.float(forKey: "temperature")
// Returns 100.5

UserDefaults.standard.integer(forKey: "temperature")
// Returns 100
```

### How to save Swift String to UserDefaults

Saving string is a also straight forward. 

```swift
// Saving String to UserDeraults
tserDefaults.standard.set("World", forKey: "Hello")

// Fetching String from userDefaults
UserDefaults.standard.string(forKey: "Hello")
```

```swift
// How to save boolean to UserDefaults
UserDefaults.standard.set(true, forKey: "shouldPlayMusic")
UserDefaults.standard.bool(forKey: "shouldPlayMusic")
```

### How to save URL to UserDefaults

```swift
// How to save URL to UserDefaults
// Note : URL is a special case.. otherwise we just store standard types.
let simpleURL = URL(string: "www.google.com")!
UserDefaults.standard.set(simpleURL, forKey: "googleURL")
UserDefaults.standard.url(forKey: "googleURL")
```

```swift
// How to save Date to UserDefaults
UserDefaults.standard.set(Date(), forKey: "someDate")
UserDefaults.standard.value(forKey: "someDate") as! Date
```

Saving other data types to 

// Talk about property list objects.

Basically any propertylist type can be stored without any manipulation in the userdefaults.

Property lists consist only of certain types of data: dictionaries, arrays, strings, numbers (integer and float), dates, binary data, and Boolean values.  // From Apple.


### Check if a UserDefault already exists

If we want to check if a userDefault exists before we add it. might be a good idea to 

We can check to see if a key is nil. T

```swift
UserDefaults.standard.value(forKey: "someNonExistentKey")
```
## A better way to manage UserDefaults keys

It is easy to introduce bugs in our apps when we use Strings as keys. Its hard to miss to differnce between shouldPlayMusic and shouldplayMusic. This can be source of bugs in your app.

We an mitigate this problem by not using strings directly in our app. Instead we can store the keys in a struct or an enum and use these constants to fetch and add values to the UserDefaults.

Using an enum to safely store values in the UserDefaults

```swift
enum UserPreferences : String {
    case numberOfChildren
    case shouldPlayMusic
}

// Saving a file
UserDefaults.standard.set(100, forKey: UserPreferences.numberOfChildren.rawValue)
UserDefaults.standard.set(true, forKey: UserPreferences.shouldPlayMusic.rawValue)
```

In the code above, we use the UserP

`UserPerferences.numberOfChildren.rawValue`

## Setting default values for UserDefaults

An alternative is set an appropriate initial value for all the key-value pairs we plan to save store in the User defaults. We use the register method

```swift
UserDefaults.standard.register(defaults: ["someKey1" : false, "someKey2" : "someValue2"])

UserDefaults.standard.bool(forKey: "someKey1")
// true

UserDefaults.standard.value(forKey: "someKey2")
// "someValue2"
```

---

## Remove a key-value pair

```swift
UserDefaults.standard.value(forKey: "Hello")
UserDefaults.standard.removeObject(forKey: "Hello")
UserDefaults.standard.value(forKey: "Hello")
```

## View all key-value pairs

We can view all the values stored in the UserDefaults using the `dictionaryRepresentation` method.

This method can be useful to get a view of all the defaults. Its also a good way to check if you have any stale/unused defaults that you might have forgotten to remove.

```swift
// See all default values
UserDefaults.standard.dictionaryRepresentation()

// Output
[
"numberOfChildren": Oops,
"simpleArr": <__NSCFArray 0x6000013df0c0>(Good,Bad,Ugly),
"someDate": 2020-02-0513:58:10+0000,
"simpleDict": {
	Hello = World;
	nested = {
		innerKey = innerValue;
		};
},
"shouldPlayMusic": 1,
"highScore": 100,
"AppleITunesStoreItemKinds": <__NSCFArray 0x6000025d5180>(eBook, artist),
"NSLanguages": <__NSSingleObjectArrayI 0x6000032d4140>(en)
]
```

---

## Things to remember about UserDefaults

**The values retrieved from UserDefaults are immutable.**

Lets look at this with some sample code. In the code below, we have set the value of the "someDateKey" to the current date.  As expected the value stored in the UserDefaults is the current date.

We then fetch the value an store it in a variable called `fetchedDate`.

Even though fetchedDate is of type NSDate which a class, we might think that modify fetchedDate should also change the value stored in the UserDefaults.

Its important to note that changing the value of fetchedDate, does not affect the value stored in the UserDefaults.

```swift
let today = NSDate()
UserDefaults.standard.set(today, forKey: "someDateKey") //"Feb 5, 2020 at 9:59 PM"

var fetchedDate = UserDefaults.standard.object(forKey: "someDateKey") as! NSDate
fetchedDate.addingTimeInterval(3600 * 24)

UserDefaults.standard.object(forKey: "someDateKey") as! NSDate
```

To change the value of the Date, you will have to use the set: method.

```swift
let changedDate = Date().addingTimeInterval(3600 * 24)
UserDefaults.standard.set(changedDate, forKey: "someDateKey")
```

Ensure you are using correct keys while updating values.

If there is an existing key-value pair, if you set a new value the old value will be overwritten even in the Data types of the values differ.

For eg. If you have a key value pair "highScore" : 100 and if you inadvertently set the "highScore" to "Random String"  the value stored in the UserDefaults will be overwritten. You will not get any warning that the types dont match.

Integer For key method will return 0 for invalid values.

When you use the integerForkey method, and try to fecth the value of a non existent key or value that is not an integer. The UserDefaults will return 0 and not nil. So its important to watch out for this.

```swift
UserDefaults.standard.integer(forKey: "SomeNonExistentKey")
```

---

## What next?

As you have seen the UserDefaults make it easy to save data types ... like... .

In the following post we will look at when we should choose user defaults to store images data.. and also what type of data is best suited to be stored in the user defaults.

We will look at how to persist non property list types (eg images). We will also look at how to persist custom objects.

The user’s defaults are stored locally on the device. Which means if a user uses your app on multiple iOS devices (eg an iPhone and an iPad) the defaults are not shared.

To share userDefaults across a users devices, we can use the NSUbi .. which uses the users' iCloud account to store the data.

We will also look at how we can make our app watch for changes to a user default values and react to those changes.
