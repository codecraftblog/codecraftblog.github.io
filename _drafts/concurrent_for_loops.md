---
layout: post
title:  "Draft - Parallelism - Running loops" 
date:   2021-06-03 21:16:00 +0530
categories: iOS 
author:
  name: Prashanth 
  twitter: johndoetwitter
---

Writing Faster loops in iOS.

Loops are a common used in almost all programs.
In the post we look at how we can use the available Multi core hardware to speed up loops by executing multiple iterations in parallel.
iOS? more specifically the GCD provides the concurrentPerorm API that allws us to execute loops in paralle and geain a boost.
But wait.. there's a catch. There are few things to keep in mind, for eg: Synchrnization, dependencies.. 

In this post we look at how we can use loops to make speed up our loops.

Executing code in parallel. 
Makes use of the multiple cores.. do things in parallel.

https://en.wikipedia.org/wiki/Multi-core_processor
A multi-core processor is a computer processor on a single integrated circuit with two or more separate processing units, called cores, each of which reads and executes program instructions.[1] 

So the point is we a multiple cores that can all execting tasks is parrallel. We need to write our code in such a way that it can be broken into pieces that can each execute in a different process.

## Introduction

Before dive into loops, lets talk about Pepe

This is Pepe. Pepe has inherited the family pizzieria.  

Being a very old store, it has a single wood-fired oven. The oven can only bake one pizza at a time, and it takes exactly 12 mintues to bake one pizza. 

When a customer walks in, Pepe goes through these steps :
- Takes the order    - 1 minute
- Prepares the dough, adds Sauce & toppings  - 1 minute
- Bakes the pizza       - 10 mins

Customer will have to wait for 13 minutes for her pizza.





# Parallelism and concurrency are different things. 
Parallelism vs Concurrency

There is a lot of debate on the exact defintion of concurrency vs paralleism.. but for us.. we will use an analogy. 

https://stackoverflow.com/questions/1050222/what-is-the-difference-between-concurrency-and-parallelism
https://docs.oracle.com/cd/E19455-01/806-5257/6je9h032b/index.html
Concurrency - A condition that exists when at least two threads are making progress. A more generalized form of parallelism that can include time-slicing as a form of virtual parallelism. 
Parallelism - A condition that arises when at least two threads are executing simultaneously.

Lets look at an analogy:

Runs the family Pizza Store, 
1 ovens (small one, can take only one pizza at a time), each pizza takes about 12 minutes to bake. 

- Prepare the dough  ?? take the order    - 1 minute
- Add Sauce & Toppings  - 1 minute
- Bakes the pizza       - 10 mins

Prashanth : Initial Animation goes here. Serving one customer at a time.
Customer will have to wait for 13 minutes for her pizza.

Word spreads and more people start visting the Pizza Store.
There is a problem, since we can only bake one pizza a time, if two customers walked in at the same time.. the first one would get his pizza in 12 mins, but the sconed cusotmer would have to wait 24 mins before he can eat.
Prasanth : Add the timeline chart here.

Ofocurse this is will not scale, if there are 3 cusomters in a gque.. 36, 48 mins.. pretty soon we will have lots of hungry and angry customers. 

Concurrency :
One tweak that John can make, when one pizza is baking, he can prepare the order of tne next customer, that would make the problem better.
We still have the same over and john but the oven is still the bottleneck.
One way to make this process smoother, is to take and prepare the next order when one pizza is baking. 
This way multiple customers can be processed at the same time. 
This is concurrency, we are still baking one pizza at a time, but by performing other tasks we when wait, we are making things more a little bit faster. 

i.e two customer are being servered, but thier pizzas are not necssiary baking. :D

Parallelism :
Install more ovens installs an additional oven. Now we can process two pizzas at the same time. i.e we can serve 2 customers at the same time.
Throw more resources the problem.

Few things have happened.

Jeff has assumed a new role, load-balancer. He now has to manage two oven to make sure they dont get burnt.

How many ovens?
So we can see that two is better than one, but why not 3 or 100.
Depends on Jeff.

Jeff the chef is the load balancer.

What if you install 50 ovens? Jeff will still be the road block.

### Paralleism is not free.
Few things to note here :
- Idle resources are not good.
- Requires more work. & Co-ordination and synchrnoization.
- There is a limit.. on how much we can parallelize (Amhdals law) 
- Hard to predict, so measure measure measure.

