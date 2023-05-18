Notes :

What are sign Posts?
How to create a signPost?
What are the different types of signposts?
How to emit an event?
How to emit an interval?
How to view in instruments?
How to build a custom instrument

How to handle serveral intertwined tasks?
    - Using signpost IDs to Disambiguate.

Introduce SignPost 

What is a SignPost?
What is a Signposts? Introduce the Generic term (in english)

SignPost in iOS or here?
An object for measuring task performance using the unified logging system.

It is a struct


Introduce SignPoster
> Use OSSignposter to create signposted intervals in your code, and then use Instruments’ os_signposts instrument to record those intervals as you run your app and perform the actions to measure. Instruments displays signpost data visually in a timeline.

Creating a SignPost
```swift
let signPoster = OSSignposter()
```

Disabling a signPost 
This is helpful when you want to disable in prod builds.
You dont have to run through your code to figure out where all to modify your code. 

```swift
let signposter: OSSignposter
#if DEBUG
signposter = OSSignposter()
#else
signposter = OSSignposter.disabled
#endif
```


Events :
### What are Events?

### How to emit an event?
Events are 

```swift
let signPoster = OSSignposter()

let signPostID = signPoster.makeSignpostID()
signPoster.emitEvent("App Init - Start", id: signPostID)


signPoster.emitEvent("App Init - End", id: signPostID)

```
### How to read an event?

SignpostMetadata


OSSignpostIntervalState
Oh this is the just the guy that gets returned. 
Check this out.. you can start a process in one and measure in another.
Just serialze and pass the state over to the other guy.


### Other possible topics
Emitting signposts from JavaScript? JSC?


Research Topics :

Apple has this thing called unified logging tools. 
A unified logging system for the reading of historical data.
OSLog Framework.


The OSLog framework allows you to read logs. With the unified logging system, you can build custom debugging and analysis tools to be used alongside Apple tools like Instruments and Console. To learn more about how to create logs, see Logging.

Here are intersted in how to use instruments ..

We can do 2 things with the logging framework; https://developer.apple.com/documentation/os/logging
1. Log Messages.
    a. Generating Log Messages.
    b. Viewing Log Messages
    c. Customizing Log messages during debugging.
2. Measure Events


### Reference :
https://pspdfkit.com/blog/2018/using-signposts-for-performance-tuning-on-ios/
https://www.donnywals.com/measuring-performance-with-os_signpost/
https://www.jviotti.com/2022/02/21/emitting-signposts-to-instruments-on-macos-using-cpp.html
