---
layout: post
title:  "Draft - Concurrency Swift" 
date:   2021-06-03 21:16:00 +0530
categories: iOS 
author:
  name: Prashanth 
  twitter: johndoetwitter
  picture: /images/profilePhoto.jpg
---

# Swift Concurrency

Remember Async/sync and Concurrent/Serial and different things.

Dispatch Queues are FIFO.
    i.e The tasks are started in the order in which they are added.

## Creating Dispatch Queues
What type of queue do you want?
How do you intend to use it?

Types of queues you an create
- Serial 
- Concurrent

Get one of the global concurrent Dispatch queues.
- Will dequeue tasks (for execution) in a FIFO manner.
- But can dequeue other tasks, when one is still running.
- Number of tasks executed concurrently varies.. System conditions (# of cores, other workload, number of tasks in other queues, priority of tasks etc.)
- 

- Serial Dispatch queues are used when you want tasks to execute in a particular order.
- this can be used instead of a lock on a shared resource or a mutable datastructure.

Unlike concurrent queues, which are created for you, you must explicitly create and manage any serial queues you want to use.

Dont create many queue to execute tasks in parallel, for that use a concurrent queue.

All Dispatch queus, allow you to associate custom objects with the queue.

## Performing Loop Iterations Concurrently
Swift 
```swift
DispatchQueue.concurrentPerform(iterations: 2) {
    print("\($0). concurrentPerform")
}
```


Important
Attempting to synchronously execute a work item on the main queue results in deadlock.
