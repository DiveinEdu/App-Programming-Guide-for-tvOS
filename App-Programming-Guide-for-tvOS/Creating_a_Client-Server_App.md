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


##创建一个C/S应用

与其它类型的应用稍有不同，C/S应用是使用JavaScript和TVML开发的。Xcode应用的主要功能是访问主JavaScript文件和将从TVML文件创建的页面展示到屏幕上。图2-1展示了C/S模型。

**图2-1** C/S模型
![C/S模型](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/flow_diagram_2x.png)

我们的JavaScript文件将TVML页面加载到内存并且压入导航栈中。当用户在应用中导航时，TVML页面会被出栈入栈。当用户关闭应用后，Apple TV显示主屏幕。图2-2显示了基本的工作流程。

**图2-2**C/S应用工作流程
![C/S应用工作流程](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/flow_diagram2_2x.png)

创建C/S应用的步骤：
1 . 打开Xcode并且创建项目。
2 . 选择tvOS的**Single View Application**模版。
3 . 删除View Controller文件和Storyboard。
4 . 打开**info.plist**并且删除**Main storyboard**入口。

>设置网络安全，见[《App Transport Security Technote》](https://developer.apple.com/library/prerelease/tvos/technotes/App-Transport-Security-Technote/index.html#//apple_ref/doc/uid/TP40016240)。

5. 修改**AppDelegate.swift**文件：

- 添加`import TVMLKit`。
- 将类的声明修改为`class AppDelegate: UIResponder, UIApplicationDelegate, TVApplicationControllerDelegate {`。
- 添加变量：`var appController: TVApplicationController?`。
- 修改`application:didFinishLaunchingWithOptions:`方法：

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    self.window = UIWindow(frame:UIScreen.mainScreen().bounds)
    
    let appControllerContext = TVApplicationControllerContext()
    
    let javascriptURL = NSURL(string: String("JavaScript文件路径"))
    
    appControllerContext.javaScriptApplicationURL = javascriptURL!
    if let options = launchOptions
    {
        for (kind, value) in options
        {
            if let kindStr = kind as? String
            {
                appControllerContext.launchOptions[kindStr] = value
            }
        }
    }
    
    self.appController = TVApplicationController(context: appControllerContext, window: self.window, delegate: self)
    
    return true
}
```

6 . 第一个例子中的代码会将一个TVML页面加载并显示到模拟器或者电视机屏幕上（Apple TV连在电脑上）。更多关于JavaScript类的信息，见[《TVJS  Framework Reference》](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html#//apple_ref/doc/uid/TP40016076)。

```js
function getDocument(url) {
    var templateXHR = new XMLHttpRequest();
    templateXHR.responseType = "document";
    templateXHR.addEventListener("load", function() {pushDoc(templateXHR.responseXML);}, false);
    templateXHR.open("GET", url, true);
    templateXHR.send();
    return templateXHR;
}

function pushDoc(document) {
    navigationDocument.pushDocument(document);
}

App.onLaunch = function(options) {
    var templateURL = 'Enter path to your server here/alertTemplate.tvml';
    getDocument(templateURL);
}

App.onExit = function() {
    console.log('App finished');
}
```
上面的JavaScript加载了一个显示警告的TVML页面，询问用户是否需要将应用升级为高级版本（增值版本）。下面是警告模版的代码。更多关于TVML的信息，见[《Apple TV Markup Language Reference》](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html#//apple_ref/doc/uid/TP40015064)。[中文版](https://github.com/DiveinEdu/Apple-TV-Markup-Language-Reference-in-Chinese)

```xml
<document>
   <alertTemplate>
      <title>Update to premium</title>
      <description>Go add free by updating to the premium version</description>
      <button>
         <text>Update Now</text>
      </button>
      <button>
         <text>Cancel</text>
      </button>
   </alertTemplate>
</document>
```