---
layout:     post
title:      "pip install guide"
subtitle:   "pip install guide"
date:       2016-12-07 22:00:00
author:     "Haoking"
header-img: "img/post-bg-re-vs-ng2.jpg"
tags:
    - iOS
    - Front-end
    - OSX
    - pip
    - python
---

> [Please indicate the source of forwarding and be a follower of my Github](https://github.com/haoking).



## **pip install**

**pip is a package management system used to install and manage softeare packages written in Python**.

when you install pip on an old OSX, you can follow the example from offcial website :

 

```shell
pip install
```

But sometimes there will be something wrong here on lastest OSX, and this is because of the new SIP protection mechanism on lastest OSX.

Just use this way:

```shell
pip install --user
```


