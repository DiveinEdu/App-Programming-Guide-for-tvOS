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

##使用游戏控制器

用户可以像连接iOS设备一样将游戏控制器连接到Apple TV上。当一个游戏控制器连接到Apple TV上时，这个控制器也可以用来控制界面上的焦点移动。底层的游戏控制器输入被自动转化为上层的事件在响应链上进行传递。如果一个应用完全依赖于UIKit以及基于焦点的交互，那么可以不做任何修改就可以支持游戏控制器。遥控器和游戏控制器的事件对我们来说都是透明的，完全由系统自动处理。

如果我们需要获取底层的控制器输入，可以使用**Game Controller**框架。见[《游戏控制器编程》](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276)。

Apple TV上的游戏控制器框架有如下几个显著的变化：
- 新的微型游戏控制器配置（`GCMicroGamepad`）表示Apple TV遥控的功能。
- 新的视图控制器类（`GCEventViewController`）被用来将遥控和控制器的输入路由到应用中。

##支持游戏控制器的需求

支持游戏控制器需要遵循以下几个要求。这些要求主要用来确保游戏的可玩性。

**游戏必须支持Apple TV遥控**。游戏不能强制用户使用控制器。
**支持游戏控制器的tvOS游戏，必须支持扩展的控制器布局**。支持各式各样的tvOS游戏控制器。

**必须支持暂停**。所有游戏控制器必须包含暂停按钮。当用户按暂停后，正在玩的游戏应该被暂停。而如果当时正处于菜单页面，按暂停按钮返回前一个页面。

##将Apple TV遥控器作为游戏控制器

Apple TV遥控可以作为一个功能受限的控制器使用。和其它控制器一样，遥控器在Game Controller框架中也被当作一个`GCController`对象。Apple TV遥控支持`GCMotion`和`GCMicroGamepad`两种模式。只有Apple TV遥控支持微型手柄模式，想要支持其它游戏控制器，我们必须实现扩展的游戏手柄模式。

扩展的控制器有以下特征：

- 遥控上的触控板可以用作十字键（D-pad），提供模拟数据输入。
- 遥控在垂直和水平方向都能使用。由应用决定是否需要自动翻转输入数据。
- 通过紧按触摸板，作为数字按钮（A）输入内容。
- 遥控上的**Play/Pause**按钮用作数字键（X）。
- 菜单按钮用作暂停按钮。

##确定控制器输入的目标

在iOS上，游戏控制器事件由Game Controller框架处理。默认情况下，tvOS上的UIKit框架处理底层的控制器输入，并且在响应链上传递上层事件。但是底层的游戏控制器事件默认不可用。因为所有事件都被UIKit框架处理了。如果我们需要直接读取控制器的输入，必须先关闭UIKit。如图6-1所示两条输入路径。

图6－1 控制器输入目标

![控制器输入目标](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/GCEventViewController_2x.png)

例如，一个常见的游戏设计是：一个视图显示主菜单选项，第二个菜单显示游戏界面。主菜单可以使用UIKit元素来实现，这样它就可以与焦点行为保持一致。当游戏视图显示的时候，我们可以关闭UIKit的输入过程，从而使用游戏控制器来直接获取输入。

在tvOS上，当我们想要在游戏里直接处理输入的时候。需要使用`GCEventViewController`或者它的子类来显示游戏内容。当`GCEventViewController`对象的视图或自视图成为第一响应者后，游戏控制器的输入被视图控制器捕获到，并且发送给应用中的游戏控制器框架。

当视图控制器的视图结构中有一个视图成为第一响应者后，UIKit正常处理事件。一旦在响应链中有一个`GCEventViewController`对象，我们可以通过`controllerUserInteractionEnabled`属性开关事件的处理流程。如果我们的游戏混合了UIKit和Metal内容，当游戏暂停时，改变`controllerUserInteractionEnabled`的属性来打开UIKit交互。当选择菜单上的选项恢复游戏的时候，应该重新切换回游戏控制器。