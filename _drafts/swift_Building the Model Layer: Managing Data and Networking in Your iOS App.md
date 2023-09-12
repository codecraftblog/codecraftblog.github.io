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
- Overview of the Model Layer
- Importance of Separating Data and Networking Logic

<!-- Include a picture or table, that shows MVVM compontnes.. and rate factors such as resuablelity etc. For eg: Model Layer is most resusable. and testingable.. etc.  -->
## Understanding the Model Layer
### What is the Model Layer?

In the MVVM design pattern, the Model layer represents the data and business logic of an application. It is responsible for managing and manipulating the data used in the application and contains the core functionalities related to data handling and processing. 

<!--TODO: Include an image here showing the overall MVVM and hilight the Model layer only. -->

The Model layer typically includes the following components:
Data Models, Data Services/Repositories,Business Logic


1. Data Models: These are the objects that represent the structured data used in the application. Data models define properties and behaviors to encapsulate and organize the data. They can also include methods for data manipulation, validation, and transformation.

2. Data Services/Repositories: These components handle the interaction with the data sources, such as APIs, databases, or local storage. They encapsulate operations to fetch, save, update, and delete data from the underlying data sources. Data services abstract away the complexities of the underlying data storage and provide a interface for accessing the data.

3. Business Logic: The Model layer often contains the business logic that defines the rules, algorithms, and operations associated with the application's domain. This logic determines how the data is processed, validated, and transformed to support the application's functionality.

By separating the data and business logic into the Model layer, the MVVM pattern promotes a clear separation of concerns, maintainability, and testability. The Model layer acts as an independent and self-contained component that can be easily tested and reused. It is decoupled from the user interface (View) and presentation logic (ViewModel), allowing for easier modifications and updates without impacting the other layers of the application.

In the context of MVVM, the Model layer serves as the single source of truth for the application's data. It handles data fetching, processing, and manipulation, ensuring data consistency and integrity. The ViewModel layer interacts with the Model layer to retrieve the required data and expose it to the View for presentation.

Overall, the Model layer plays a vital role in the MVVM pattern by providing a structured and maintainable approach to managing the application's data and encapsulating the business logic. It supports data management, retrieval, manipulation, and validation, making it a critical component for building robust and scalable applications.

### Responsibilities of the Model Layer

- Representing the structured data used in the application.
- Encapsulating the data and business logic of the application.
- Defining data models or entities that represent the domain-specific objects.
- Managing data fetching, processing, and manipulation.
- Handling data validation and ensuring data integrity.
- Interacting with data sources (such as APIs or databases) through data services/repositories.
- Implementing business rules, algorithms, and operations related to the application's domain.
- Providing a single source of truth for the application's data.
- Supporting data transformation, formatting, and serialization/deserialization.
- Ensuring data consistency across different parts of the application.
- Assisting in maintaining data state and handling data updates.
- Enabling proper testing of data-related functionalities.
- Promoting reusability and interoperability of the data and business logic components.
- Being decoupled from the view (UI) and presentation logic (ViewModel) layers for better maintainability and modifiability.

These responsibilities collectively make the Model layer a crucial part of the MVVM pattern, as it manages and handles the core data-related operations and supports the separation of concerns within the application.



### Benefits of a Well-Designed Model Layer
### Why do we need Model Layer? What's the benefit?

A well-designed Model layer several benefits, including:
The key benefits are 
1. Separation of Concerns: 
By keeping the data and business logic from the user interface and presentation logic.
This separation allows for better code organization and maintainability, as each layer has its own specific responsibilities and can be modified independently.
Single Source of trugh for all data within the app.

2. Testability:
Another benefit of keeping the Modle layer spearte it can  
The separation allows for focused testing of individual components as they can be tested in isolation from the user interface, making it simpler to validate the behavior and integrity of the data.

3. Resueablity : Model layer sperate eg: If you wnat to build a webapp. the mbile app and web app share the same model layer.

5. Maintainability: API changes etc can be handled within the modle layer.
Single Source of trugh for all data within the app.

Overall, a well-designed Model layer in the MVVM pattern helps in achieving code maintainability, testability, reusability, and scalability. It promotes a clear separation of concerns, allowing for focused development and easier maintenance. By centralizing data management and enforcing data consistency, it ensures the integrity of the application's data. Additionally, a well-designed Model layer provides the flexibility to adapt to changes and interoperability with external components, making it a crucial component for building robust and extensible applications.












## Designing the Data Model

    Briefly explain the importance of converting API JSON responses to custom entities.

In a typical app, data comes from an extneral source like an API or DataBase .
The format in which we get data from an extenral source is not expected to match our apps 'view' of the data. also the data structure we use in our app need not come form a single srounce.
i.e we might need to merge data from two or more source to get the data in the desired format.

We could use it directly but that 

Here is where we desing a data Model that is closer to the structure of our app.

So we have two stpes.

    Highlight the benefits of using custom entities (such as structs or classes) for representing data in your app.

Using custom entities, such as structs or classes, for representing data in your app offers several key benefits:

