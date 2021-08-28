---
layout: post
title:  "Draft - Parallelism - Running loops" 
date:   2021-06-03 21:16:00 +0530
categories: iOS 
author:
  name: Prashanth 
  twitter: johndoetwitter
  picture: /images/profilePhoto.jpg
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


# Parallelism and concurrency are different things. 
Parallelism vs Concurrency

There is a lot of debate on the exact defintion of concurrency vs paralleism.. but for us.. we will use an analogy. 

https://stackoverflow.com/questions/1050222/what-is-the-difference-between-concurrency-and-parallelism
https://docs.oracle.com/cd/E19455-01/806-5257/6je9h032b/index.html
Concurrency - A condition that exists when at least two threads are making progress. A more generalized form of parallelism that can include time-slicing as a form of virtual parallelism. 
Parallelism - A condition that arises when at least two threads are executing simultaneously.

Lets look at an analogy:

Pizza,

6 ovens (small one, can take only one pizza at a time), each pizza takes about 6 minutes to bake. 

- Prepare the dough     - 1 minute
- Add Sauce & Toppings  - 1 minute
- Bakes the pizza       - 10 mins

Thats about 13 minutes to finish one pizza.
Lets say its early in the day and only one customer shows up. She would have to wait 13 mins to get her pizza.
A little later a two more customers walks in and he is placing his order. A third cusomter is waitingin the queue. He definitley would not like to wait for 13 mins. before placing his order. 
That would be bad for business.
One way to make this process smoother, is to take and prepare the next order when one pizza is baking. 
This way multiple customers can be processed at the same time. 
This is concurrency, we are still baking one pizza at a time, but by performing other tasks we when wait, we are making things more a little bit faster. 

Next, lets say, the word spread around and business is really good. We have added a new chefs, but the oven is still a problem.

Now the problem is obovous and infacty silly, "use the other ovens"
simple Jeff now uses multiple ovens so that mulitple pizzas can bake at the same time.
If used effectively, we can 

Jeff the chef is the load balancer.

What if you install 50 ovens? Jeff will still be the road block.

Few things to note here :
- Idle resources are not good.
- Requires more work. & Co-ordination and synchrnoization.
- There is a limit.. on how much we can parallelize (Amhdals law) 

https://developer.apple.com/videos/play/wwdc2017/706/?time=175
Parallelism : Simultaneous execution of closely related computations. (Across different cores) - Requires multiple cores.
Concurrency : Composition of independently executed tasks. - Can even be done on a single core. 

- ANother example 

For eg: Lets look at the example of an airport check in counter.
There are 8 counters. One Load balancer. ques of passengers.

When a passegner reach a counter. 
- They id is checked. 
- Boarding passes are printed. 
- Luggage is weighed & tagged. 

If it takes 2 mins to complete the three steps and if we have 100 passengers, it should take 200 min to clear the queue.

There a few ways we can speed this up.
For each passenger what if we process more than one counater a time. 
say, when the luggage of one passgeinger is being processed, we call the next passenger to check thier ID.
Here staff is concurrenlty procesisng more than one customer. An importnat point to note is that the one passenger is being attneed to at any given instant.

One a ocomputer 

Parallelism: Is where we have mulitple counters open, say 8. If the load balancer keeps sending passeger to one counter when others are sittle idle is not good.
Parallemsim, the cpu is not shared instaed mulitple passengers are processed simultanesulsy!

In theory we should get nice speed-up. should be around 8 times.. so our procesisng time clear the quue should drop to 24 mins
Amhadals law goes here?
https://en.wikipedia.org/wiki/Amdahl%27s_law


Imagine the loadbalancer keeps


Thinks of a your bank teller, no matter how fast a single teller is.. 
Teller for eg: can service more than one customer at the same time. (Prashanth , instead of teller, can we change this to a cook)
When one guys order is cooking it can get the order ready for another customer.
Parallelism : When multiple burners, and cooking more than one dish at the at the same time.

Teller 1 : Receives the requests -> Counts the money -> User Verifies the money.
Teller 2 : Sitting idle.

<<_Animation can go here :_>>

in short, take a large problem and break it down into smaller pieces.. and maybe join them together.


### Sample code here

Lets look at an example, say we have an app that dispalys photos with a different types of filters.
We start with a bunch of images that we want apply a mono-chreom filter to.

```swift
    //Sample code : Regular for-loop that pops a photo can converts it.
```

Output : Show the total time. This is our bench mark.

Problems :
    Hardware is not fully utilized.
    We are runnning on the main thread, means app wont be repsonsive at these times.
    CPU consumption - show it can go > 100%

Running a for-loop, show how this loop runs on the main thread.
Show which CPU core is used, and how the other threads are sitting idle.

Lets look at how to fix this.

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

https://en.wikipedia.org/wiki/Reentrancy_(computing)
Reentrancy 

problems to addres :
https://en.wikipedia.org/wiki/Synchronization_(computer_science)

https://en.wikipedia.org/wiki/Amdahl%27s_law#Relation_to_the_law_of_diminishing_returns
Amdahl's law

## Privatization

-Wikipedia https://en.wikipedia.org/wiki/Privatization_(computer_programming)
Dependencies arise when two or more reading or writing a variable at the same time.
Privitization gives each thread its own copy. This can be written to simultaneusly.. and there fore parallely. 

Read/Write Conflicting variables introduce dependencies between different execution threads and hence prevent the automatic parallelization of the program. 
The two major techniques used to remove these dependencies are 
    Privatization - Privatization gives each thread a private copy, so it can read and write it independently and thus simultaneously. 
    Reduction : Each thread is provided with a copy of the R/W Conflicting variable to operate on it and produce a partial result, which is then combined with other threads' copies to produce a global result.[1]

First try reduction & then privatization. 

Reduction : Increases Computational overhead.
Privatization is a space-time trade-off. : Increases the memory consumed by the program.
Mutual Exclusion (sync) : Helps reduces memory 
There is this other thing called expansion.

Mutual exclusion is another technieque.. here we dont have the space time trade off.. but its slower.


## Synchroniziation
Mutual Exclusion : https://en.wikipedia.org/wiki/Mutual_exclusion
https://en.wikipedia.org/wiki/Lock_(computer_science)

- Serial Queues
- Semaphore




## How do you decide the number of threads.


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


References : 
https://www.hackingwithswift.com/example-code/media/how-to-desaturate-an-image-to-make-it-black-and-white

