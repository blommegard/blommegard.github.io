---
layout: post
title: "#include \"tgmath.h\""
date: 2013-11-19 16:10
comments: false
categories: arm64 iOS 
---

There are a lot of things to think about when it comes to the new architecture that apple calls arm64. Apple got a [great transition guide](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaTouch64BitGuide/Introduction/Introduction.html) including the [size and alignment tables](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaTouch64BitGuide/Major64-BitChanges/Major64-BitChanges.html) for the different datatypes. This is a biggie, and you should be very carefull when going forward and code for both archirectures. One datatype I use a lot in my UI code is `CGFloat`, and I hope you do as well. :)

Until now I have always used `roundf()` and the other "f-functions" to round these values off. Now, however, when there is different underlying types we have to do this another way, a correct way. While reading the [64-Bit Transition Guide for Cocoa](https://developer.apple.com/library/mac/documentation/cocoa/conceptual/Cocoa64BitGuide/Introduction/Introduction.html) (note, not the same as the Cocoa Touch above) I found out about the tgmath-header.

`tgmath.h` includes a bunch of very usefull macros to make it easier working with different datatypes on different architectures. So, go ahead and `#include "tgmath.h""` and just use the `round()` macro instead! Naturally, it includes the other common math-related operations as well.
