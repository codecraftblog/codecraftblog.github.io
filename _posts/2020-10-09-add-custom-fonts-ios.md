---
layout: post
title: How to add custom fonts to iOS App
categories:
  - iOS 
---

Applications often use custom fonts to add a touch of personalization and use a font that better represents the character of the app.
For eg: a game might use a more "playful" font - perhaps at the cost of legibility - while a news app might use a more "formal" - a more legible font.

There might be other factors that require you to use a custom font such as adhering corporate brand standards, maintaining consistency across platforms (Android, web, iOS, etc.)

In this post, we look at **How to add a custom font to your Xcode project or iOS app?**. 
We also give a few important factors that you should consider before using a custom font.

### Steps to add custom fonts to an iOS project

<!-- toc -->
1. [Review custom font files](#review-custom-font-files)
2. [Add the custom font to your Xcode project](#add-the-custom-font-to-your-xcode-project)
3. [Update the project info.plist](#update-the-project-info-plist)
4. [Access the custom font](#access-the-custom-font)
<!-- tocstop -->

<!--more-->
<br>

<form name="contact" method="POST" data-netlify="true">
  <p>
    <label>Your Name: <input type="text" name="name" /></label>   
  </p>
  <p>
    <label>Your Email: <input type="email" name="email" /></label>
  </p>
  <p>
    <label>Your Role: <select name="role[]" multiple>
      <option value="leader">Leader</option>
      <option value="follower">Follower</option>
    </select></label>
  </p>
  <p>
    <label>Message: <textarea name="message"></textarea></label>
  </p>
  <p>
    <button type="submit">Send</button>
  </p>
</form>



---
## Review custom font files

In this post, we will add a font named [*Lexie Readable.*](https://www.k-type.com/fonts/lexie-readable/) to an iOS app.

I downloaded the font asset from [here](https://www.k-type.com/fonts/lexie-readable/). The font asset was a folder that contained two variants of the font. 
	- LexieReadable-Bold.ttf
	- LexieReadable-Regular.ttf
 
![Font_Download_Contents](https://github.com/codecraftblog/codecraftblog.github.io/blob/master/images/lexie_readable_font_folder_contents.jpg?raw=true)

Your custom font should also have a similar structure. 

There are many [types of fonts](https://www.fonts.com/support/faq/font-formats), but the most popular types are *True Type Fonts (.ttf)*, *Open Type Fonts (.otf)*. The process described below will work for both .ttf and .otf fonts.

Review your font assets and decide which variant you want to use in your app.

Next, let us add the custom fonts to our Xcode project.

<br>
## Add the custom font to your Xcode project
 
Open the Xcode project or workspace and add the custom font files along with the rest of your project files.

You can either drag the .ttf/.otf files into your project or use the menu *File* > *Add Files to \<your-project-name\>*

In the prompt that follows, select the following options : 
- ☑️ *Copy items if needed*
- ☑️ *Add to targets*

![Add Font Files to Xcode Project](https://github.com/codecraftblog/codecraftblog.github.io/blob/master/images/add_fonts_to_project.jpg?raw=true)

Keep in mind, your font might have multiple files - one for each font variant - add the files associated with each of the font variants you plan to use in your app.

> ⚠️ Make sure you add the font files (.ttf, .otf, etc) and **NOT the entire folder**.

Next, we make the custom font available in our code and Interface builder.

<br>
## Update the project info plist

Open your project's *info.plist* file.

We need add a new key-value pair to the *info.plist*

**Key**: *"Fonts provided by application"* or `UIAppFonts` 
**Value**: Names of the font files. LexieReadable-Bold.ttf

> UIAppFonts : App-specific font files located in the bundle and that the system loads at runtime.

The key `UIAppFonts` or "*Fonts provided by application*" is an array. Add one entry for each of the file we added in the [Add the custom font to your Xcode Project](#add-the-custom-font-to-your-xcode-project) section above.

> ⚠️ Make sure you have the entire file name including the extension (.ttf, .otf).
<br>

![Update Info.plist](https://github.com/codecraftblog/codecraftblog.github.io/blob/master/images/Update_Info_plist.jpg?raw=true)

Alternatively, you can edit the `info.plist`

```xml
<key>UIAppFonts</key>
	<array>
		<string>LexieReadable-Bold.ttf</string>
		<string>LexieReadable-Regular.ttf</string>
	</array>
```

We can now start using the custom font in our project.

<br>
## Access the custom font 

We can access the custom font both in Interface builder and in code.

#### Access the added font in Interface builder

The *Attributes Inspector* tab for any UI element that displays text (UILabel, UITextView, UITextfield) should now list the font we just added.

![](https://github.com/codecraftblog/codecraftblog.github.io/blob/master/images/Access_CustomFont_Storyboard.jpg?raw=true)

>💡 tip : If the font does not show up, try cleaning the Xcode project (Product > Clean Build Folder) or cmd + shift + K*

#### Access the added font in code

To access the custom font in swift code, we need to specify the name of the font we just added.

First, we need to get the **font family name** of the custom font we added to the project.

> ⚠️ The font family name may not be the same of the name of the font file (.ttf/.ott) we added above.

Here are two ways to get the font family name 

- Use the Fontbook app
	- On the Mac, you can install the font by double-clicking the font.
	- Open the 'Font Book' app
	- You can view the `PostScript Name` property to find the name of the font.

- Use temporary code to print all the available fonts
The code below prints a list of all font family names available to your project. Add the code to you any function that gets called in your app. You should see all the font family names printed to the console.
>💡 : Remember, this code is only for temporary use.
```swift
UIFont.familyNames.forEach {
	print("Font Family : \($0)")
	UIFont.fontNames(forFamilyName: $0).forEach { 
		print("\t\($0)")
	}
}
```
Output :
```xml
Font Family : Lexie Readable
		LexieReadable-Regular
		LexieReadable-Bold
```

And.. that's how we use add a custom font to your iOS app.

<br>
## And one more thing, have you considered Accessibility?

Custom fonts are a great way to make your app stand out. However, when you decide to use custom fonts there are some factors we need to think about. 
For instance,
	- Is the font legible at different font sizes?
	- Do the UI elements using your font respect the various iOS accessibility features?

Keep in mind that certain iOS accessibility features - Dynamic Type, Bold Text, etc- enable the user to override the font settings you might have used in your app.

[Dynamic Type](https://developer.apple.com/design/human-interface-guidelines/accessibility/overview/text-size-and-weight/) lets your users choose from a set of pre-defined font sizes. 

The accessibility features built into iOS are helpful when your app is used in less than ideal conditions. 
For eg: Some users might use a larger font size while using their devices in less than ideal conditions (reading on a moving train/bus, reading in poor lighting/bright sunlight, etc.) 

More importantly, certain accessibility features are especially helpful for users with vision impairment.

> According to a [World Health Organisation report](https://www.who.int/blindness/publications/globaldata/en/#:~:text=Globally%20the%20number%20of%20people,are%2082%25%20of%20all%20blind), in the year 2010, over 285 million had some form of visual impairment.

**The default system fonts automatically alter the text properties based on the user's accessibility settings.**

When using custom fonts, it is your responsibility to ensure the custom fonts respect the user's accessibility preference.

This [post](https://developer.apple.com/documentation/uikit/uifont/scaling_fonts_automatically) explains how to make custom font react changes in iOS Accessibility settings.
<!-- Prash link to your post here --> 
