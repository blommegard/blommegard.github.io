---
layout: post
title: "A take on custom transitions with UINavigationController"
date: 2014-01-31 02:29
comments: false
categories: ios iosdev uinavigationcontroller 
---

There exists a lot of great posts about how to create custom view controller transitions for `UINavigarionController` with the new API:s in iOS7, for example this by [objc.io](http://www.objc.io/issue-5/view-controller-transitions.html).
This works fine, most of the time. However, if you want to have the default behavour, [Apple tells us](https://developer.apple.com/library/IOs/documentation/UIKit/Reference/UINavigationControllerDelegate_Protocol/Reference/Reference.html) that you just return nil from the delegate methods `…animationControllerForOperation:…` & `…interactionControllerForAnimationController:…`. This only sort of works. You get the correct animation controller features, but the interactive part is long gone. In my project I want the majority of view controllers pushed the default way (and of course with the interactive part as well) but one with my own transition.

I want the result to feel like the real deal, and to write the pan-logic is not tempring at all, so I started to investigate. If we have a look att the `interactivePopGestureRecognizer` property of `UINavigationController` it holds a target of the class [`_UINavigationInteractiveTransition`](https://github.com/nst/iOS-Runtime-Headers/blob/master/Frameworks/UIKit.framework/_UINavigationInteractiveTransition.h), wich is a subclass of [`_UINavigationInteractiveTransitionBase`](https://github.com/nst/iOS-Runtime-Headers/blob/master/Frameworks/UIKit.framework/_UINavigationInteractiveTransitionBase.h), on this one we find the property [`animationController`](https://github.com/nst/iOS-Runtime-Headers/blob/master/Frameworks/UIKit.framework/_UINavigationInteractiveTransitionBase.h#L42), bingo! We return that in `…animationControllerForOperation:…`, but still no luck.

If we look even more on our `interactivePopGestureRecognizer` property it looks like everything should be ok, it is enabled, got a valid vew in the view hierarchy and a valid target/action. I still have no idea why it does not fire (checked by adding another target/action).

What I ended up doing was creating a new instance of a `UIScreenEdgePanGestureRecognizer`, configure it with the left edge and set the exact same target/action as our `interactivePopGestureRecognizer`, with other words, the instance of `_UINavigationInteractiveTransition` and the action `handleNavigationTransition:`. And finally add it to the `UINavigationController`s view.
There is a bit of `-valueForKey:`and `NSSelectorFromString()` in the final solution that I am not very proud of, but it works perfectly. Remember to be safe when implementing a solution like this, Apple change stuffs, as we all know.

I’m [@blommegard](http://twitter.com/blommegard) on Twitter, ping me with feedback or if I have missed something obvious. :)
