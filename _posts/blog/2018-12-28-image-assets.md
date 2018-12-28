---
layout:     post
title:      iOS引导图适配iPhoneX
category: blog
description: Assets.xcassets适配
---

**引导图适配**

在开发iOS app的时候常常会有这样一个需求：app引导图，引导图的功能就是在用户首次安装应用或更新版本的时候提示用户的作用。通常情况下是四五个左右滚动的页面，实现这个需求非常简单。只要写一个带有page效果的UIScrollview，并在app启动的时候判断首次安装或者更新的时候显示出来就行了。这在iphoneX等全面屏iPhone发布以前不会有任何问题。但是现在我们遇到了这样的问题。

我们的引导图通常是铺满屏幕的几张图片，在Xcode中图片管理非常简单，只需设计师把图片切成@1x，@2x，@3x三种尺寸即可，这三种尺寸对应iPhone的三种分辨率。但是这三张图满足不了现在的需求，因为在iPhoneX这样的设备下，全屏图片会被拉伸。其实这个问题不难解决，我们可以在代码中判断设备类型，然后根据不同的类型，加载不同图片达到适配的目的。但是我觉得像Xcode这样强大的工具应该有更好的解决方式。

打开Assets.xcassets,选中一个图片资源，在Xcode中这个被称为图片集，就是这样的效果：

![](http://bucket-zyf.oss-cn-beijing.aliyuncs.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-12-28%20%E4%B8%8B%E5%8D%884.15.18.png)

没错只有这三种尺寸，并没有iPhoneX那样的分辨率，这不科学啊，怎么这么不人性化。其实并不是，点击导航栏加号新建一个LaunchImage是这样的：

![](http://bucket-zyf.oss-cn-beijing.aliyuncs.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-12-28%20%E4%B8%8B%E5%8D%884.23.22.png) 

快看，最上面的两个图片框就是iPhoneX的图片适配，为毛其他的图片没有，也找不到任何菜单可以添加。不甘心。
分别在自己的图片资源和LaunchImage资源上右键查看菜单，单击show in finder，分别用编辑器打开Contents.json

LaunchImage是这样的:

```
{
  "images" : [
    {
      "orientation" : "portrait",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "minimum-system-version" : "11.0",
      "subtype" : "2436h",
      "scale" : "3x"
    },
    {
      "orientation" : "landscape",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "minimum-system-version" : "11.0",
      "subtype" : "2436h",
      "scale" : "3x"
    },
    {
      "orientation" : "portrait",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "minimum-system-version" : "8.0",
      "subtype" : "736h",
      "scale" : "3x"
    },
    {
      "orientation" : "landscape",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "minimum-system-version" : "8.0",
      "subtype" : "736h",
      "scale" : "3x"
    },
    {
      "orientation" : "portrait",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "minimum-system-version" : "8.0",
      "subtype" : "667h",
      "scale" : "2x"
    },
    {
      "orientation" : "portrait",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "minimum-system-version" : "7.0",
      "scale" : "2x"
    },
    {
      "orientation" : "portrait",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "minimum-system-version" : "7.0",
      "subtype" : "retina4",
      "scale" : "2x"
    },
    {
      "orientation" : "portrait",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "scale" : "1x"
    },
    {
      "orientation" : "portrait",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "scale" : "2x"
    },
    {
      "orientation" : "portrait",
      "idiom" : "iphone",
      "extent" : "full-screen",
      "subtype" : "retina4",
      "scale" : "2x"
    }
  ],
  "info" : {
    "version" : 1,
    "author" : "xcode"
  }
}
```

正常的图片资源Contents.json

```
{
  "images" : [
    {
      "idiom" : "universal",
      "filename" : "2.png",
      "scale" : "1x"
    },
    {
      "idiom" : "universal",
      "filename" : "2@2x.png",
      "scale" : "2x"
    },
    {
      "idiom" : "universal",
      "filename" : "2@3x.png",
      "scale" : "3x"
    },
    {
      "idiom" : "iphone",
      "filename" : "guide_two.png",
      "subtype" : "2436h",
      "scale" : "3x"
    }
  ],
  "info" : {
    "version" : 1,
    "author" : "xcode"
  }
}
```

对比一下就发现那两个iPhoneX的配置了，在正常的配置下添加那两项保存，看看Xcode的变化，果然多了两个图片的尺寸，把设计师切好的适配iPhoneX的图片拖拽到里边就行了，运行项目看一下，完美适配，大功告成