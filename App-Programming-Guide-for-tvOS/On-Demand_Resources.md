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

按需加载资源是指应用程序的内容被托管在App Store，而不是存放在我们下载的应用程序包中。它们能够缩减应用程序包的大小、加快下载和丰富程序内容。应用程序请求一组按需加载的资源，操作系统负责管理下载和存储过程。应用程序使用资源，然后释放请求。在下载完成后，这些资源会在设备上存储多个启动周期，从而加快访问速度。

每个存储在Apple TV上的应用程序大小都被限制在200MB以内。为了创建一个大于这个数目的应用，我们必须将应用拆分为多个可下载的包。在Xcode中创建标签并将它们附加到必须的资源上。当我们的应用请求附加了标签的资源后，操作系统会自动下载请求的数据。我们必须等待下载完成后才能在应用中使用。

这些资源集被分为可以管理的组。比如我们将游戏第五关的资源放到一个标签下。确保在下载资源的时候要给用户UI上的提示。测试我们的应用，来找到合适的可以下载文件大小。更多关于按需请求资源，见“[On-Demand Resources Guide](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)”。