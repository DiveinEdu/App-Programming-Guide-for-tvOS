新的Apple TV不再只是一个单调的媒体播放器。它能够用来玩游戏、使用生产力工具、看电影和分享体验等。所有这些都给开发者提供了更多的机会。

新的Apple TV使用**最新的iOS**框架和一些**tvOS**特有的框架。Apple TV应用开发与iOS开发非常类似，但是还有一些诸如共享的多人体验是iOS设备所不具备的。该文档描述了Apple TV特有的能力，并且提供了关于开发tvOS应用的更多信息的链接。

>Apple TV开发与发布需要一个新的开发者证书。我们可以通过类似创建iOS证书的方式获取。

####Apple TV的硬件信息
新的Apple TV硬件规格：

- 64位A8处理器
- 32GB或64GB存储空间
- 2GB内存
- 10/100Mbps以太网口
- WiFi支持802.11a/b/g/n/ac
- 1080p高清视频
- HDMI
- 新的Siri遥控/Apple TV遥控

Apple TV遥控有两个特点——内置Siri和屏幕搜索功能。其中Siri遥控在以下国家可用：

- 澳大利亚
- 加拿大
- 法国
- 德国
- 日本
- 西班牙
- 英国
- 美国

其它国家的Apple TV提供的是Apple TV遥控。图1-1是新的遥控器。它有一下一些按钮：

1. 触摸板。滑动导航、点击选取、长按弹出上下文菜单。
2. 菜单按钮。点击返回上一层。
3. Siri/Search按钮。在支持Siri的国家，长按启动Siri遥控。在其它国家长按打开屏幕搜索。
4. Play/Pause按钮。播放和暂停多媒体。
5. Home按钮。按一次返回主屏幕，按两次显示已经启动的应用。长按进入休眠状态。
6. Volume按钮。控制电视机的音量。
7. 闪电接口（Lightning connector）。充电插头。

**图1-1** Siri遥控和Apple TV遥控

![Siri遥控](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/remote_callouts_2x.png)

####客户端－服务器（C/S）应用

创建Apple TV的过程与创建iOS应用相同。我们可以创建游戏、工具、多媒体等应用。同时可以创建贯穿多个应用，拥有自定义用户界面的C/S应用。我们使用Web技术来构建C/S应用，例如HTTPS、XMLHTTPRequest、DOM以及JavaScript。通过苹果自定义的标记语言TVML创建用户界面，然后使用JavaScript实现应用的行为。TVMLKit框架在本地代码和JavaScript/TMVL之间起到桥梁的作用。

应用程序的初始化启动行为在一个JavaScript文件中进行定义。我们像往常一样创建二进制应用，并且使用TVMLKit框架加载JavaScript文件。JavaScript文件加载TVML页面并将它们显示在屏幕上。每个TVML文件表示屏幕上一个单独的页面。我们可以使用苹果提供的模版创建TVML页面。每个模版通过全屏方式展示信息。模版上的元素可以进行修改。关于TVML模版和元素的更多信息可以查看[《Apple TV Markup Language Reference》](https://github.com/DiveinEdu/Apple-TV-Markup-Language-Reference-in-Chinese)。

Apple TV上所有的视频都是通过HTTP Live Streaming和FairPlay Streaming提供。详情可以查看[《About HTTP Live Streaming》](https://developer.apple.com/library/prerelease/tvos/referencelibrary/GettingStarted/AboutHTTPLiveStreaming/about/about.html#//apple_ref/doc/uid/TP40013978)和[《FairPlay Streaming Overview》](https://developer.apple.com/library/prerelease/tvos/featuredarticles/FairPlayStreaming_Overview/FPS/FPS.html#//apple_ref/doc/uid/TP40015216)。

####顶层

用户可以将任何Apple TV的应用放置在应用菜单的最上一行，最多五个应用图标。当用户选择其中一个的时候，屏幕上方就会显示这个应用相关的内容。我们把这里叫做“Top shelf”。顶层可以展示应用内容让用户可以进行预览，或者直接跳转到应用的某个位置。

####视差图片

分层的图片能够让封面图变得更加生动。当一个界面元素被用户选中高亮，但是还没有点击的时候，能够获取焦点。这时分层图片会随着用户在遥控器触摸板上的动作旋转。每层图片的旋转速度会有细微的差别，从而产生视差效果。这些细微的效果让用户的注意力更加集中。

我们的视差图片当然是由设计师创建的，但是如何将它们运用到应用中呢？`UIImageView`被改造成可以支持视差图片。