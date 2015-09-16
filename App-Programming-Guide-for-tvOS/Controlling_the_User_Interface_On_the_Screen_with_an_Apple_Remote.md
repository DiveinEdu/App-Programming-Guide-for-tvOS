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
焦点引擎根据遥控器或者其它输入设备的导航事件自动决定焦点的移动。用户可以在二维平面上移动焦点：上、下、左、右还有斜向移动（需要硬件支持）。例如用户往左滑动，焦点引擎将在当前焦点视图的左侧尝试寻找一个可以获取焦点的控件。如果存在这样一个控件，则让它获取焦点，否则继续停留在当前视图上。

焦点引擎自动处理输入设备所支持的复杂行为。包括由快速滑动引起的动量运动，根据焦点速度调节动画快慢，播放导航音乐，移动到屏幕外导致滚动。了解更多焦点相关的动画，见[《UIFocusAnimationCoordinator Class Reference》](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UIFocusAnimationCoordinator_Class/index.html#//apple_ref/doc/uid/TP40015427)。

####决定焦点的移动

当用户移动焦点的时候，焦点引擎首先给应用程序界面创建一个快照，然后将所有可见区域中可以获取焦点的视图高亮。这意味着如果一个视图如果完全被另外的视图覆盖的话，它将被忽略。而那些只是部分被覆盖的视图，也只会考虑它的可见区域。通过这种技术，焦点引擎从当前视图开始，查找运动路径上的所有可以获取焦点的区域。搜索区域的大小与当前焦点视图的大小直接相关。

**图3-1 焦点运动**

![焦点运动](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/large_view_2x.png)

如果焦点引擎找到了一个可以接受焦点的视图，它会通过`shouldUpdateFocusInContext:`代理方法给应用提供一个评估是否移动焦点的机会。焦点引擎按前一个和下一个的顺序通知每个焦点环境（先通知前一个视图，然后下一个视图，最后通知它们的父视图）。只要有一个视图返回`NO`，这次移动就会被取消。更多关于焦点环境的信息，见[《UIFocusEnvironmentProtocol Reference》](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UIFocusEnvironment_Protocol/index.html#//apple_ref/doc/uid/TP40015223)。

####初始化焦点和焦点移动链
程序启动时，焦点引擎默认选择一个起始视图。通常是最靠近屏幕左上角的一个可获取焦点的视图。

**图3-2 初始焦点视图的查找顺序**

![](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/search_hierarchy_2x.png)

应用程序可以通过`UIFocusEnvironment`协议为默认的焦点视图的查找提供信息。设置初始焦点的时候，焦点引擎首先会向窗口请求优先获取焦点的视图。窗口将返回根视图控制器的`preferredFocusedView`。由于返回的是一个响应`UIFocusEnvironment`协议的`UIView`对象，因此焦点引擎会继续询问哪一个是需要优先获取焦点的视图。整个查找过程形成一条优先焦点视图的查找链。焦点引擎将最后那个视图作为默认的视图获取焦点（Next focused view）。

下面是焦点引擎查找的一个示例：

1. 焦点引擎询问窗口的`preferredFocusedView`，返回根视图控制器的`preferredFocusedView`。
2. 根视图控制器（Tab view controller）返回它的选择视图控制器的`preferredFocusedView`。
3. 选择视图控制器覆盖`preferredFocusedView`方法，返回某个特定的`UIButton`对象。
4. `UIButton`对象默认返回它自己，并且能够获取焦点的话，就会被焦点引擎选中问下一个焦点视图（获取焦点）。

当焦点更新到某个视图时，新的焦点视图被设置为该视图焦点查找链最深的视图。另外一个例子是：当一个视图控制器被作为模态视图弹出到当前焦点视图上时，焦点呗设置到新的视图控制器上。

####焦点更新

当用户移动焦点的时候（如滑动遥控器），应用程序会请求更新焦点或者系统触发一个自动的更新。

####焦点更新剖析

当焦点更新或者移动到一个新的视图时，决定是选择当前视图的子视图还是另外一个视图的条件。

- `focusedView`属性被更新为新的焦点视图或者它的优先焦点视图。
- 焦点引擎通过调用`didUpdateFocusInContext:withAnimationCoordinator:`通知包含前一个焦点视图和下一个焦点视图的所有焦点环境，然后使用动画协调器调度焦点相关的动画。见[《UIFocusAnimationCoordinator》](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UIFocusAnimationCoordinator_Class/index.html#//apple_ref/occ/cl/UIFocusAnimationCoordinator)。
- 通知所有相关的焦点环境后，所有被协调的动画都同时执行。
- 如果下一个焦点视图在一个滚动视图里，并且处于屏幕外面，滚动视图将会通过滚动内容使下一个焦点视图移动到屏幕上。

####系统产生的焦点更新动作

在许多环境下，必要的时候UIKit会自动更新焦点。下面是焦点自动更新的一些示例：

- 焦点视图被移除。
- `UITableView`或`UICollectionView`重新加载数据。
- 弹出一个新的视图控制器。
- 用户按**Menu**按钮返回。

####程序更新焦点



####在应用中支持焦点

####在视图控制器中支持焦点

####在CollectionView和TableView中支持焦点

####在自定义视图中支持焦点

####焦点相关动画

####调试焦点问题

####为什么焦点移动出错

