---
layout:     post
title:      关于UIViewController的释放
category: blog
description: ios内存优化与管理。
---

我们知道，在 iOS的视图管理中，UIViewController是以栈的形式存储的。对于栈这种存储方式，最简单的理解为一个杯子，先放进杯子里的东西一定是最后被拿出来。当然在代码里我们可以实现删除栈中间的数据。
对于刚刚入行没几年的开发者来讲，内存管理似乎不需要过多的关注。因为强大的arc已经替我们完成了大部分内存管理的工作。但实际上，arc这种引用计数机制在实际开发中仍然存在很多坑。比如我们在controller里写了这样的代码：

```
    _webView.UIDelegate = self;
    _webView.navigationDelegate = self;
```

大家都知道在viewcontroller生命周期结束后，要将delegate置为nil。我们通常会这样去写：


```
- (void)dealloc {
    self.webView.UIDelegate=nil;
    self.webView.navigationDelegate=nil;
}
```


这样的写法通常情况下不会出错，但是当UIViewController中存在内存泄漏时，比如在block中使用了self的强引用，造成循环引用导致dealloc函数无法执行，所以清空delegate的代码也就不会执行。为了防止这种情况出现，除了检查代码中是否存在循环引用的情况，我们可以把delegate的置空语句写在viewdiddisappear函数里。
另外，如果你实在找不到究竟是哪句代码导致了内存泄漏，使得dealloc方法无法执行。我们也可以强制从视图栈里移除当前controller


```
-(void)viewDidDisappear:(BOOL)animated{
    [self removeFromParentViewController];
}
```


这样你可以打个断点看一下，dealloc函数是不是执行了。当然在写代码的时候就做好内存管理及优化才是最好的方法。