Thats about 13 minutes to finish one pizza.
Lets say its early in the day and only one customer shows up. She would have to wait 13 mins to get her pizza.
A little later a two more customers walks in and he is placing his order. A third cusomter is waitingin the queue. He definitley would not like to wait for 13 mins. before placing his order. 
That would be bad for business.

Next, lets say, the word spread around and business is really good. We have added a new chefs, but the oven is still a problem.

Now the problem is obvious and infacty silly, "use the other ovens"
simple Jeff now uses multiple ovens so that mulitple pizzas can bake at the same time.
If used effectively, we can 

Jeff the chef is the load balancer.

What if you install 50 ovens? Jeff will still be the road block.


https://developer.apple.com/videos/play/wwdc2017/706/?time=175
Parallelism : Simultaneous execution of closely related computations. (Across different cores) - Requires multiple cores.

Concurrency : Composition of independently executed tasks. - Can even be done on a single core. 

------------

### Sample code here

Lets look at an example, say we have an app that dispalys photos with a different types of filters.
We start with a bunch of images that we want apply a mono-chreom filter to.

```swift
import Foundation
import UIKit

struct ImageUtils {
   
    @discardableResult
    func applyMonochromeFilter(inputImage : UIImage) -> UIImage? {
        guard let currentCGImage = inputImage.cgImage else {
            assertionFailure("Image Processing Failed")
            return nil
        }
        
        let currentCIImage = CIImage(cgImage: currentCGImage)
        
        let filter = CIFilter(name: "CIColorMonochrome")
        filter?.setValue(currentCIImage, forKey: "inputImage")
        filter?.setValue(CIColor(red: 0.5, green: 0.5, blue: 0.5), forKey: "inputColor")
        filter?.setValue(1.0, forKey: "inputIntensity")
        
        guard let outputImage = filter?.outputImage,
              let processedCGImage = CIContext().createCGImage(outputImage, from: outputImage.extent) else {
                  assertionFailure("Image Processing Failed")
                  return nil
              }
        
        return UIImage(cgImage: processedCGImage)
    }
}

```

To convert and image we call the applyMonoChromeFilter method 
```swift
let inputImage = UIImage(named: "54_kb.jpeg")!
let imageUtils = ImageUtils()

let _ = imageUtils.applyMonochromeFilter(inputImage: inputImage)

```

Output : Show the total time. This is our bench mark.

On an iPhone 11 this takes about 0.017 secs.

------------
## Loops
To process multiple images, wc an use a simple for loop. 

```swift
// Simple For Loop :

lazy var allImages = Array(repeating: inputImage, count: 100)

for image in allImages {
    let _ = ImageUtils().applyMonochromeFilter(inputImage: image)
}

```

On an iPhone 11 this takes about 1.4 secs.

![Time to process image in a loop](/images/concurrent_loops/graph_loop_time.png)

We can see that time to convert n images should take n * t where t is the time to convert each image.
so in our case, converting 100 images should take 100 * 0.01 = ??? secs

The time taken is proportional to the number of images to convert O(n)
O(n) will begin to be a problem to be a problem if the number of images are large. 

The code above has a few things going for it. 
1. For loops are easy to understand & it works for in a wide variety of applicaitons. ??
2. O(n) time may not work for all scenarios.Its slow. As we add more images its going to take longer.

Disadvantages :
1. Doesnt take full advantage of the hardware.
- Running on the main thread.
- Blocks all other tasks when main thread is busy. 

To get a better understanding of whats going on behind the scences. Introducing Instruments.

If you are new to instruments and want to understand how to profile your app, look at this blog post.
To learn how to profile you app using instruments check out this post ---link---

![Code runs on the main thread](/images/concurrent_loops/concurrent_loops_001.png)

- Running on the main thread.
- Blocks all other tasks when main thread is busy. 


![Code runs on the main thread](/images/concurrent_loops/instruments_idle_cpu_cores.png)


1) All the code is running on the Perofnce cores, which are more expesive
2) The effiecient cores are sitting idle. and thats a waste of resources.

https://www.trustedreviews.com/news/apple-a13-bionic-3936887

This is like jeff the pizza guy, he has 8 ovens at his disposal, yet he uses just one of them and keeps his customers waiting.

Problems :
    Hardware is not fully utilized.
    We are runnning on the main thread, means app wont be repsonsive at these times.
    CPU consumption - show it can go > 100%

Running a for-loop, show how this loop runs on the main thread.
Show which CPU core is used, and how the other threads are sitting idle.

Lets look at how to fix this.

--------------

# We can do better

Our objective is two-fold :
1) Get off the main thread.
2) Use all the resources at our disposal.

