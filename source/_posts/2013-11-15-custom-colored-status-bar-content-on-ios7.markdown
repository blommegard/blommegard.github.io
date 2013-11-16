---
layout: post
title: "Custom colored status bar content on iOS7"
date: 2013-11-15 21:25
comments: false
categories: UIKit iOS
---

As we all know the only two values of `UIStatusBarStyle` are `UIStatusBarStyleDefault` & `UIStatusBarStyleLightContent`, the background behind the bar on the other hand can vary a lot nowadays. During the iOS7 redesign at [Wrapp](http://wrapp.com) we found it hard to match our yellow and brown colors with the white or black status bar content.

{% img /images/StatusBarBlackWhite.png 640 %}

I started digging around for solutions and found the ivar `_statusBarWindow` on [`UIApplication`](https://github.com/JaviSoto/iOS7-Runtime-Headers/blob/master/Frameworks/UIKit.framework/UIApplication.h#L22), yeah, yeah, I know it is private.


``` objective-c
  UIApplication *app = [UIApplication sharedApplication];
  UIWindow *window = [app valueForKey:@"statusBarWindow"];
  if (window && [window isKindOfClass:[UIWindow class]]) {
    UIImageView *overlayImageView = [[UIImageView alloc] initWithFrame:app.statusBarFrame];
    [overlayImageView setImage:[UIImage imageNamed:@"Yellow Navigation Bar"]];
    [overlayImageView setClipsToBounds:YES];
    [overlayImageView setContentMode:(UIViewContentModeScaleAspectFill)];
    [overlayImageView setAlpha:.4f];
    
    [window addSubview:overlayImageView];
  }
```

I use the same image as the `UINavigationBar` background image, but clip it to the status bar frame and add it above the bar in the window. The result:

{% img /images/StatusBar.png 640 %}

If you got a translucent bar, there will be a little more work to match the colors. Another take on this may be to create anoter window, place it in front and add the overlay to that one. But then you have to dispatch all touch event from that one as well.

Another example of what you can do when you have a reference to the status bar window is translate it with the hamburger menu just like [Interesting](http://flyosity.com/interesting/) does by [@flyosity](http://twitter.com/flyosity)

I’m [@blommegard](http://twitter.com/blommegard) on Twitter, ping me with feedback!
