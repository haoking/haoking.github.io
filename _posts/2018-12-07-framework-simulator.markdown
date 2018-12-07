---
layout:     post
title:      ".a or framework compile on Simulator without x86_64 architecture"
subtitle:   "x86_64 architecture error"
date:       2018-12-07 9:00:00
author:     "Haoking"
header-img: "img/post-bg-re-vs-ng2.jpg"
tags:
    - .a project
    - Framework
    - Simulator
    - architecture x86_64
    - Xcode Error
---

> [Please indicate the source of forwarding and be a follower of my Github](https://github.com/haoking).



## **.a or framework cannot compile on simulator **

**Architecture x86_64 error**

```shell
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

This error sometimes appears when I invoke some .a or framework which doesn't support simulator.

As the x86_64 is desktop cpu architecture which Xcode simulator depends on.



**Solve the problem**

The best way to solve this problem is not to use the functions in the framework.

Even if you invoke this framework in your project, it will not compile error when you don't use the specific functions in the framework.



**Another workaround to solve this problem is to take advantage of the .a or framework's inheritance features**

Look at compiling a framework process on iOS.

Step1: iOS will invoke the framework's .h file first.

Step2: The framework's .m file will be compiled in the project.

Step3: The full project will compile all other .h and .m files which are not in the framework.



Depends on this we have a skill to solve the previous problem.

If we create a .m file in the project, this .m file will hook the framework's .m file before system invokes it.

Okay, depends on this process, this following specific code will be simple.



Example:

xxxx.a or xxxx.framework exposure the .h header file: EncryptionInfo.h

```objective-c
@interface EncryptionInfo : NSObject

-(void)initGlobalVariable;

@end
```



Then we can create a .m file in our working project: EncryptionInfo.m

The next we use "TARGET_IPHONE_SIMULATOR" this target to process this .m file

```objective-c
#import <EncryptionInfo.h>// or #import "EncryptionInfo.h"
#if TARGET_IPHONE_SIMULATOR//simulator target
@implementation EncryptionInfo

@end
#endif
```



Now, this problem is solved.

Even though this will works on simulator and device, I don't recommand to keep this in online version.

Thanks!



If you like my blog, please :

[Please indicate the source of forwarding and be a follower of my Github](https://github.com/haoking).



By the way, if you have any questions or there is something wrong here, please contact me on Email:

haokinus@gmail.com 