GCD provides helps us get off the main thread, we need to use DispatchQues. --link to dispatchQ post here --
Divide our picture array in to chunks and process these chunks in parallel on the different CPU Cores we have idle.


```swift
extension Array {
    func chunked(by distance: Int) -> [[Element]] {
        let indicesSequence = stride(from: startIndex, to: endIndex, by: distance)
        let array: [[Element]] = indicesSequence.map {
            let newIndex = $0.advanced(by: distance) > endIndex ? endIndex : $0.advanced(by: distance)
            //let newIndex = self.index($0, offsetBy: distance, limitedBy: self.endIndex) ?? self.endIndex // also works
            return Array(self[$0 ..< newIndex])
        }
        return array
    }
    
}
```


_Utilizing the multiple cores_

Introduce GCD concurrentPeform.

```swift
let chunkSize = 20
let chunks = allImages.chunked(by: chunkSize)

DispatchQueue.concurrentPerform(iterations: chunks.count) { iteration in
    let chunk = chunks[iteration]
    for image in chunk {
        let _ = ImageUtils().applyMonochromeFilter(inputImage: image)
    }
}
```

Output : Time to process = 0.34 secs 
Thats a 4x faster

![Time to process image in a concurrent loop](/images/concurrent_loops/graph_concurrent_loop_time.png)

------------

There is the addtional overhead of spliting the input array, but we still got an impressive 4x boost.

![worker threads are busy](/images/concurrent_loops/instruments_not_idle_worker_threads.png)

![Other cores threads processing images](/images/concurrent_loops/instruments_not_idle_cpu_cores.png)

### Choosing an iteration count.

Iteration count was 5 here multiple of the number of cores.. but measure measure measure.
the size of each chunk should be suffielnty larget, breaking ito into very small chunks will slow down due to the additonal overhead of load balancing.

Its a blanace between the effor required to load balance vs time to exect


### Moving stuff completely off the main thread.
Theres is still some processsing happening on the main thread.

Assingng a different queue.

```swift

```

Dispatch async to a a worker thread, we can move things off the main thread. 

### How fast can we make Parallel code run?
Amhadals law goes here?
https://en.wikipedia.org/wiki/Amdahl%27s_law

------

In the example we saw above, running loops in parallel was trivial. By breaking down the loop in smaller chunks we were able to run code in parallel and got a nice boost. But it can that easy . There hos to be a catch. Unfortunatley there is.

### Whats the catch?

The `applyMonoChromeFilter` method is pure function, i.e. it takes in image as input and returns another version of the image or nil.
It does not depend on the sorrounding scope. Also within the loop, we simply discard the output image.

The logic within the loop was did not have any dependency on any surrounding code, therefore, it could be run in parallel and each parallel exeuction did not affect any other iteration.
There's a fancy term for code that behaves this way. The code is said to be "Rentrant"

> A computer program or subroutine is called reentrant if multiple invocations can safely run concurrently on multiple processors, or on a single processor system, where a reentrant procedure can be interrupted in the middle of its execution and then safely be called again ("re-entered") before its previous invocations complete execution.  wikipedia
https://en.wikipedia.org/wiki/Reentrancy_(computing)

That made it easy for use to simply extract the contents of our loop and execute it within the concurrentPeform.

This is example of an embarssingly-parallel loop. Another fancy term.

However this loop is not very useful, since we dont keep the converted image. 

Let say we want to apply a filter and store the output image in an array.
our code will look like this.
Sample code : loop updates an array.
```swift
```

Prashanth : Add the Bad Access error code. here.. 
We read from the input array and keep the converted images say in another array. 
This would mean our code is no longer re-entrant, i.e we different parts of our loop needs to update the same output array. 

So here we would have two iterations would have something in common. Different iterations running on different threads could try to update the output 
array the same time. This can lead to upredictable or in consistent behavior.
https://docs.swift.org/swift-book/LanguageGuide/MemorySafety.html

So, we need some way to synchronize access to shared resources. The output array in our case.

In general, here are some of the problems with concurrent loops. 

1) Increased complexity - We now have code to chunk 
2) Global state. - Shared input (Privitization) and Shared output (Synchornization)
2) Dependency is an enemy of .. parallelism. 
3) Increased Memory consumption, all those additional chunks we created will add to the memory.
5) Ambiguity.

However, the benefits outwight the cons.
- Phones are powerful and its a crime to not use it.
- Pefromanc is not optional.

So lets look at some ways to solve these problems.

