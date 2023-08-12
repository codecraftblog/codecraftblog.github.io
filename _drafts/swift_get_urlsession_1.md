---
layout: post
title:  "Making a Network Call in Swift iOS"
date:   2023-08-08
categories: iOS Networking 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

If you're building a mobile app, chances are you'll need to make network calls to fetch data from a server or send data to a backend API. In this blog post, we'll explore how to make a HTTP GET network call in an iOS app using Swift.

To make a HTTP GET network call in Swift, we can use the `URLSession` class provided by Apple. 

Here are the steps a simple example of how to make a GET request to fetch data from a server:
1. **Create a URL**: *"https://openlibrary.org/works/OL21177W.json"*  
2. **Create a URLSessionDataTask**: Use URLSession's `dataTask(with:)` method
3. **Trigger the Data Fetch** : Kickstart the URLSessionDataTask to fetch the data
4. **Process the response** : Check returned resopnse for errors & process the response

<!--more-->
<br/>
```swift
    func fetchData(urlString: String) {
        
        // Step 1: Build URL from a string
        guard let targetURL = URL(string: urlString) else { return }
        
        // Step 2: Create a URLSessionDataTask using the `shared` URLSession
        let dataTask = URLSession.shared.dataTask(with: targetURL) { (data, response, error) in
            // Step 4:  Process the received response
            
            // Step 4.1: Check for Errors
            guard error == nil else { return }
            
            // Step 4.2: Check is HTTP response is valid
            guard let httpResponse = response as? HTTPURLResponse,
                  (200...299).contains(httpResponse.statusCode) else { return }
            
            // Step 4.3: Check if we have a valid response
            guard let responseData = data else { return }
            
            // Step 4.4: Read and Consume the Response.
            print(responseData)
        }
        
        // Step 3: Kick off the Data Download.
        // Note: The URLSessionDataTask created in Step 2, is created in suspended state.
        dataTask.resume()
    }
```
<br />

A natural next step is to parse and use the response from the server in our app. 
check out this post if you looking how to parse a JSON.

The code sample above uses the `URLSession` `dataTask(with:completionHandler:)` method. URLSession also supports other methods to fetch data. 

If you are looking for *"How to make a HTTP GET call using the async/await syntax?"*, check out this post.

To use the URLSession along with Apple's Combine framework. Check our this post.

A natural next step is to parse and use the response from the server in our app. 
check out this post if you looking how to parse a JSON.

### References 

[Apple Developer Documentation](https://developer.apple.com/documentation/foundation/urlsession/) 

### Step-by-Step Instructions

In the post aove 


Step 1 : Introduce the URL.
// We have a URL and want to fetch its contents.  
// What is the URL we are trying to fetch?
// Create a URL in iOS Swift. 

How to fetch the contensts :
1. URLDataTask ?
2. Session.data
3. Combine

Step 2 : Create a Data Task.
/// Data Task
/// For small interactions with remote servers, you can use the URLSessionDataTask class to receive response data into memory (as
//A data task is ideal for uses like calling a web service endpoint.
// You use a URL session instance to create the task. If your needs are fairly simple, you can use the shared instance of the URLSession class.
//Sessions can be reused to create multiple tasks, so for each unique configuration you need, create a session and store it as a property.
// Once you have a session, you create a data task with one of the dataTask() methods. Tasks are created in a suspended state, and can be started by calling resume().







1. Create a URL object with the URL of the server endpoint you want to make a request to. In the example above, we used \"https://api.example.com/data\" as the URL.

2. Create a URLSessionDataTask object using the shared URLSession instance. This object represents the task that will perform the network request.

3. Inside the data task's completion handler, you can handle the response, data, and error returned by the server. In the example above, we simply print the error if there is one, and process the data if it exists.

4. Call the `resume()` method on the data task to start the network request.

### In-Depth Explanation

The URLSession class in Swift provides an easy way to make network requests. It handles the low-level details of establishing a connection, sending the request, and receiving the response.

In the example above, we used the `shared` instance of URLSession, which is a singleton that you can use throughout your app. You can also create your own URLSession instance if you need more control over the configuration.

The `dataTask(with:completionHandler:)` method of URLSession is used to create a data task that performs a GET request. Inside the completion handler, you can handle the response, data, and error returned by the server.

If there is an error, you can handle it accordingly. In the example above, we simply print the error message using `localizedDescription`.

If there is data returned by the server, you can process it as needed. This could involve parsing JSON, decoding data into custom objects, or saving the data to disk.

Once you've set up the data task, you need to call `resume()` to start the network request. This tells URLSession to begin the request and call the completion handler when it's done.

### Conclusion

Making network calls is a fundamental part of mobile app development. In this blog post, we explored how to make a network call in Swift for iOS development using URLSession. We provided a simple example and step-by-step instructions to help you get started.

Remember to handle errors and process the data returned by the server appropriately in your own app. You can also explore more advanced features of URLSession, such as making POST requests or handling background downloads.

If you want to learn more about networking in Swift, check out the [official Apple documentation on URLSession](https://developer.apple.com/documentation/foundation/urlsession) or the [Alamofire](https://github.com/Alamofire/Alamofire) library, which provides a more convenient and powerful API for networking in Swift.

Feel free to leave a comment if you have any further questions or suggestions for topics you'd like us to cover in future blog posts. Happy coding!

### Further Reading

- [URLSession - Apple Developer Documentation](https://developer.apple.com/documentation/foundation/urlsession)
- [Alamofire - Elegant HTTP Networking in Swift](https://github.com/Alamofire/Alamofire)
