---
layout: post
title:  "Making POST Requests in Swift : A Beginner's Guide"
date:   2023-08-08
categories: iOS Networking Swift 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

When creating a mobile app, it's important to be able to fetch and send data using network calls. Let's explore how to send data to a server using http POST requests in Swift for iOS app development. 

POST requests are a way to send data to a server, allowing us to perform actions like submitting forms, creating new resources, or updating existing ones. This beginner's guide will focus on the basics of making POST requests using Swift, helping you add functionality and interactivity to your mobile apps.

To make POST request in an iOS app, we can utilize `URLSession`, an Apple-provided class. Here's a simple example of the steps involved in making a POST request to update some information on a remote server:

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
To illustrate how to make a POST call, we use Swagger's PetStore (v3) api `https://petstore3.swagger.io/#/pet/addPet`. 
The PetStore sample API provides multiple endpoints to setup a petstore. For eg: We can look up pets that are avaible for adoption, add new pets to the store etc. 
In this we use the POST `/pet` endpoint to create a new pet and add it to the store.   
You can try out this API here `https://petstore3.swagger.io/#/pet/addPet`

### Creating the POST body
Before we make the POST call to the `/pet` api, let us first create a payload that decribes the pet we want to add. 

```json
{
  "id": 10999999,
  "name": "Danny Dog",
  "category": {
    "id": 1001,
    "name": "Dogs"
  },
  "status": "available"
}
```

Lets look at at how to add a new Pet a Dog 
Here's an example of how you can achieve this in Swift:

Let say we want to add a new Dog to our petstore. The JSON that represents this dog is mentioned below.
We pass in an ID. 

### Setting Request Headers 
When we make our POST call along with the body/payload we created above we can also pass in additional headers.

For eg: we can specify the format of the POST body. Using the 'Content-Type" header we can tell the server in what format the payload is. In our case we plan to pass the payload in the JSON format. 
The PetSTore api can also accesspt a XML or url encoded form therefore we need to specify which format we are sending to the API.

We have now have the request body and the headers, we are ready to make the call. 

### Setting up the swift code to make the POST call.
```swift
func postData() {
    // Step 1: Create a URL Object
    let url = URL(string: "https://petstore3.swagger.io/api/v3/pet")!
    
    // Step 2 : Create and configure a URLRequest
    var urlRequest = URLRequest(url: url)
    urlRequest.httpMethod = "POST"
    
    // Step 2.2: Set the request headers
    urlRequest.setValue("Application/json", forHTTPHeaderField: "Content-Type")
    
    // Step 5: Set the request body
    let httpBody: [String: Any] = ["id": -999999, "name": "CodeCraftDogChicken", "category": ["id": 1, "name":"Dogs"], "status": "available"] as [String : Any]
    let httpBodyData = try! JSONSerialization.data(withJSONObject: httpBody)
    
    // Step 2.3: Set the request headers
    urlRequest.httpBody = httpBodyData
    
    // Step 6: Create a URLSessionDataTask
    let dataTask = URLSession.shared.dataTask(with: urlRequest) { data, response, error in
        //Step 8: Handle the response
        print(error?.localizedDescription)
        print("response \(response as! HTTPURLResponse)")
        guard let respData = data else {return}
        let json = try! JSONSerialization.jsonObject(with: respData)
        print(json)
    }
    
    // Step 7: Send the request
    dataTask.resume()
}
```
<br />

### Validating that our POST worked.
Make a GET request with ID and check. If you are not sure how to make a GET request. Check out this blog. 
You can also check using the Swagger API Playground. 

<br />
<hr />
### References 
[Apple Developer Documentation](https://developer.apple.com/documentation/foundation/urlsession/) 

