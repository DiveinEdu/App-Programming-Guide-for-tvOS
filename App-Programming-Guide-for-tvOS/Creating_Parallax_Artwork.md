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

UIKit在运行时支持两种不同的包含分层数据的格式。

##使用视差预览应用创建LSR图片

##使用Xcode创建LSR图片

##创建LCR图片

##使用视差图片