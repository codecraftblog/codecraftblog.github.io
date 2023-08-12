---
layout: post
title:  "Making GET Requests in Swift : A Beginner's Guide"
date:   2023-08-08
categories: iOS Networking Swift 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

When developing a mobile app, network calls to fetch or send data are often necessary. In this blog post, we'll explore using Swift to make HTTP GET calls in an iOS app.

To accomplish this, we can utilize `URLSession`, an Apple-provided class. Here's a simple example of the steps involved in making a GET request to fetch data:

1. **Create a URL**: "https://openlibrary.org/works/OL21177W.json"
2. **Create a `URLSessionDataTask`**: Use `URLSession` `dataTask(with:completionHandler)` method.
3. **Trigger the Data Fetch**: Start the `URLSessionDataTask` to retrieve the data.
4. **Process the response**: Afer the response is received. Check for errors and handle the response appropriately.

<!--more-->
<br/>
```swift
    func fetchData(urlString: String) {
        
        // Step 1: Build URL from a string
        guard let targetURL = URL(string: urlString) else { return }
        
        // Step 2: Create a URLSessionDataTask using the `shared` URLSession
        let dataTask = URLSession.shared.dataTask(with: targetURL) { (data, response, error) in
            
            // Step 4:  Process the response from the network call
            // Step 4.1: Check for errors
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

A natural next step is to parse and utilize the server response in our app. 
If you're interested in how to consume the server response in your app, I recommend checking out this post.

The code sample above demonstrates the use of URLSession's `dataTask(with:completionHandler:)` method. URLSession offers various other methods for fetching data.

If you're curious about making a HTTP GET call using the async/await syntax, I suggest reading this post.

Additionally, if you'd like to explore using URLSession alongside Apple's Combine framework, take a look at this post.
<br />
<hr />
### References 
[Apple Developer Documentation](https://developer.apple.com/documentation/foundation/urlsession/) 

