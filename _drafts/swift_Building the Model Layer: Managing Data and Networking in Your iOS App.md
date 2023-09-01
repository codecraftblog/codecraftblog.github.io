---
layout: post
title: "Building the Model Layer" 
date: 2022-06-20 22:45:00 +0530
categories: iOS Swift MVVM Architecture
author:
    name: Prashanth
    twitter: codecraftblog
---

## Introduction
  A. Overview of the Model Layer
  B. Importance of Separating Data and Networking Logic

## Understanding the Model Layer
  A. What is the Model Layer?
  B. Responsibilities of the Model Layer
  C. Benefits of a Well-Designed Model Layer

## Designing the Data Model
  A. Defining the Data Structures and Entities
  B. Mapping API Responses to Data Models
  C. Considerations for Data Validation and Transformation

## Implementing Networking in the Model Layer
  A. Introducing URLSession and Network Requests
  B. Creating API Service Classes
  C. Handling Responses and Error Handling
  D. Parsing Data and Converting to Models

## Managing Local Data Storage
  A. Choosing the Right Storage Mechanism (e.g., CoreData, Realm, UserDefaults)
  B. Integrating Local Data Storage with the Model Layer
  C. Caching and Offline Support Considerations

## Testing the Model Layer
  A. Importance of Testing the Model Layer
  B. Writing Unit Tests for Networking and Data Processing 


## Designing the Data Model

1. Introduction
   - Briefly explain the importance of converting API JSON responses to custom entities.
   - Highlight the benefits of using custom entities (such as structs or classes) for representing data in your app.

2. Understanding the API Response
   - Discuss the structure of the API JSON response.
   - Identify the entities or data structures you care about in the response.

3. Defining Custom Types in Your Project
   - Explain the process of creating custom entity types (structs or classes) in your project.
   - Discuss the considerations for choosing between structs and classes.

4. Mapping API Responses to Data Models
   - Explain the process of mapping the JSON response to your custom entity types.
   - Discuss techniques like manual mapping or leveraging Swift's Codable protocol.

5. Data Validation and Transformation
   - Discuss the importance of data validation to ensure the integrity of the API response.
   - Explain techniques for validating and transforming the data to fit the requirements of your custom entities.

6. Building the Transformation Logic
   - Provide code examples demonstrating how to transform the API JSON response to your custom entities


### Understanding the API Response
### Defining Custom Types in Your Project
### Mapping API Responses to Data Models
### Data Validation and Transformation


### Understanding the API Response

In this post we will us Swagger Pet Store API (v3). https://petstore3.swagger.io
The Swagger Pet Store API v3 is a RESTful web service that provides various endpoints for managing and interacting with pet-related resources.

We will work with two endpoints.
<!-- Prashanth add the Table here -->

POST /pet               - Add a new pet to the store
GET  /pet/findByTags    - Finds pets by tags

Both of these endpoints deal with a Pet entity. 

Sample response JSON representing a PET 

```json
    {
      "id": 101,
      "name": "doggie",
      "status": "available",
      "category": {
        "id": 1,
        "name": "Dogs"
      },
      "photoUrls": [
        "photoURL"
      ],
      "tags": [
        {
          "id": 0,
          "name": "string"
        }
      ]
    }
```

- "id" : Integer representing a unique identifier for the pet.
- "name" : String that holds the name of the pet.
- "status" : String that represents the current availability status of the pet. 
    - It is a string with possible values like "available", "pending", or "sold". 
- "category" : Object that contains the "id" and "name" properties. It represents the category or type of the pet, with "id" being the category's unique identifier and "name" denoting the category's name (in this case, "Dogs").
- "photoUrls" :  Array containing string values. It represents the URLs of photos associated with the pet.
- "tags" : Array of objects, each representing a tag associated with the pet. Each tag object has properties "id" (a unique identifier) and "name" (the name or description of the tag).


Now that we have lets create entities to use in our app.

### Defining Custom Types in Your Project

As Since the Pet entity is 
Why do we need custom types.
We have defined the Pet entity and some supporting types the `Tag` Struct and enum `Status`

