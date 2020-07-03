![WWDC2020.png](https://upload-images.jianshu.io/upload_images/2257417-ce6f154031ef9677.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **WWDC2020** 大会将通过 `Apple Developer App` 和 `Apple Developer`网站免费向所有开发者开放! 从去年火遍全网的 `SwiftUI` 以及 `Combine` 到今年全新的“Swift Student Challenge” 无时不刻都在透露 Swift 时代已经来临


本人也是`Swift`一个深度着迷的开发者，并且有点喜欢玩底层！ 非常感谢苹果爸爸 `Swift` 高度开源, 但是日常开发过程中总感觉还是缺了一点什么...

```
其实说白了要是能够我不能懂的底层，直接跑进源码看看流程，分析一下原理！这就完美了... 所以我毫不犹豫：Swift源码编译走起
```

着急尝鲜的小伙伴，请点击我的快速通道：[Swift源码编译](https://github.com/LGCooci/KCBuildSwiftSource.git)

### 一、Swift编译准备

* [apple/swift 官网地址](https://github.com/apple/swift) 这里clone我们需要编译的源码 
* 版本准备：**macOS 10.15.3  Xcode 11.5** (这是我当前的版本，应该是只需要 Xcode 11.2)
* 这里最新的源码编译时需要 Xcode 12.0  我本人现在没有升级，况且还只是beta 就不去玩，毕竟还要一段时间！以防不稳定
* 由于编译速度还是有点慢，建议电脑性能稍微高一点，具体你们自己定
* 网络建议：小楼梯 （不解释），稳定网线 
* 最重要的就是：**keep patient** (肯定会有各种问题报错，还是耗时非常严重：1-2h 这是正常现象)

### 二、开始编译吧

首先编译的手法有很多：Xcode - Ninja - Linux - VScode ！各有各的方便，这里我们不去说其他的先来一个大家非常熟悉的：Xcode

#### 1：准备编译目录

创建一个你喜欢的文件目录
```
mkdir swift-source
cd swift-source
```

#### 2：clone 源码

这里用的是 `swift-5.2.4-RELEASE` 这个稳定版本，对于现在开发来说够了！未来更新再说，请记住一定要根据我这个版本，因为版本不一样会和你Xcode不兼容，后面报错，我可就不负责了：哈哈哈哈
```
git clone --branch swift-5.2.4-RELEASE
https://github.com/apple/swift.git
```

* 这里如果你有小楼梯，应该很快的

![](https://upload-images.jianshu.io/upload_images/2257417-9fae1cae867dac1e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3：clone 补充核验

* 跳到swift文件下面 `utils/update-checkout`
* clone 出后面编译需要的文件
* 这很重要，因为update-checkout 将检出Swift源目录旁边的存储库
* 这一步也是比较耗时的，这个时候你可以：`Have a cup of coffee`

```
./swift/utils/update-checkout --tag swift-5.2.4-RELEASE --clone
```

![](https://upload-images.jianshu.io/upload_images/2257417-77d6ee3da429288a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4：buid Swift （waiting）

* 利用swift源码中的脚本编译

```
sudo ./swift/utils/build-script -x -R --debug-swift
```

* 苹果github上面也指出了几个编译实例，大家也可以自己去玩！
* 如果你编译还不是很熟练，也想避免踩坑（毕竟这一踩就要1-2h） 跟我给你的步骤走，安全又可靠！

```
utils/build-script --release-debuginfo --debug-swift # Swift frontend built in debug
utils/build-script --release-debuginfo --debug-swift-stdlib # Standard library built in debug
utils/build-script --release-debuginfo --debug-swift --force-optimized-typechecker # Swift frontend sans type checker built in debug
```

当然也会有一些玩得好的，想要更多参数命令，推荐这个脚本查阅
`./swift/utils/build-script -h` 由于篇幅原因不展开，大家可以自行去玩！比如说编译标准库和编译LLDB以及全部 等等。。。

![image](https://upload-images.jianshu.io/upload_images/2257417-252e3f3d1040cf9b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

构建完上一步，就会进入非常漫长难受的等待过程！如果觉得无聊可以趁这个时间去看看我其他博客：[Cooci_和谐学习——不急不躁](https://juejin.im/user/5c3f3c415188252b7d0ea40c/posts)

### 三、调试Swift源码

要在 `Xcode` 中打开 `Swift 项目`，请打开`/swift-source/build/Xcode-ReleaseAssert+swift-DebugAssert/swift-macosx-x86_64/Swift.xcodeproj`。它将为所有可用目标自动创建很多方案。常见的调试流程将涉及：

* 选择 `swift` scheme。
* 调出 `scheme` 编辑器（`⌘⇧<`）。
* 选择 `Arguments` 选项卡，然后单击 `+`。
* 添加命令行选项。
* 关闭`scheme` 编辑器。
* 编译并运行。

> 另一个选择是将方案更改为 `Wait for executable to be launched`，然后在终端中运行构建产品。

**到目前为止，基本操作就完毕了，大家可以自由自在的畅玩在Swift的海洋，注意发量 ~ 哈哈哈~**


### 四、构建失败原因分析

* clone 失败大概率就是网络问题

* 确保使用正确版本的 `Xcode`。

* 如果您已更改 `Xcode` 版本，但仍然遇到与Xcode版本有关的错误，请尝试将传递 `--clean` 给 `build-script`。

* 当发布新版本的 `Xcode` 时，您可以通过传递 `--reconfigure` 选项来更新构建，而无需重新编译整个项目。

* 确保所有存储库都是上述 `update-checkout` 命令中最新的

### 感谢

[Swift源码编译](https://github.com/LGCooci/KCBuildSwiftSource.git)

* https://github.com/apple/swift
* https://lldb.llvm.org/index.html
* https://forums.swift.org/t/debug-local-stack-frame-variables-fails-failed-to-get-module-builtin-from-ast-context/19171/12
* https://github.com/apple/swift/blob/master/docs/DebuggingTheCompiler.rst#types-loghttps://github.com/apple/swift/blob/master/docs/DebuggingTheCompiler.rst#types-log
* https://www.polidea.com/blog/how-to-build-swift-compiler-based-tool-the-step-by-step-guide/
https://github.com/vadimcn/vscode-lldb/blob/master/MANUAL.md


> OK 这篇文章就先写到这里，大家可以先行去下载尝鲜，如果觉得还可以还请不要吝啬你的 `点赞和star`
