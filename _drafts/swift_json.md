---
 layout: post
 title:  "Draft - How to handle JSON in Swift"
 date:   2020-06-28 21:16:00 +0530
 categories: iOS 
 author:
 name: Prashanth 
 twitter: johndoetwitter
 picture: /images/profilePhoto.jpg
---

Possible titles 
- JSON Basics: Everything You Need to Know
- What is JSON and Why Should You Care?
- The Beginner's Guide to JSON
- JSON Basics: Everything You Need to Know
- How to Use JSON in JavaScript
- Working with JSON in Other Programming Languages
- The Best JSON Tools and Libraries
- Common JSON Mistakes and How to Avoid Them
- The Future of JSON
- 10 Tips for Writing Effective JSON
- The Ultimate Guide to JSON

### What is JSON.

JSON, or JavaScript Object Notation, is a lightweight data-interchange format.
- Text based format that is language independent. thefore can be used by any programming laguage that can work with text.
- Easy for humans to read and edit
- Widely used in Web and mobile applications.
- Popular Data interchange format

JSON defines a small set of structuring rules for the portable representation of structured data.

## JSON Keys
Must be double quotd strings

https://www.ecma-international.org/wp-content/uploads/ECMA-404_2nd_edition_december_2017.pdf
> ECMA-404 - Definition:
> JSON is a lightweight, text-based, language-independent syntax for defining data interchange formats. It was derived from the ECMAScript programming language, but is programming language independent. JSON defines a small set of structuring rules for the portable representation of structured data.


## JSON Numbers
A number is a sequence of decimal digits with no superfluous leading zero.
<Prash> Tidbit about JSON, you cannot have {"hello":04}, this is not a valid JSON.
{"hello":-.4}  //Not valid JSON
{"hello":-0.4}  // Valid JSON

A number is a sequence of decimal digits with no superfluous leading zero. 
It may have a preceding minus sign (U+002D). 
It may have a fractional part prefixed by a decimal point (U+002E). 
It may have an exponent, prefixed by e (U+0065) or E (U+0045) and optionally + (U+002B) or – (U+002D). 
The digits are the code points U+0030 through U+0039.


Escaped with U+005C (reverse Solidus)
<titbit> its not back slash

NaN and Infinity are unsupported.
Spaces, commas etc are not allowed within JSON.

Sturcture of JSON.
JSON data is stored in objects. An object is a collection of key-value pairs.

### Simple JSON

Simple JSON representing a user 
```javascript
{
    "firstName": "John"
}
```
In the JSON above the key is the string "firstName" and value is also a string "John"

Keys are strings and Objects can be any of the following types.

The following types are allowed as JSON values:

Primitive types :
Strings
Numbers
Booleans
Null

Two Structured Types :
Objects
Arrays

> An object is an unordered collection of zero or more name/value pairs, where a name is a string and a value is a string, number, boolean, null, object, or array.

> An array is an ordered sequence of zero or more values.

Strings are enclosed in double quotes. Numbers can be integers or floating-point numbers. Booleans can be either true or false. Null is a special value that represents the absence of a value. Objects are collections of key-value pairs. Arrays are collections of values.

JSON Strings escape sequence. 

### JSON Keys

Here are some restrictions on JSON keys:

Keys must be strings.
Keys must be enclosed in double quotes.
Keys must be unique. - Wrong.. but it will take the last value. (you can amke a short post on this. to show what happens when JSON has duplicate keys.)
https://www.rfc-editor.org/rfc/rfc8259#section-4

- Keys must not start with a number. (WRONG)
- Keys must not contain any special characters, such as spaces, commas, or colons.  (Wrong)

Here are some examples of valid JSON keys:
"name"
"age"
"is_active"
"address"
"numbers"

Here are some examples of invalid JSON keys:
123
name,age
is active
address line 1
numbers[0]




JSON data is stored in objects. An object is a collection of key-value pairs. The key is a string, and the value can be any type of data, including strings, numbers, objects, arrays, or booleans.


It is easy for humans to read and write. It is easy for machines to parse and generate. It is a text-based format that is completely language independent. JSON is a common data format with diverse uses in electronic data interchange, including that of web applications with servers.

JSON is a text format that is used to store and transmit data. It is a lightweight format, which means that it is easy to read and write. JSON is also a language-independent format, which means that it can be used by any programming language.

JSON data is stored in objects. An object is a collection of key-value pairs. The key is a string, and the value can be any type of data, including strings, numbers, objects, arrays, or booleans.

JSON arrays are used to store lists of data. An array is a collection of values. The values can be any type of data, including strings, numbers, objects, or arrays.

Sample JSON.


### Introduce a basic JSON

