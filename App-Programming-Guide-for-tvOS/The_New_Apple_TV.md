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


##新的Apple TV

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

我们的视差图片当然是由设计师创建的，但是如何将它们运用到应用中呢？`UIImageView`被改造成可以支持视差图片，因此大多数情况下我们只要对代码进行微调就可以了。整个工作流如何改变取决于我们是直接将图片添加到应用内还是从服务器动态加载。

####新的tvOS框架

Apple tvOS引入了一下几个特有的框架。

- TVMLJS。描述了C/S应用中用来加载TVML页面的JavaScript接口。见[《TVJS Framework Reference》](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076)。
- TVMLKit。提供一种整合JavaScript和TVML的方法。见[《TVMLKit Framework Reference》](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLKit/Reference/TVMLKit_Collection/index.html#//apple_ref/doc/uid/TP40016429)。
- TVServices。描述了如何为应用添加顶部扩展[《TVServices Framework Reference》](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412)。

####从iOS中继承的框架

Apple tvOS从iOS继承了许多框架。

- Accelerate
- AudioToolbox
- AudioUnit
- AVFoundation
- AVKit
- CFNetwork
- CloudKit
- CoreBluetooth
- CoreData
- CoreFoundation
- CoreGraphics
- CoreImage
- CoreLocation
- CoreMedia
- CoreSpotlight
- CoreText
- CoreVideo
- Darwin
- Foundation
- GameController
- GameKit
- GameplayKit
- GLKit
- ImageIO
- MachO
- MediaAccessibility
- MediaPlayer
- MediaToolbox
- Metal
- MetalKit
- MetalPerformanceShaders
- MobileCoreServices
- ModelIO
- OpenGLES
- SceneKit
- Security
- simd
- SpriteKit
- StoreKit
- Swift Standard Library
- SystemConfiguration
- UIKit

####新用户界面的挑战

Apple tvOS并没有提供鼠标给用户，并且也不能通过手势和触摸交互。相反，用户使用新的Siri遥控或者游戏控制器进行操作。

使用新的控制方式，整个用户体验都会变得完全不一样。Mac和iOS设备一般是单人体验。虽然用户可能会通过应用与其它人进行交互，但是同一个设备一般只有一个人使用。但是新的Apple TV的用户体验更加社会化。几个人可以一起坐在沙发上一边交流一边使用应用。在设计应用的使用将这些变化都考虑进去是非常重要的。

####资源限制

Apple TV上的应用没有持久化的本地存储。这意味着每个应用都应该将数据存放在iCloud，并且通过一种用户体验较好的方式将它们获取到。

除了缺乏本地存储，Apple TV应用的大小也被限制在200MB。如果一个应用的大小超过了限制，就需要使用按需加载的方式将它们打包。知道什么时候加载以及如何价值新的资源对开发一款成功的应用非常重要。更多关于按需加载资源的信息，见[《On-Demand Resources Guide》](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)。

下一章：[《创建一个C/S应用》](./Creating_a_Client-Server_App.md)。