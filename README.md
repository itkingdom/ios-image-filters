ios-image-filters
=========

Uh, so everyone seems to want instagram-style filters for images on iPhone. The way to do this (I think) is to examine how people have implemented equivalent filters in Photoshop and code them in objective c. Unfortunately, the imaging libraries are all geared for gaming (meaning they're totally inscrutable and make you want to cry to do simple operations. Go on, try to understand the OpenGL sample code Apple ships for image processing. I dare you.) 

Inexplicably, there seem to be no photoshop-style interfaces for image processing on iOS. So, I made some. Badly. Please help me fix them. My lomo filter is OK, but could be a lot better. The basics seem to work.

It's like photoshop for the UIImage class!
======================
I've worked hard to mimic the photoshop color adjustment menus. Turns out these are clutch in pretty much all the best image filters you can name right now (lomo, polaroid). For example, if you want to manipulate levels, here's your method, built straight onto the UIImage class:

    - (UIImage*) levels:(NSInteger)black mid:(NSInteger)mid white:(NSInteger)white

An API just like that cool menu in Photoshop!

The neat part is curves - a popular lomo filter makes extensive use of curves. Want to do a curves adjustment you saw on a blog? Just do this:

    NSArray *redPoints = [NSArray arrayWithObjects:
            [NSValue valueWithCGPoint:CGPointMake(0, 0)],
            [NSValue valueWithCGPoint:CGPointMake(93, 76)],
            [NSValue valueWithCGPoint:CGPointMake(232, 226)],
            [NSValue valueWithCGPoint:CGPointMake(255, 255)],
            nil];
    NSArray *bluePoints = [NSArray arrayWithObjects:
             [NSValue valueWithCGPoint:CGPointMake(0, 0)],
             [NSValue valueWithCGPoint:CGPointMake(57, 59)],
             [NSValue valueWithCGPoint:CGPointMake(220, 202)],
             [NSValue valueWithCGPoint:CGPointMake(255, 255)],
             nil];
    UIImage *image = [[[UIImage imageNamed:@"landscape.jpg" applyCurve:redPoints toChannel:CurveChannelRed] 
     applyCurve:bluePoints toChannel:CurveChannelBlue];

The problem with my curves implementation is that I didn't use the same kind of curve algorithm as Photoshop uses. Mainly, because I don't know how, and other than the name of the curve (bicubic) all the posts and documentation I simply don't understand or cannot figure out how to port to iOS. But, the curves I use (catmull-rom) seem close enough for most things.

How to integrate
======================
Copy and paste the ImageFilter.* and Curves/* classes into your project. It's implemented a Category on UIImage.

How to use
======================
    #import "ImageFilter.h"
    UIImage *image = [UIImage imageNamed:@"landscape.jpg"];
    self.imageView.image = [image sharpen];
    // Or
    self.imageView.image = [image saturate:1.5];
    // Or
    self.imageView.image = [image lomo];

What it looks like
======================
![Screen shot 1](/esilverberg/ios-image-filters/docs/ss1.png)
![Screen shot 2](/esilverberg/ios-image-filters/docs/ss2.png)


What is still broken
======================

- Gaussian blur is slow!
- More blend modes for layers
- Curves are catmull rom, not bicubic
- So much else...

Other options
=============
- Simple Image Processing: http://code.google.com/p/simple-iphone-image-processing/
- GLImageProcessing: http://developer.apple.com/library/ios/#samplecode/GLImageProcessing/Introduction/Intro.html
- CImg: http://cimg.sourceforge.net/reference/group__cimg__tutorial.html 

License
=======
MIT License, where applicable. I borrowed code from this project: http://sourceforge.net/projects/curve/files/ , which also indicates it is MIT-licensed. 
http://en.wikipedia.org/wiki/MIT_License