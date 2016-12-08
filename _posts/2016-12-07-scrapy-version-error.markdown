---
layout:     post
title:      "scrapy version error: cannot import name xmlrpc_client"
subtitle:   "scrapy version error: cannot import name xmlrpc_client"
date:       2016-12-07 23:00:00
author:     "Haoking"
header-img: "img/post-bg-re-vs-ng2.jpg"
tags:
    - iOS
    - Front-end
    - OSX
    - pip
    - python
    -scrapy
---

> Please indicate the source of forwarding (http://haoking.me)



## scrapy version error

**error: cannot import name xmlrpc_client **

when you check the version of scrapy :

```shell
scrapy version
```

But the disappointment error: cannot import name xmlrpc_client

**Actually, the reason comes from "six library".**

In principle, you just need to delete the six library and reinstall.

There is the issue on official website, and the workaround is :

```shell
sudo rm -rf /Library/Python/2.7/site-packages/six*
```

```shell
sudo rm -rf /System/Library/Frameworks/Python.framework/Versions/2.7/Extras/lib/python/six*
```

```shell
sudo pip install six
```



But the second step is always an error. No matter what you did or how many times you did, you still cannot pass it.

I also meet this problem when i install scrapy.

I did search on internet. There are many solutions to this problem, but no one works.

In the end, I fixed this.(this is a hard and long time,55555555)



Actually, all errors come from the lastest OSX system.

On lastest OSX system, apple do add an new protection mechanism : SIP

And you cannot  through sip even using root or sudo.



**The only way is to shut down SIP:**

​	1. Restart system, and have a long press the "command + R".

​	2.Enter system recovery mode, and open Terminal on menu.

​	3.enter this command:

```shell
csrutil disable
```

​	4.restart.

After this , you shut down your sysytem SIP.

Then you can enter the command above and enjoy the success!!!!!