1. Strong Typing: Custom entities provide a strong type system, allowing for more precise and reliable coding. By defining specific properties and their types in your entities, you can catch compile-time errors and ensure that the data conforms to the expected structure. This enhances code robustness and reduces the likelihood of runtime errors.

2. Encapsulation and Abstraction: Custom entities encapsulate the data and behavior related to a specific concept or entity in your app. By grouping related properties and methods together, entities create a clear and distinct representation of the underlying data. This encapsulation promotes abstraction, as entities can hide internal implementation details and expose only necessary properties and methods to the rest of the application.

3. Modularity and Reusability: Custom entities promote modularity and code reusability. By creating self-contained entities, you can use them across different parts of your app or even in other projects. This reduces duplication of code and helps maintain consistency in the data representation. Reusing custom entities eliminates the need to recreate the same structure and logic, improving development efficiency.

4. Data Validation and Integrity: Custom entities allow you to enforce data validation and maintain data integrity. By incorporating validation logic within the entities, you can ensure that the data adheres to specific constraints, rules, or business logic. This helps prevent invalid or inconsistent data from entering your app and ensures the integrity of your data.

5. Enhanced Readability and Understandability: Custom entities improve the readability and understandability of your code. By using entities with expressive and meaningful names for properties, methods, and relationships, you make the code more self-explanatory. It becomes easier for other developers to understand the purpose and usage of the data, improving collaboration and maintainability.

6. Flexibility and Adaptability: Custom entities provide flexibility and adaptability to meet changing requirements. As your app evolves, you can modify or extend the entities to accommodate new data fields, relationships, or behaviors. This scalability allows your app to grow and evolve without impacting other parts of the codebase.

7. Separation of Concerns: Custom entities promote separation of concerns by clearly defining the boundaries between data and business logic. By representing data in dedicated entities, you separate data manipulation and processing from other components in your app, such as view controllers or network services. This separation enhances code maintainability, testability, and makes it easier to track and handle data-related issues.

In conclusion, using custom entities offers benefits like strong typing, encapsulation, modularity, data validation, enhanced readability, flexibility, and separation of concerns. By leveraging custom entities, you can create a more robust, maintainable, and scalable codebase that ensures the integrity and consistency of your app's data throughout its lifecycle.


### Understanding the API Response
In order to define the custom types in our app, its helpful to look at the API response. 

- Discuss the structure of the API JSON response.

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

- Identify the entities or data structures you care about in the response.

### Defining Custom Types in Your Project
   - Explain the process of creating custom entity types (structs or classes) in your project.
   - Discuss the considerations for choosing between structs and classes.

## Designing the Data Model
  A. Defining the Data Structures and Entities
  B. Mapping API Responses to Data Models
  C. Considerations for Data Validation and Transformation

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
   - Explain the process of mapping the JSON response to your custom entity types.
   - Discuss techniques like manual mapping or leveraging Swift's Codable protocol.

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

## Implementing Networking in the Model Layer

Next let make a network call and link the network response to generating our entites.
In iOS we use the URLSession class to make network requests.
Lets first 

Lets first create a Pet in Swagger Editor.
To create a pet login to this and use the editor to crate it.
<!--TODO: Add a section here on how to create a pet in Swagger Editor   -->

<!-- TODO: Link to get and POST request -->
For a complete understanding on how to make a network call lets look at this example.

```swift
    func fetchData(urlString: String) {
        
        // Step 1: Build URL from a string
        guard let targetURL = URL(string: urlString) else { return }
        
        // Step 2: Create a URLSessionDataTask using the `shared` URLSession
        let dataTask = URLSession.shared.dataTask(with: targetURL) { (data, response, error) in
            
            guard error == nil else { return }
            
            print(responseData)
        }
        
        // Step 3: Kick off the Data Download.
        // Note: The URLSessionDataTask created in Step 2, is created in suspended state.
        dataTask.resume()
    }

    // Make network call with URL
```

Add Code sample here on how to make a POST request.
```swift

```


### Creating API Service Classes


### Handling Responses and Error Handling
- Handling Responses and Error Handling
   - Discuss the importance of data validation to ensure the integrity of the API response.
    - Error Handling.
    - Convert to our custom types. The response needs to be converted to ur model format

While it the code above works, it would be make ti more robust to crating a API Service class.

Add code here to show the entire service class. But we can link to another post that explains the Service class in more detail.

```swift
// Add code here that explains how to make a GET and POST request using our PETStore API.

```

### Parsing Data and Converting to Models
Here we show how using the decodeable progocol makes it easy to conver the JSON Data into our apps model.
   - Provide code examples demonstrating how to transform the API JSON response to your custom entities
   - Explain techniques for validating and transforming the data to fit the requirements of your custom entities.


## Testing the Model Layer
- Importance of Testing the Model Layer
- Writing Unit Tests for Networking and Data Processing 

We are now ready to start consuming our data model. Yes we dont have any UI ready but we can write tests and check if our model layer is behaving as expected.

<!-- also make this as a separate post -->
Internal Linked Post : 4. "Swift Data Model Mapping: Transforming API Responses into Data Models"



