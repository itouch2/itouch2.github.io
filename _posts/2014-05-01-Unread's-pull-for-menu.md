---
layout: post
title:  "Unread 的 pull-for-menu"
date:   2014-05-01
---

*革命进化的解析*

（本文译自：[Unread's pull-for-menu](http://subjc.com/unread-overlay-menu/) ）

##背景

在2013年中期，互联网的RSS世界有一个巨大的改变。Google 宣称他们的 RSS 订阅服务，[Google Reader](http://www.google.com/reader/about/)，被关闭。对此，数百万的声音在惊慌中呼叫，然而突然间又销声匿迹了。

缩减的用户量是其宣称关闭Google Reader的原因，尽管来自 [Google Reader](http://www.google.com/reader/about/) 的用户的强烈反应表明这项服务仍然有广大的用户群。虽然互联网有种乐观的意识认为对于在RSS方面对于很多公司有很多机会，因为以前这个服务的市场已经被像 Google 这样的巨头占领了，但是仍然存在对 RSS 的担忧。越来越多的 [Google Reader](http://www.google.com/reader/about/) 的替代品出来。

尽管曾经有导致 RSS 消失的因素，RSS 至今仍活跃着，并且如今如 [Feedly](http://feedly.com/#discover), [Feedwrangler](https://feedwrangler.net/welcome.html) 与 [Feedbin](https://feedbin.com/) 正提供着 Google Reader 以前的服务。随之而来的是一大批现代的 iOS RSS 阅读器。在其中，有 [Unread](http://jaredsinclair.com/unread/)，一个轻量干净并易于使用的提供上述服务的客户端 ，由 [Jared Sinclair](http://jaredsinclair.com/)。在很短的时间里，它就在 app store 里有很多用户，多到很有可能你现在正通过 [Unread](http://jaredsinclair.com/unread/) 阅读本文。

这篇文章是有关 [Unread](http://jaredsinclair.com/unread/) 的 pull-for-menu 的交互，同时它也有关这个交互方面的历史，这些交互是怎么演变的。

##风景

如果我们去探索 iOS 上的有关新闻与内容聚合的 app 的情况，我们可以发现像 [Flipboard](http://flipboard.com/) 与 [Pulse](https://www.pulse.me/) 这些的应用，这些应用不仅仅提供内容消费的服务，同时提供内容的探索。你可以想像自己使用它们，并在周日的早上一边喝着咖啡（或地球的另一端喝着下午茶）一边沉浸在阅读杂志的体验中。

在另一方面，我们有些应用如 [Reeder](http://reederapp.com/ios/)，这种应用可以提供以一种最高效的方式来消费内容，这种应用你可以用来摆脱每天出行在路上的单调，使你避免 [FOMO](http://en.wikipedia.org/wiki/Fear_of_missing_out)。此时你可以变想使用 [Unread](http://jaredsinclair.com/unread/)。

[Unread](http://jaredsinclair.com/unread/) 延续了我们[之前](http://subjc.com/castro-playback-scrubber/)讨论的限制的主题。它使用的方式很简单：你只要登录你的 RSS 订阅的账号，然后就可以开始享受阅读了。[Unread](http://jaredsinclair.com/unread/) 提供了一个专门为单手用户设计的交互体验方式。

为了更好地认识 [Unread](http://jaredsinclair.com/unread/) 的菜单交互方式的来源。让我们先从 Darwinistic 开始。

##进化

现在我们回过头看看 [Tweetie](http://en.wikipedia.org/wiki/Tweetie)，一款认为 iOS 应用的典范。正是它引入了现在随处可见的下拉刷新。下拉刷新如此被接受，甚至被 Apple 采纳为 Mail.app 收件箱刷新的默认交互方式。

随之，Facebook 的 iOS 应用使抽屉式的导航（又称为 "God Burger","Burger Basemen"）。尽管现在他们把其从主界面移除（在联系人中仍保留），但该交互在 iOS 设计领域中的使用使其成为一种传统的可接受的模式。

到现在，我们有 [Unread's menu](http://jaredsinclair.com/unread/)，它是这两种被接受的传统的交互方式的结合，这是已经教给我们如何与设计进行交互的两种革命性交互 方式的进化。

[Unread](http://jaredsinclair.com/unread/) 提供了一种呈现菜单的方式，当然你也可以认为这不是必需的。这是这些现有交互的产物。

##解析

今年的 WWDC 发布了一个闪亮的新特性来帮助开发者：UIKit Dynamics, Text Kit, Sprite Kit 以及一些 UIViewController 过渡。我们将复用这其中的两种来实现 [Unread's menu](http://jaredsinclair.com/unread/)，UIViewController 过渡以及 UIKit Dynamics，尽管 后者我们不会支持使用。	

我们拉开内容呈现菜单时第一件注意的是 pull 指示器的弹跳效果。这个效果在视觉上很显明。让人想起 iOS 6 中的下拉刷新动画，非常轻量又有意义的交互过程。

我们之前已经简单地介绍与使用过 UIKit Dynamics，这次我们会创建一个抽象的层。 

##7个参数的方法

Objective-C 中的一个优良的特性即是函数命名习惯。Objective-C 有其语言的多样性，提供了一种自然的方式来描述方法的作用，尽管其方法的长度会吓坏一些新的开发者。这其中有一个即是新添加的 UIView block 的动画方法 ```animateWithDuration:delay:usingSpringWithDamping:initialSpringVelocity:options:animations:completion:```，尽管这个不是 Cocoa Touch 里最长的，但是绝对 是可以上榜的。

尽管其看起来很复杂，它还是一个十分简单又有效的给你的界面添加动态效果的强大方法，并且不会过多的使用 UIKit Dynamics 栈。在早些的博客中，一些观察敏锐的读者就用过这个方法，这个看起来是一个很不错的 `pull-for-menu`的弹簧效果。

##拉伸

你想像下你在拉伸一根橡胶带，你拉得越多，橡胶带就会越瘦。这种物理行为就在 `Unread` 的拉伸效果中体现。尽管这是个你需要仔细观察才会发现的小细节，它仍能加强我们在拖拽滚动视力超过其边界```contentSize```时遇到了阻力的感知。

为了实现这个效果，我们这里创建一个view(```SCSpringExpandingView```)，它可以在两帧之前进行动画。综合的 view 会与其 superview 匹配高度，变成一个小的正方形的view

```
- (CGRect)frameForCollapsedState
{
    return CGRectMake(0.f, CGRectGetMidY(self.bounds) - (CGRectGetWidth(self.bounds) / 2.f), 
                      CGRectGetWidth(self.bounds), CGRectGetWidth(self.bounds));
}
```

当我们把 view 变成扩展的状态，它的 frame 会变成 superview 的高度，但是宽度是其一半。我们也会改变其水平方位的位置以使其居中。

```
- (CGRect)frameForExpandedState
{
    return CGRectMake(CGRectGetWidth(self.bounds) / 4.f, 0.f, 
                      CGRectGetWidth(self.bounds) / 2.f, CGRectGetHeight(self.bounds));
}
```

同时，我们设置其圆角半径为宽度的一半，使其在缩合扩展的时候有一个圆形的外观。我们还需要在其扩展缩合的过程中修改其宽度，否则，圆角会与 view 的宽度的相反。

```
- (void)layoutSubviews
{
    [super layoutSubviews];
    self.stretchingView.layer.cornerRadius = CGRectGetMidX(self.stretchingView.bounds);
}
```

现在剩下的就是如何用我们的长名字的新朋友 ```animateWithDuration:delay:usingSpringWithDamping:initialSpringVelocity:options:animations:completion:``` 来动画切换两种状态。

我们刚刚已经介绍了这个方法很多遍，现在让我们再细看下这两个参数，```usingSpringWithDamping```与```initialSpringVelocity```。

```usingSpringWithDamping``` 的值是从0.0~1.0来决定弹簧效果的振幅，在物理上即是弹跳的力度。越接近1.0越会增强弹跳的力度并导致很小的振幅。越接近0.0则会减弱弹跳的力度并导致很大的振幅。

```initialSpringVelocity``` 接受一个 CGFloat 为输入，并且其值会影响在动画期间其动画的距离。1.0的值可以在1秒中完成完整的动画距离，而0.5会在1秒中完成一半的距离。

尽管这些参数是基于物理的属性，这种 case 最大的感受是

```
[UIView animateWithDuration:0.5f
                      delay:0.0f
     usingSpringWithDamping:0.4f
      initialSpringVelocity:0.5f
                    options:UIViewAnimationOptionBeginFromCurrentState
                 animations:^{
                     self.stretchingView.frame = [self frameForExpandedState];
                 } completion:NULL];
```

就是这样就好了。只要一个方法调用与一几个神奇的数字，我们就可以发挥 iOS7的 UIKit 的优势。


##一排三个

现在我们创建了 ```SCSpringExpandingView```，我们需要创建一个 view 来容纳三个 SCSpringExpandingView。我们称其为 ```SCDragAffordanceView```.

```SCDragAffordanceView```的基本作用是去排列三个```SCSpringExpandingView```，同时也提供拉动拉出菜单的交互界面过程。、

为了排列```SCSpringExpandingView```，我们重写 layoutSubviews 并且等分排列每个 view 的坐标。

```
- (void)layoutSubviews
{
    [super layoutSubviews];
    
    CGFloat interItemSpace = CGRectGetWidth(self.bounds) / self.springExpandViews.count;
    
    NSInteger index = 0;
    for (SCSpringExpandView *springExpandView in self.springExpandViews)
    {
        springExpandView.frame = CGRectMake(interItemSpace * index, 0.f, 4.f, 
                                 CGRectGetHeight(self.bounds));
        index++;
    }
}
```

现在我们可以展示这些 view 了，我们还需要在被设置 ```setProgress:``` 的方法去更新。如果我们再看看```Unread```，我们可以看到每一个弹簧view 都有三个唯一的状态：缩合，展开与完成。