### Solving problems related to concurrency.

-Wikipedia https://en.wikipedia.org/wiki/Privatization_(computer_programming)
Dependencies arise when two or more reading or writing a variable at the same time.
Privitization gives each thread its own copy. This can be written to simultaneusly.. and there fore parallely. 

Read/Write Conflicting variables introduce dependencies between different execution threads and hence prevent the automatic parallelization of the program. 
The two major techniques used to remove these dependencies are 
    Privatization : Privatization gives each thread a private copy, so it can read and write it independently and thus simultaneously. 
    Reduction : Each thread is provided with a copy of the R/W Conflicting variable to operate on it and produce a partial result, which is then combined with other threads' copies to produce a global result.[1]

## Privatization

First try reduction & then privatization. 

Reduction : Increases Computational overhead.
Privatization is a space-time trade-off. : Increases the memory consumed by the program.
Mutual Exclusion (sync) : Helps reduces memory 
There is this other thing called expansion.

Mutual exclusion is another technieque.. here we dont have the space time trade off.. but its slower.



#### Synchronisation

We run into problems of synchronization, multiple threads trying to write to the same object.
We need some mechanism to synchoromize the writes. 

problems to addres :
https://en.wikipedia.org/wiki/Synchronization_(computer_science)

Why we need synchornization.

Mutual Exclusion : https://en.wikipedia.org/wiki/Mutual_exclusion
https://en.wikipedia.org/wiki/Lock_(computer_science)

- Serial Queues
- Semaphore

Refer to this post to understand how to use Semaphores. <!-- link to semaphore post -->

#### Using Serial Queue to synchornize access to an array


#### Privatization 

### Eliminating depenecy in code.

------
### Cost of Parallellism


## Parallelism
Here we look at how we can use all the cores and run our code in parallel.introduce loop level parallelsim.
or lets not talk about loop-level paralleism..
In this post we will focus on loop level parallelism.
https://en.wikipedia.org/wiki/Loop-level_parallelism

# Running our loop in Parallel

# Parallel for loops.
Split our loop code into splits.

1) Break our data into chunks. 
2) Process each chunk separately.
3) Join the data back.

Prashanth : Add the Bad Access error code. here.. 

Challenges : 
Aceess to array is not theard safe, dead locks, if more than one thread tries to write to our array at the same time.
Size of the task.

Whether you can parallelize loops.. You cant just take you input loop.

Embrassingly Parallel loops : Just run it parallel and it works.
Each iteration is independent of others. However such things are hard to find.

There are different types of dependencies you can encounter in  your code :
1) Shared input  - Multiple threads reading from a same input (chunk it out)
2) Shared Output - Multiple threads are writing to the same entity

- Or both ?

in most cases, we need to do some task to extract tasks from loops that can be parallelized.
Usually this is happens when data is stored in "Random Access Data Structures"
This amount of gain (spped up) is usually inline with Amdahl's law.

Loop level Parallelism : 

DOAll - Just run all the statemsnts in a loop parallely
DOAcross - 

https://en.wikipedia.org/wiki/Embarrassingly_parallel
https://en.wikipedia.org/wiki/Parallel_slowdown

Prashanth This is some generci thing.. i.e Dependencies that exist in code. Not limited to loops.

Enemys of parallelism

# Types of dependencies in *code*
- true Dependence -> A Writes to Some location that is next read by B
- anti Dependence
- Output Dependence
- Input Dependence



## Things to consdier :
Size of the block. Cost of Load Balancing vs Benefits of parallelism.

## Examples to show :
Data Parallelism 
  -> Adding all the elements of an array
  -> Finding the average of elemenets of an array 
  -> filter the elements of an array. / Count .. filter will lose the sequence.
 

  -> Covert images in to a different format all the images in an Dictionary.
  -> & Resize the image.
GOL = Finding the neighbour count
    -> Struct to represent a cell. 


# Pitfalls:
Some way to lock.
Semaphoore
Using serial queues

Conclusion : Always mesasure.. 

Live example :

## Multi Core CPUs:
What are multi-core CPU's
How to use them.

As we saw on the instuments tool, we are running this code on a CPU with multiple cores and these cores are sitting idle. We also have the main thread blocked.

Modern computers are multi core processores

A multi-core processor is a computer processor on a single integrated circuit with two or more separate processing units, called cores, each of which reads and executes program instructions.[1]

------------

References : 
https://www.hackingwithswift.com/example-code/media/how-to-desaturate-an-image-to-make-it-black-and-white

