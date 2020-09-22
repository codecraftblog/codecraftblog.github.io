---
layout: post
title:  "Draft - Automating Mac Applications uings JavaScript"
date:   2020-09-04
categories: Mac Automation JavaScript JXA 
author:
  name: Prashanth 
  twitter: codecraftblog 
  picture: /images/profilePhoto.jpg
---

Automating repetitive tasks on a mac.

Loaded with Apps suing which you can build a complete workflow.
Very often I end up doing the same tasks over and over. For eg: 

The possiblities are endless. and the true power shines through when differnet apps interat with each other.
Your reminders ..talks to your calendars .. talks to your notes.

On a Mac there are two popular languages you can use to write your script.
1. AppleScript - This is language created by Apple for primarily for writing scripts.
2. JavaScript - An extremely popular language used widely to build things on the internet.

Note : There’s also shell script, but that’s more low level and close to the OS.

Most application on your Mac can be controlled using a script. A script is small program that instructs different applications on your Mac to perform some task. For eg : You can ask the ‘Mail’ application to launch and send an email.

The true power of scripting is exposed when you combine multiple simple scripts that make make different applications talk to each other.

In the post we look at how to use JavaScript to automate simple tasks.

<!--more-->

<br>
#### Opening Script Editor
First, we look at what we need to start writing scripts.

Every Macs come installed with app called `Script Editor`. The Script Editor is not  

#### Opening Script Editor
Applications > Utilities > Script Editor
 or 
Search for `Script Editor` using Spotlight

Script Editor allows you to write scripts, excute them and view the results.
It is not a very sophiticalted editor, but its good enough for simple scripts.

![Script Editor](/images/jxa-notes-automation/script_editor_intro.jpg)

By default the script editor expects Scripts to be written in AppleScript. 
Change the language to JavaScript as shown above.

Lets check if evertying is fine.

```javascript
console.log("Hello World")
```
In the 'Accessory View' you should see the string "Hello world" printed.

/* Hello World */

Now that we have the environment working. Lets look at how to write scripts that talk to apps.
We see how to create a new note in Apple Note app.

<br>
## Scripting Apple Notes app

We first get a reference to the Apple notes app. 
We then instruct the notes app to create a new note. 
Finally we add the newly created note to all the other notes.

**Listing 1.1** : *Create a new Note*
```javascript
// Create a new Note in Apple Notes using JavaScript (JXA)

// Get a reference to the notes app
notesApp = Application('Notes')

// Launch Notes app and bring it to the foreground 
noteApp.activate()

// -------------  Create a new note --------------//
// Create a new note object
newNote = notesApp.Note({name : "Hello World!"})

// Add the newNote to the notes app
notesApp.defaultAccount.notes.push(newNote)
```
1. `Application` is a global object <Prashanth > get mroe stuff form jxa dox.
We tell this global 'Applicaiton' object get us a reference to the notes app.

2. Tell the notes app to aactivate.

3. Createe a new note :
    To create a new note we we only need to give the note a name .

4. Add the newly created note to our list of notes.

defaultAccount : You can have multiple accounts in our note. Here we ask the note to push it to the defaultAccount.

Final Results :
In your notes app, you shoud see a new note with the named `Hello World!`  

This script a baby step, but using the same process we can do almost anything you do with you app you should be able to script it.

In this post we take a deeper look on how to script apple notes. <lInk to apple notes post here.>

![Final Note Created](/images/jxa-notes-automation/notes_created_final_result.jpg)

<br>
## Automating Safari browser using JavaScript

This script will open a new Safari window and open a few tabs. Each tab will open a website I visit daily.

<hr />

## Automating Reminders app

Create a new remidner using JXA.

List of all remidners due today.a new remidner using JXA.

<hr>

Create a new email using JXA.

Send an email every monday?


Mail 
    
Notion 
 Notion
 Evernote

 Reminder Apps :
    Reminders :

Executing keystrokes

Writing our first Script.
Which Apps can be scripted?

Whats next ?
    No coding : Checking out how to use Automator do the same tasks.