JSON (JavaScript Object Notation
uses human-readable text to store and transmit data objects consisting of attribute–value pairs and arrays (or other serializable values)

> In computing, serialization (or serialisation) is the process of translating a data structure or object state into a format that can be stored (e.g. files in secondary storage devices, data buffers in primary storage devices) or transmitted (e.g. data streams over computer networks) and reconstructed later (possibly in a different computer environment).  --wikipeida

attibute-value pair
Many languages give include code to generate and parse JSON-format Data.

Other types XML, ProtoBuf (???) 
https://www.freecodecamp.org/news/what-is-serialization/

To serialize an object means to convert its state to a byte stream so that the byte stream can be reverted back into a copy of the object.
https://docs.oracle.com/javase/tutorial/jndi/objects/serial.html

### Simple JSON

Simple JSON representing a user 
```javascript
{
    "firstName": "John"
}
```

### JSON can also have other values
basic datatypes include :
- Number
- String
- Boolean
- Aray
- Object (JSON Object) - collection of name-value pairs (key-value)
- null

### Enhanced JSON

```javascript
{
    "firstName": "John",
    "lastName": "Doe",
    "age": 24,
    "isPremiumSubscriber": true,
    "hobbies": ["running", "coding", "music"],
    "stats": {
        "lastLoginTime": "",
        "loginCount": 24
    }
}
```

Cons of JSON:
// JSON does not have support for complex ?? objects like Dates.
Parsing Dates:

Where is JSON used.
List of popular API for  your next project. (Prashanth Another cross posting link)

Flight Data API

Its important to understand that the what you see on API is what the JSON looks like, but what you get is data.
i.e. JSON has been turned into a serirese of bytes, so that it can be sent over to us. 

Steps to consume a JSON.

1. Download the JSON. (Serliazed form)
2. Desserialze it, into a STring.
3. Parse it into a language specific format.
4. Read the JSON.

### Encoding
Explain the term (Generic. Not in the context of JSON)
Encodeing a generic term use to make sense - more like there is a message.. you encode it.
to put information into a form in which it can be stored, and which can only be read using special technology or knowledge
https://dictionary.cambridge.org/dictionary/english/encode

### Decoding
Decode : Something cryptic your decode it.
to discover the meaning of information given in a secret or complicated way:

Explaing the term Decoding (Generic term)
Explain the term Decoding in terms of JSON.
Introduce JSONSerialization

### Encoding
Explain the term (Generic. Not in the context of JSON)
https://stackoverflow.com/questions/3784143/what-is-the-difference-between-serializing-and-encoding

To "serialize" an object means to convert its state into a byte stream in such a way that the byte stream can be converted back into a copy of the object.

https://en.wikipedia.org/wiki/Marshalling_(computer_science)


### Serialization
Explain the term (Generic. Not in the context of JSON)
What does it mean to Serialize something

To "serialize" an object means to convert its state into a byte stream in such a way that the byte stream can be converted back into a copy of the object.

To "serialize" an object or Data Structure is to convert it into a sequence (series) of bytes or characters that can be stored or trasmitted.
The serialized data can then be deserialized into the original object or Daata structure.

> Bard: 
> Serialization is the process of converting an object or data structure into a sequence of bytes or characters that can be stored or transmitted. The serialized data can then be deserialized back into the original object or data structure.

Not so with endoing eg: You might encode an audio signal into a mp3 for eg FLAC to mp3, then you cant get back the same audio quality back from the mp3.

Serialization is the process of converting an object or data structure into a sequence of bytes or characters that can be stored or transmitted. The serialized data can then be deserialized back into the original object or data structure.

JSON Data -> Bytes -> Dictionary -> Parse Dictionary into Custom Types

When encoding or decoding, we save one step. Directly from Data to and from Custom type.


https://en.wikipedia.org/wiki/Marshalling_(computer_science)


### Deserialization
Explain the term (Generic. Not in the context of JSON)
What does it mean to de-Serialize something

Explain how serialzation and codable decodeable is hanlded.

Serialization is the process of converting an object or data structure into a sequence of bytes or characters that can be stored or transmitted. The serialized data can then be deserialized back into the original object or data structure.


Examples of audio coding formats include MP3, AAC, Vorbis, FLAC, and Opus.
Codable (introduded iOS 8)
if an item is Decodable, i.e. We can then directly decode into any format we want. 

### Decoding
Explaing the term Decoding (Generic term)
Explain the term Decoding in terms of JSON.

DONT EVEN TALK ABOUT JSON HERE>. it can be any type.

https://swiftpackageindex.com/apple/swift-protobuf
BSON
YAML
https://medium.com/nsistanbul/data-serialization-formats-available-in-swift-d0dc2971dbdajkkkk

Introduce JSONSerialization
https://developer.apple.com/swift/blog/?id=37

JSONDecoder : An object that decodes instances of a data type from JSON objects.
Image goes here - Show how the decoder converts from one type to another.
<note> In this post we just give a overview. need to go into decoder in depth in another post.

JSONEncoder :



### Encoding
Explain the term (Generic. Not in the context of JSON)



### Parsing a JSON
When we fetch JSOn, we get a serilazed format (prashanth vlaidate this). We need to convert this into a languae specific format.
For eg: In Swift, we will conver thtis to a Struct.
In swift the data model of your appliation is usually a struct.
Here we parse the JSON Data can convert it to a type that can be used in your swift program.
SOme options are :
1. Convert JSON Data to a Dictionary. (JSONSerialization)
2. Using the Encodeable and Decodabel protocols convert it to a struct/class.

#### Converting JSON to dictionary.
Simple but can be error prone need to hanld 

Lets look at simple JSON, three fields, and two types (String & Int)

### Decoding a JSON

Using the JSON Decoder
```swift
struct Person {
    let firstName: String
    let lastName: String
    let age: Int
}

```
How does this work?


Common Errors when decoding JSON.

Decodeable Protocol.

Saving JSON to file

### Parsing a JSON

## Reference
https://www.ecma-international.org/wp-content/uploads/ECMA-404_2nd_edition_december_2017.pdf