```swift 
struct Pet {
    let id: UInt32 = UInt32.random(in: 11111...UInt32.max)
    var name: String
    var status: Status
    var tags: [Tag] = [Tag(id: Int32.random(in: 99...999), name: "CCD")]
    var photoUrls: [String] = []
}

struct Tag {
    let id: Int32
    let name: String
}

enum Status: String {
    case available
    case pending
    case sold
}
```

Description of the code above
- The Pet struct represents a pet entity in our app.
- Each pet has a unique identifier called "id" that is automatically generated and cannot be changed.
- Pets have a name, which is a string that identifies them.
- The "status" property represents the current status of the pet. It can be one of several predefined values.
- Pets can have tags associated with them. Tags provide additional information or categorization for the pet. The tags property is an array of Tag objects.
<!-- Prashanth Modify this --> to explain why use CCD Tag. or should it be CCB tag.
- Each Tag object has its own unique identifier called "id", which is randomly generated when the pet is created.
- The name property of a Tag object represents the name or description of the tag.
- Pets can also have one or more photo URLs associated with them. The photoUrls property is an array of strings that store the URLs of the pet's photos.


### Mapping API Responses to Data Models

Next we need a way to convert the API response to our custom types.
We will soon be making a network call to the PetStore API and will expect to use the API response in our app.

THere are two possible apporaches. 
- Parse the JSON using Swift's JSONSerialization or a JSON decoding library (eg: SwiftyJSON). 
- Use Swift's Encodable and Decodeable (Codeable) protocol. 
    - https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_typesjk  

In this post we foucs on making our custom types conform to the codeable protocol. If you are interested on how to parse a JSON using Swift's JSONSerialization or using a JSON decoding library. Look that this post. 

<!-- 
TODO: Link to post here.
how to parse a JSON using Swift's JSONSerialization or using a JSON decoding library
-->

Converting/Mapping the JSON response to other types essentially involves 3 steps:
1. Make our custom types conform to the Ecodeable or Decodabel protocol.
2. use the try! JSONSerialization to encode to custom types.

<!--TODO: Add a box or quote here, saying if you only plan to read use decdoeable, only to write codeable and to do both Codeable. -->

Lets look at a code sample. and lets first focus on converting the extenral JSON representation to our custom datatype
Pet.

<!-- from Apple site Rephares this. -->
The simplest way to make a type codable is to declare its properties using types that are already Codable. These types include standard library types like String, Int, and Double; and Foundation types like Date, Data, and URL. Any type whose properties are codable automatically conforms to Codable just by declaring that conformance.

Lets decare the 

In order for a custom type to conform to the 

```swift
struct Person {
    var name: String,
    var age: UInt
}

```
To make this conform to codable we just add the coformace. Since both the properties are of codeable, Person is also codeable.  
Here we create an exteion on Person and delcate that Person conforms to Codeable.

```swift
    extension Person: Codeable { }
```

Switching to the Pet type, while the properties like name etc confrom. We also our custom types Tag and Status.
We simply need to make Tag and Status too compatible.

```swift
    extension Tag: Codeable { }
```

```swift
    extension Status: Codeable { }
```

<!-- TODO: Here the names between JSON response and TYpes match. Add a note "burb box" here that such scenarios are possible and that they need to refer to my other post on coding keys -->


<!-- TODO: Here link to a more detailed post on why we would to create custom entities. -->
Now that we have lets create entities to use in our app.

There are several benefits to creating a custom entities. 
1. Strong Typing
2. Data Abstraction
3. Modularity and Reusability
4. Encapsulation of Logic
5. refere to the OpenAI note on this 
<!--TODO: Refer to note to expand this section : prash_workspace/note_why_custom_entities.md -->

The main entity is a Pet. 
We inlcude a custom types to store Tags and enum to store Status.

Becuase PET is a struct, Swift is smart enought to undertand  that we need and init init(name: String, status: Status)

### Making a network call 

Next let make a network call and link the network response to the end entity.

For a complete understanding on how to make a network call lets look at this example.

```swift
    func fetchPetByStatus(status: Status) {

    }
```


### Data Validation and Transformation



<!-- also make this as a separate post -->
Internal Linked Post : 4. "Swift Data Model Mapping: Transforming API Responses into Data Models"



