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


##操作用户界面

iOS设备上，用户通过直接触摸屏幕进行交互。但是Apple TV只能通过遥控器间接进行操作。用户在屏幕上导航，并且通过遥控选取某个物体。当用户导航到一个物体时，它就会获取焦点。在基于焦点的交互模型中，如果一个视图可以获取焦点，则用户可以将焦点移动到屏幕上的其它物体上。这样会触发焦点更新动作。获取焦点的视图会被作为用户下一个动作的目标。例如，如果屏幕上的一个按钮已经获取焦点了，当遥控器上的选取事件发生时，按钮的动作会被触发。

UIKit框架只支持基于焦点的界面，并且在大部分情况下这种行为是由系统默认提供的。我们在程序中也可以触发焦点移动，但是不能够指定它的移动方向。例如`UIButton`对象可以获取焦点，但是`UILabel`对象不行。当使用用户自定义控件的时候，我们需要实现自定义的焦点获取行为。这个在“[在应用内支持焦点](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwiththeAppleTVRemote.html#//apple_ref/doc/uid/TP40015241-CH5-SW2)”章节可以看到。UIKit控件如`UIButton`、`UITextField`、`UITableView`、`UICollectionView`、`UITextView`、`UISegmentedControl`和`UISearchBar`等都默认可以获取焦点。

####使用焦点引擎控制焦点

UIKit中通过一个叫“焦点引擎”的东西管理控件的焦点的获取和移动。用户有多重方式控制焦点：遥控器（不同种类的）、游戏控制器、模拟器等等。焦点引擎监听输入源的焦点事件。当一个事件发生时，焦点引擎自动决定是否应该更新和通知我们的应用。焦点系统帮助我们在不同应用之间提供统一的用户体验，自动为应用提供对所有输入方法的支持，而开发者只要专注于应用本身的功能，而不是定义和重新发明基本的导航事件。

只有焦点引擎能够显式的更新焦点，意味着系统并没有提供API来直接在某个方向操作和移动焦点。只有在用户发送移动事件、系统和应用请求更新的时候，焦点引擎才会更新焦点。更多关于焦点的更新，见[《Updating Focus Programmatically》](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwiththeAppleTVRemote.html#//apple_ref/doc/uid/TP40015241-CH5-SW14)。

焦点引擎的控制使得焦点不会全屏乱跑，并且在不同的应用之间提供统一的操作方式。这样做可以防止造成用户感到困惑，同时开发者不需要去自己管理焦点。

####`UIFocusEnvironment`协议

焦点引擎通过`UIFocusEnvironment`协议与应用进行通信，定义一组视图的焦点行为。UIKit中的一些类能够响应这个协议，如`UIView`、`UIViewController`、`UIWindow`和`UIPresentationController`等。重写`UIFocusEnvironment`方法可以让我们对程序的焦点行为进行控制。

####用户移动焦点

####决定焦点的移动

####初始化焦点和配置焦点移动链

####焦点更新

####解剖焦点更新

####系统移动焦点

####程序更新焦点

####在应用中支持焦点

####在视图控制器中支持焦点

####在CollectionView和TableView中支持焦点

####在自定义视图中支持焦点

####焦点相关动画

####调试焦点问题

####为什么焦点移动出错

