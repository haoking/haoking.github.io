---
layout:     post
title:      "UITableView scroll by fixed distance"
subtitle:   "UITableView scroll by fixed distance"
date:       2016-12-07 23:00:00
author:     "Haoking"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - iOS
    - Front-end
    - OSX
    - UITableView
    - scrollView
---

> [Please indicate the source of forwarding](http://haoking.me).
>



## **UITableView scroll by fixed distance**

**Each sliding operation on UITableVIew, sometimes we want tableview to scroll by fixed distance**

You can do this by many kinds of ways, but the most direct way : 

```objective-c
- (void)scrollViewWillEndDragging:(UIScrollView *)scrollView withVelocity:(CGPoint)velocity targetContentOffset:(inout CGPoint *)targetContentOffset;
```

This method comes from UIScrollViewDelegate.

As we all known, UITableView also inherited from UIScrollView.

So we can use this method in UITableView or UITableViewController.

As fo the specific implementation of sliding , Let's have a look on code firstly:

```objective-c
#define PAGE_SIZE 100  //PAGE_SIZE means the height of cell
- (void)scrollViewWillEndDragging:(UIScrollView *)scrollView withVelocity:(CGPoint)velocity targetContentOffset:(inout CGPoint *)targetContentOffset
{
        CGFloat pageSize = PAGE_SIZE;
        NSInteger page = roundf(targetContentOffset->y / pageSize);
        CGFloat targetY = pageSize * page;
        targetContentOffset->y = targetY + CGRectGetHeight(self.tableView.bounds) / 2;
}
```

Then let's explain the specific attributes in this method.

**velocity**

This is the speed when ending drag. 

On this way, scrollView will slide a distance in the process of reducing the speed to 0.



**targetContentOffset**

if you do nothing in this method, targetContentOffset will be the end point of scrollView.

To use this attribute, you can use it like this : 

```objective-c
targetContentOffset->x
targetContentOffset->y 
```

or

```objective-c
 *targetContentOffset.x
 *targetContentOffset.y
```

But if you change the  targetContentOffset, as the result, the end point of scrollView will be changed st the same time.

So now you know that the most important thing is:

**Calculate the desired point**



That's it.


