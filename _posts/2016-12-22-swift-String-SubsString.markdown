---
layout:     post
title:      "swift SubString"
subtitle:   "swift String—>SubString"
date:       2016-12-22 23:00:00
author:     "Haoking"
header-img: "img/post-bg-re-vs-ng2.jpg"
tags:
    - iOS
    - Swift
    - Objective-C
    - String
    - SubString
    - Front-end
    - OSX
---

> [Please indicate the source of forwarding and be a follower of my Github](https://github.com/haoking).



## **Swift SubString**

**swift String—>SubString**

How many people are complaining to Swift on SubString ?

Haha. Absolutely.

When we old iOS developers are using Objective-C, we often pick a substring like this:

```objective-c
NSString *string = @"love我爱你"; // if we want to pick: "ve我"
NSString *stringTemp = [string substringFromIndex:2];
NSString *result = [stringTemp substringToIndex:3];
```

or we can just use "substringWithRange:NSMakeRange":

```objective-c
NSString *string2 = [string substringWithRange:NSMakeRange(2, 3)];
```



As we can see, it is pretty easy.

But come to Swift (Swift3), we cannot use this kind of methods anymore.

This makes us confused.

But today Let us solve it at once!!!!!



Here I will give you 6 kinds of methods to solve it!!! (Oh, my god! Crazy!) 666666666



The first method

We can convert "String" to "NSString", then we use the Methods from "NSString":

```swift
let string = "love我爱你"   // if we want to pick: "ve我"
let ocString = string as NSString
let resultString = ocString.substring(with: NSMakeRange(2, 3))
```



The second method

Convert "String" to "Array", then we use the Methods from "NSString" to pick subArray, and convert back in the end:

```swift
let string = "love我爱你"   // if we want to pick: "ve我"
let stringArray = Array(string.characters)
let resultString = String(stringArray[2..<2+3])
```



The third method

We use "String" methods

string.substring(from:   String.Index) and string.substring(to:   String.Index)

Firstly, we use "string.substring(from:   String.Index) " to pick part of String,

then we use "string.substring(to:   String.Index)" to get the result:

```swift
let string = "love我爱你"   // if we want to pick: "ve我"
let resultString = string.substring(from: string.index(string.startIndex, offsetBy: 2)).substring(to: string.index(string.startIndex, offsetBy: 3))
```



The fourth method

We improve the third method a little bit, we can make it easier:

```swift
let string = "love我爱你"   // if we want to pick: "ve我"
let resultString = string.substring(from: "xx".endIndex).substring(to: "xxx".endIndex)
```



The fifth method

We use "String" methods: "string.substring(with: Range(String.Index))"

We build the "Rang" firstly, then use it:

```swift
let string = "love我爱你"   // if we want to pick: "ve我"
let start = string.index(string.startIndex, offsetBy: 2)
let end = string.index(string.startIndex, offsetBy: 2+3)
let range = start..<end
let resultString = string.substring(with: range)
```



The sixth method

We don not use method come from "SubString Class", we directly use the methods from "String Class":

```swift
let string = "love我爱你"   // if we want to pick: "ve我"
let resultString = string[string.index(string.startIndex, offsetBy: 2)..<string.index(string.startIndex, offsetBy: 2 + 3)]
```



Come to the end.

Hope you like these methods to pick substring, and I believe it is enough.



If you like my blog, please :

[Please indicate the source of forwarding and be a follower of my Github](https://github.com/haoking).



By the way, if you have any questions or there are something wrong here, please contect me on Email:

haokinus@gmail.com 


