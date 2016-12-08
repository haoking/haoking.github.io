---
layout:     post
title:      "pip install guide"
date:       2016-12-07 22:00:00
author:     "Haoking"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - iOS
    - Front-end
    - OSX
    -pip
    -python
---

> Please indicate the source of forwarding (http://haoking.me)



## pip install

**pip is a package management system used to install and manage softeare packages written in Python**.

when you install pip on an old OSX, you can follow the example from offcial website :

 

```python
pip install
```

But sometimes there will be something wrong here on lastest OSX, and this is because of the new SIP protection mechanism on lastest OSX.

Just use this way:

```python
pip install --user
```

