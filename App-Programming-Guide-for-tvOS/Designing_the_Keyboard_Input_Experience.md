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

##设计键盘输入体验

由于没有硬件键盘，Apple TV上的键盘输入会比较困难。我们需要花费更多的时间来确保一个愉快的用户体验。如果可以的话，尽量设计自己的UI不要过多的输入文字。Apple TV有两种类型的键盘：正常类型和内嵌类型。

##键盘输入

使用`UIAlertController`和`UITextField`创建自定义键盘和定制键盘输入体验，如E－Mail键盘和数字键盘。`UIAlertController`和`UITextField`的所有可用选项都包含在`UIKit/UITextInputTrailts.h`中。`inputAccessoryView`和`inputAccessoryViewController`允许我们自定义键盘。

##使用UIAlertController

键盘的UI完全由UIKit控制，但是通过使用`UIAlertController`可用添加一些按钮和文本输入框。我们可以在键盘的上方放置一些标题和说明文字。这种键盘会更加好用。但是用户还是不得不在遥控上点击多次。

##使用UITextField

使用`UITextField`可以在屏幕上放置一个全屏的键盘。用户通过`Next`和`Previous`按钮在文本框之间移动。这样用户就不需要总是重复移动焦点、点击的操作。在所有文本框都被填好后会出现一个“完成”（Done）按钮，让用户返回前一个页面。开发者可以将键盘放置在任何位置，它们都能接受前进/后退的操作。但是这个键盘的使用比`UIAlertController`要复杂，因为我们需要创建所有的视图，并且将它们放置在合适的位置。

##高级UITextField键盘

iOS上的第一响应者机制在tvOS上都能起作用。这个机制允许用户显示和隐藏一个界面，并且通过调用`becomeFirstResponder`方法让其中一个文本框称为第一响应者。成为第一响应者会自动弹出键盘界面，而不需要用户导航到文本框进行点击。当用户从键盘退出或者点击“完成”按钮时，通过第一响应者的回调函数获取不再接收键盘输入的文本框。

##内嵌UITextField输入

行内键盘在同一行内显示所有可能的输入结果。使用`UISearchController`创建一个可以和第三方内容完成整合的键盘。使用`UISearchController`的时候，还是有很少的自定义选项需要设置。我们不能再访问输入框本身、自定义输入特性以及添加附加的输入组件。