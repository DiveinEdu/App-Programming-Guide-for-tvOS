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

大多数UIKit的视图都会对用户的操作做出合适的反应，比如当用户在遥控上点击一个按钮，或者在触摸板上操作手势时。这跟iOS上的触摸事件一样。按钮点击事件在响应链上进行处理。`UIGestureRecognizer`和`UIResponder`类增加了新的方法来处理遥控上的按钮点击和释放。另外，Siri遥控的触摸板还支持拖动和滑动手势。

##使用手势识别器

点击手势识别器能够用来检测按钮的点击。默认情况下点击手势由**Select**按钮触发。`allowedPressTypes`属性能够指定触发某个手势的按钮类型。

**表 4-1** 创建一个由**Play/Pause**按钮出发的手势识别器

**表 4-1** 检测**Play/Pause**按钮（Swift/Objective-C）

```swift
let tapRecognizer = UITapGestureRecognizer(target: self, action:"tapped:")
tapRecognizer.allowedPressTypes = [NSNumber(integer: UIPressType.PlayPause.rawValue)];
self.view.addGestureRecognizer(tapRecognizer)
```

```objc
UITapRecognizer *tapRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self, action:@selector(tapped:)];
tapRecognizer.allowedPressTypes = @[@(UIPresstypePlayPause)];
[self.view addGestureRecognizer:tapRecognizer];
```

与此类似，滑动识别器和拖动识别器也可以用来检测遥控器触摸板上的事件。**表 4-2**演示了如何检测从左到右的滑动手势。

**表 4-2** 检测滑动手势

```swift
let swipeRecognizer = UISwipeGestureRecognizer(target: self, action:"swiped:")
swipeRecognizer.direction = .Right
self.view.addGestureRecognizer(swipeRecognizer)
```

```objc
UISwipeRecognizer *swipeRecognizer = [[UISwipeGestureRecognizer alloc] addTarget:self, action:@selector(swiped:)];
swipeRecognizer.direction = UISwipeGestureRecognizerDirectionRight;
[self.view addGestureRecognizer: swipeRecognizer];
```

##底层事件处理

tvOS使用`UIPress`对象替换`UITouch`来提供遥控器和其它输入设备的事件。`UIPress`对象描述了按钮对象以及它当前的状态。对于模拟按键，`UIPress`对象还会提供压力信息。`UIPress`对象使用`type`属性标识状态发生改变的物理按键，其它属性表示发生了哪些改变。

`UIGestureRecognizer`和`UIResponder`对象能够实现一些可以在触发按钮事件时被调用的方法。这些方法会接收到一个描述该事件的`UIPressesEvent`对象和一组状态发生改变的按钮。**表 4-3**显示了一个视图控制器如何为一个选择事件实现底层处理。和触摸事件的处理一样，如果我们需要处理按钮事件，就需要实现所有的四个按钮处理方法。

```swift
override func pressesBegan(presses: Set<UIPress>, withEvent event: UIPressesEvent?) {
    for item in presses {
        if item.type == .Select {
            self.view.backgroundColor = UIColor.greenColor()
        }
    }
}
 
override func pressesEnded(presses: Set<UIPress>, withEvent event: UIPressesEvent?) {
    for item in presses {
        if item.type == .Select {
            self.view.backgroundColor = UIColor.whiteColor()
        }
    }
}
 
override func pressesChanged(presses: Set<UIPress>, withEvent event: UIPressesEvent?) {
    // ignored
}
 
override func pressesCancelled(presses: Set<UIPress>, withEvent event: UIPressesEvent?) {
    for item in presses {
        if item.type == .Select {
            self.view.backgroundColor = UIColor.whiteColor()
        }
    }
}
```

```objc
- (void)pressesBegan:(NSSet<UIPress *> *)presses withEvent:(UIPressesEvent *)event {
    for(UIPress *item in presses) {
        if(item.type == UIPressTypeSelect) {
            self.view.backgroundColor = UIColor.greenColor()
        }
    }
}
 
- (void)pressesEnded:(NSSet<UIPress *> *)presses
           withEvent:(UIPressesEvent *)event {
    for(UIPress *item in presses) {
        if (item.type == UIPressTypeSelect) {
            self.view.backgroundColor = UIColor.whiteColor()
        }
    }
}
 
override func pressesChanged(presses: Set<UIPress>, withEvent event: UIPressesEvent?) {
    // ignored
}
 
- (void)pressesChanged:(NSSet<UIPress *> *)presses
             withEvent:(UIPressesEvent *)event {
    for (UIPress *item in presses) {
        if (item.type == UIPressTypeSelect) {
            self.view.backgroundColor = UIColor.whiteColor()
        }
    }
}
```