---
layout:     post
title:      "swift timer"
subtitle:   "swift timer--thread safety--GCD"
date:       2016-12-17 23:00:00
author:     "Haoking"
header-img: "img/post-bg-re-vs-ng2.jpg"
tags:
    - iOS
    - Swift
    - Objective-C
    - Timer
    - GCD
    - Front-end
    - OSX
---

> [Please indicate the source of forwarding and be a follower of my Github](https://github.com/haoking).



## **Swift Timer**

**swift timer--thread safety--GCD**

We can easily find the swift timer online or in GitHub.

But I have no idea wether that is thread safety or not.

Because I  don't find the thread operation in some libraries.

But when I build my Rest Library today, I need a timer, and I build it.

Objective-C version :

```html
https://github.com/haoking/Timer-Objective-C
```

Swift version :

```html
https://github.com/haoking/Timer
```

If you download it, you can try it right now.

For now, I am going to explain some special point.

Firstly, Init:

```swift
    private var timeInterval : TimeInterval?
    private var timer : DispatchSource?
    private var target : AnyObject?
    private var selector : Selector?
    private var repeats : Bool = false
    private var serialQueue : DispatchQueue?
    private var _timerIsInvalidated : UInt32? = 0
    private init(timeInterval: TimeInterval, target: AnyObject?, selector: Selector, repeats: Bool)
    {
        super.init()
        self.timeInterval = timeInterval
        self.target = target
        self.selector = selector
        self.repeats = repeats
        let queueName = "WHCTimer.\(self)"
        self.serialQueue = DispatchQueue(label: queueName, qos: .default, attributes: .concurrent, autoreleaseFrequency: .inherit, target: DispatchQueue.main)
        self.timer = DispatchSource.makeTimerSource(flags: [], queue: self.serialQueue) as? DispatchSource
        self.start()
    }

    public class func timerCreate(timeInterval: TimeInterval, target: AnyObject?, selector: Selector, repeats: Bool) -> WHCTimer
    {
        return WHCTimer.init(timeInterval: timeInterval, target: target, selector: selector, repeats: repeats)
    }
```

Init the timer by DispatchSource and build serialQueue.



Secondly,  define the selector and set is_repeat, but don't forget to keep thread safety.

```swift
    private var _tolerance : TimeInterval? = 0
    private var tolerance: TimeInterval? {
        get
        {
            objc_sync_enter(self)
            return self._tolerance
        }

        set
        {
            objc_sync_enter(self)
            if newValue != _tolerance
            {
                self._tolerance = newValue
                self.resetTimerProperties()
            }
            objc_sync_exit(self)
        }
    }

    private func resetTimerProperties()
    {
        let intervalInNanoseconds : UInt64 = UInt64(self.timeInterval!) * NSEC_PER_SEC
        let toleranceInNanoseconds : UInt64 = UInt64(self.tolerance!) * NSEC_PER_SEC
        __dispatch_source_set_timer(self.timer!, DispatchTime.now().rawValue, intervalInNanoseconds, toleranceInNanoseconds)
    }

    private func start()
    {
        self.resetTimerProperties()

        __dispatch_source_set_event_handler(self.timer!, { [unowned self] in
            self.fire();
        })
        self.timer!.resume()
    }
```

There are some skills here, how to use self in getter and setter.

The most directly way is to rebuild a new variable and use it in getter and setter.

Another skill is what the @synchronized is in swift version:

```swift
objc_sync_enter(self)
	//do something here
objc_sync_exit(self)
```

When i built the Timer,  I used the system interface as much as possible, such as GCD, Thread.



Then we can publish another funcation in the timer: Fire

```swift
    public func fire()
    {
        if (OSAtomicAnd32OrigBarrier(1, &_timerIsInvalidated!) > 0)
        {
            return;
        }

        _ = self.target!.perform(self.selector, with: self)

        if self.repeats == false
        {
            self.cancel();
        }
    }
```

You can invoke the method to execute the selector whenever you want , and there is no impact on the Timer.



Lastly but not least, don't forget to canel the timer, when this class is released.

```swift
    public func cancel()
    {
        if (!OSAtomicTestAndSetBarrier(7, &_timerIsInvalidated))
        {
            let timer : DispatchSource = self.timer!
            self.serialQueue!.async {
                timer.cancel()
            }
        }
    }

    deinit
    {
        self.cancel()
    }
```

 

Okay, all you have done. Just go and enjoy it.



By the way, if you have any questions or there are something wrong here, please contect me on Email:

haokinus@gmail.com 


