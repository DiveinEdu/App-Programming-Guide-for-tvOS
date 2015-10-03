# App-Programming-Guide-for-tvOS
中文版苹果官方《tvOS应用编程指南》
英文原版：[《App Programming Guide for tvOS》](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html#//apple_ref/doc/uid/TP40015241-CH12-SW1)

本翻译没有获得**苹果公司**的授权，纯属学习爱好。

####欢迎大家帮忙翻译校正或者提供更好的Apple TV开发资料。
####交流群：85157830（AppleTV开发梦工厂）
####问答网站：[潜心俱乐部](http://divein.club)

##致谢
####翻译（按加入时间排序）
- [戴维营教育](http://v.diveinedu.com)
- [AndyLiao8](https://github.com/AndyLiao8)
- [Cheetah.Kiong](https://github.com/wuqiong)
- [wcrane](https://github.com/wcrane)

##在线阅读

本文档使用[Gitbook](http://diveinedu.gitbooks.io/app-programming-guide-for-tvos/)制作，可以直接在线阅读或者导出PDF、EPUB等格式。


##主要内容

##创建视差插图

应用程序图标必须使用分层图片，而其它可以获取焦点的UI控件可以选择性使用分层图片。如果我们的界面使用UIKit视图和焦点API。当这些控件获取焦点的时候，自动读取合适的图层并生成视差效果。可以获取焦点的UI控件需要包含一个分层的图片。

UIKit在运行时支持两种不同的包含分层数据的格式。它能够从一个新的分层格式（**.lsr**）图片或者资源目录下读取分层图片。系统中支持其它图片格式的地方都支持分层图片格式文件。例如游戏中心的成就图片就是使用的分层图片。可以使用Xcode或者Parallax Previewer应用创建**.lsr**文件。

##使用视差预览应用创建LSR图片

[下载](https://developer.apple.com/tvos/download/)Parallax Previewer创建和预览**.lsr**图片。图7-1展示了Parallax Previewer的使用情况。它由如下几部分组成：

1. 分层图片区。包含每个分层的组成图片。
2. 预览区，包含和显示图层。
3. 视图尺寸，用于改变图片的查看尺寸。
4. 位置区，调整所选中的图层的位置。
5. 添加和删除图层。
6. 尺寸区，调整选中的图层尺寸。
7. 播放按钮，显示图片的视差效果。
8. Apple TV图标选择框，勾选给Apple TV的图标添加阴影效果。
9. 背景，点击改变视图区的背景。

图7－1 Parallax Previewer

![](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/ParallaxPreviewer_2x_2x.png)

**.lsr**图片的创建流程：

1. 为每个图层导入单独的**.png**或者**.jpeg**文件。
2. 调整图层的尺寸和位置。
3. 点击**File > Export > LSR**创建新的LSR文件。

##使用Xcode创建LSR图片

将**.png**图片拖入应用程序的Assets.xcassets中，创建一个图片栈（Apple TV Image Stack）。每个**.png**代表视差图片中的一个图层。点击属性区的**Export**按钮导出图片。图7-2显示了一个应用程序图标。

图7-2 XCode创建视差图片

![](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/xcodeimages_2x.png)

将已有的**.lsr**文件直接拖入应用的资源目录就可以将它包含到应用程序中。当应用程序编译打包后，**.lsr**图片会被包含到**.car**文件中。

##创建LCR图片

Xcode使用**.lsr**文件或者资源目录包含视差图片。这些文件在处理后被包含在应用程序里。如果我们想要加载一个没有存放在应用程序中的图片（或者按需加在资源）。需要将这些图片处理成**.lcr**格式的文件。使用`layerutil`命令可以创建**.lcr**图片。`layerutil`工具将**.lsr**文件转化为**.lcr**文件。

打开终端，并且使用如下命令创建**.lcr**图片。

表7-1 `layerutil`命令创建**.lcr**图片。

```bash
xcrun —-sdk appletvos layerutil —-c <filename.lsr>
```

创建的**.lcr**图片与LSR文件同名。我们可以使用`--o <new_filename.lcr>`选项给**.lcr**文件命名。

##使用视差图片

应用视差图片的步骤：

- 创建`UIImage`对象。
- 根据图片的位置选用不同的加载方式。
	1. Bundle——使用`imageNamed:`加载。
	2. 下载的文件——使用`imageWithContentsOfFile:`加载。
- 使用加载的图片创建`UIImageView`对象。
- 如果一个`UIImageView`是另外一个视图的一部分，需要将它的`adjustsImageWhenAncestorFocused`属性设为`YES`。

当视图获取焦点后，视差图片将会正确显示。