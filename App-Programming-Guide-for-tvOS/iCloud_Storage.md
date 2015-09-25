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

Apple TV的存储空间非常有限，并且不保证存储在设备上的数据在用户下次打开程序的时候还继续存在。因此为了在多个设备上分析用户的数据，我们需要将用户的数据存储的其它地方。苹果给我们提供了两个选择：iCloud键值存储和CloudKit。

当存储空间小于1MB的时候，我们可以选择iCloud KVS。iCloud KVS自动在不同设备之间进行同步。只有应用拥有者才能访问存储在iCloud KVS上的数据。应用的其它用户不能够访问这些信息。更多信息，建“[设计iCloud中的键值数据](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7)”。

如果需要存储更多数据，比如说超过1MB，我们需要在应用中实现CloudKit。CloudKit允许一个用户存储的数据被另一个用户访问。这个在一个用户的行为能够影响都另外的用户时非常有用。比如游戏中一个用户的动作能够直接影响另外的用户。更多关于在应用中实现CloudKit的信息，见”[Cloud Quick Start](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987)“。