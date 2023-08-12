---
layout: post
title:  "Making POST Requests in Swift : A Beginner's Guide"
date:   2023-08-12
categories: iOS Networking Swift 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

When creating a mobile app, it's important to be able to fetch and send data using network calls. Let's explore how to send data to a server using HTTP POST requests in Swift for iOS app development.

POST requests are a way to send data to a server, allowing us to perform actions like submitting forms, creating new resources, or updating existing ones. This beginner's guide will focus on the basics of making POST requests using Swift, helping you add functionality and interactivity to your mobile apps.

To make a POST request in an iOS app, we can utilize URLSession, an Apple-provided class. Here are the steps involved in making a POST request to update some information on a remote server

## Steps to Make a POST Request in Swift
1. **Create a URL object**: Start by creating a URL object that represents the endpoint you want to send the POST request to. This can be a web API or any other server that accepts POST requests.

2. **Create a URLRequest object**: Next, create a URLRequest object using the URL you just created. This object will hold all the necessary information for the request, such as the HTTP method (POST), headers, and body.

3. **Set the HTTP method to POST**: Set the HTTP method of the URLRequest object to \"POST\" using the `httpMethod` property.

4. **Set the request headers**: If your server requires specific headers, you can set them using the `setValue(_:forHTTPHeaderField:)` method of the URLRequest object. Headers are often used to provide authentication tokens or specify the content type of the request.

5. **Set the request body**: To send data with the POST request, you need to set the request body. This is where you can include any parameters or payload you want to send to the server. The body can be in various formats, such as JSON, XML, URL-encoded form data etc.

6. **Create a URLSessionDataTask**: Create a URLSessionDataTask object using the shared URLSession. This object will handle the actual sending of the request and receiving the response from the server.

7. **Trigger the request**: Call the `resume()` method on the URLSessionDataTask object to send the request to the server.

8. **Handle the response**: Once the request is sent, the server will respond with a response object. You can handle the response in the completion handler of the data task. This is where you can parse the response data, check for errors, and update your UI accordingly.

<!--more-->
<br/>

## Example: Using Swagger's PetStore API
To illustrate how to make a POST call, we will use Swagger's PetStore (v3) API, specifically the `/pet/addPet` endpoint. This endpoint allows us to create and add new pets to the store. 

Before we make the POST call to the `/pet/addPet` API, let's first create a payload that describes the pet we want to add. In this example, we will add a new dog to the pet store. The JSON representation of the dog's details would look like this:

```json
{
  "id": 10999999,
  "name": "Snoop Dog",
  "category": {
    "id": 1001,
    "name": "Code Craft Pets"
  },
  "status": "available"
}
```

### Setting Request Headers 
To send this POST request, we need to set up the request headers correctly. For instance, we can specify the format of the payload by using the `"Content-Type"` header and setting it to `"application/json"`. This indicates that we are sending the payload in JSON format.

The PetStore api can also accesspt a XML or url encoded form therefore we need to specify which format we are sending to the API.

Now that we know what the request body and headers should be, we can proceed with the Swift code to make the POST call. Here's an example:

### Setting up the swift code to make the POST call.
```swift
    func postRequest() {
        // Step 1: Create a URL object
        let url = URL(string: "https://petstore3.swagger.io/api/v3/pet")!
        
        // Step 2: Create a URLRequest object
        var urlRequest = URLRequest(url: url)
        
        // Step 3: Set the HTTP method to POST
        urlRequest.httpMethod = "POST"
        
        // Step4: Set the request headers:
        urlRequest.setValue("Application/json", forHTTPHeaderField: "Content-Type")
        
        // Step 5: Set the request body
        // Here we convert the body from a Swift Dictionary to a JSON
               let newPet: [String: Any] = ["id": 109999991,
                                       "name": "Snoop Dog",
                                       "category": ["id": 1001,
                                                    "name":"Code Craft Pets"],
                                       "status": "available"]
        // Serialize the request JSON JSON to Data
        let httpBodyData = try! JSONSerialization.data(withJSONObject: newPet) 

        // Set the request body
        urlRequest.httpBody = httpBodyData
        
        // Step 6: Create a URLSessionDataTask
        let dataTask = URLSession.shared.dataTask(with: urlRequest) { data, response, error in
            
            //Step 8: Handle the response
            
            // Check for Errors
            guard error == nil else { return }
            
            // Check is HTTP response is valid
            guard let httpResponse = response as? HTTPURLResponse,
                  (200...299).contains(httpResponse.statusCode) else {
                print("Invalid HTTP Response Error: \((response as? HTTPURLResponse)!.statusCode)")
                return
            }
            
            // Check if we have a valid response data
            guard let responseData = data else { return }
            
            // Process/Consume the response.
            let json = try! JSONSerialization.jsonObject(with: responseData)
            print(json)
        }
        
        // Step 7: Trigger the request
        dataTask.resume()
    }

```
<br />

This code snippet sets up the URL, configures a URLRequest with a POST method, adds the necessary headers, sets the request body using the prepared JSON payload, and creates a URLSessionDataTask to handle the network request.

When the request is sent, the `dataTask` closure will be executed with the response data, response object, and any errors. In this example, we print the response data as a string for demonstration purposes. You can perform further handling and processing of the response data based on your specific requirements.

### Validation: Did our POST call work? 
To validate if our POST request was successful, you can make a subsequent GET request using the pet ID and check if the pet is now available in the PetStore API. If you not sure how to make GET request, check out this post. <!-- Prashanth Add the GET POST here -->
You can also use the Swagger API Playground to interactively test the API.
https://petstore3.swagger.io/#/pet/getPetById

In conclusion, you have learned the basics of making POST requests in Swift for iOS app development. POST requests are indispensable for sending data to servers and performing various actions. 

Remember to handle the response and customize the code as per your app's specific needs.
<!-- Prashanth Add the POST related to building a full fleged app here -->

<br />
<hr />
References:
- [Swagger PetStore API Documentation](https://petstore3.swagger.io/#/pet/addPet)
- [Apple Developer Documentation](https://developer.apple.com/documentation/foundation/urlsession/)
- [Swift.org](https://swift.org/)
