# GN_mini1.0
/*------------------------------------- !! GN_mini引擎使用声明 !! ----------------------------------------\
|                                                                                                        |
|	本引擎精简版源码是个采用MIT协议开源免费,超轻量,依赖少,配置要求低,优化到位,可定制式样,                         |
| 动效丰富且跨平台的实时GUI和2D引擎运行库	                                                                 |
|	下面的一些模块定义列举了当前版本所依赖到的一些开源库, 如果您不需要特定功能您可以剪裁它以使它更轻量              |
|                                                                                                        |
|	本引擎主要依赖到的其它库有: OpenGL, GLUT, SDL, libpng, libzip, OpengAL(播放声音用), FFmpeg.(播放视频用)    |
|	如在使用过程中遇到问题, 或有何好建议欢迎加技术QQ群 152670375 交流, 如想了解更多使用例子或更详细的文档可以登陆: |
|	www.gncreate.com 或 www.img-power.com 查阅, 或下载此引擎的可视化UI编辑器!                                 |
|                                                                                                        |
|	引擎功能简介Demo视频: https://www.bilibili.com/video/BV1qhm3BmE9a                                       |
|                                                                                                        |
\-------------------------------------------------------------------------------------------------------*/


本项目目录文件介绍:

<GN_system>         目录 为引擎依赖项! 包含基础UI和默认字体,默认文件图标等, 您所开发的应用必须包含它
<项目跟>            目录 一般而言您所编译好的动态库和可执行文件应放在这! 因为示例里指定的路径以此开始
<TempProject>       目录里有个图为开机LOGO, 您可以自由替换成您喜欢的
<project/scr>	    目录为 GN_mini 源码目录
<project/external>	目录为外部依赖到的开源库, 里面有头文件和一些帮您预编译好几个平台的动态库, 
                    当您想调试运行某个平台的程序时, 您需要把 external/lib/对应平台里的动态库复制到项目跟目录里
<project/build>     目录里有编译各平台应用程序的工程文件

 GN_mini.sln        用于编译Windows平台程序的工程 需用Visual Studio2019及以上版本打开它, 里面有两个项目, GNwin32编译出GN_mini.dll,
                    GNapp编译出可执行文件GNapp.exe 编译本项目您至少要有如图(VS2019需安装工具.png)所列出的安装项
 
 GNx64.xcodeproj    用于编译Macosx平台程序的工程, 由Xcode8生成, 您需要安装它, 用于编译出 libGNx64.dylib 引擎动态库
 GN_Test.xcodeproj  用于编译出 Test.app 在Macosx平台里执行的程序
 
 <.vscode>          里有 Linux 或 Android 的编译工程, 可用Vscode 打开
 
<Sample>            目录里的为示例代码 你可以把里面 main.cpp 和其中一个sample0?.cpp 加到编译可执行程序的工程里编译生成示例执行文件
                    在编写或修改示例代码时, 其它那些Window平台的API, UI, 组件, 类等可用可不用, 
                    但如果您希望您所开发的应用能够全平台运行, 引用外部的库最好是开源或各平台都有分发版的!
   
<project/scr/GN.h>  引擎功能总目录文件 该文件下边列出了很多子头文件, 都有包含相关功能的介绍, 如需要再查看更详细的, 就进入对应子头文件查阅即可
   
   
   
编译指定平台的应用程序:
找到 project/scr/_SysDef/GN_SysDef.h 文件打开它
文件顶部有各个平台对应框架的编译路径, 把您想要编译的路径宏放出来 其它注释掉, 就可以执行该平台的程序编译
其中有★的为支持得比较完善的分支, Windows平台下还支持 DirectX9 和 DirectDraw 分支, 但这两只支持32位程序
多给几个图形API分支你, 以便了解到项目是如何做跨平台接口转接的! 当然您也可以将图形接口改成DirectX12 图形模块的代码都在 src/_Graphics 目录里

此外在编译为Windows平台, 如您想把动态库拆分多个或合并成1个exe, 搬动代码文件到其它项目工程时, 需要在编译预处理定义里添加 GN_???_USING_LIB_EXPORTS 系列对应的宏 
涉及到平台模块目录的代码都有对应的宏 比如src/_Sys目录里的代码要跟 GN_SYS_USING_LIB_EXPORTS 宏, src/_Main 要跟 GN_MAIN_USING_LIB_EXPORTS 宏
平台有相关的模块代码通常放在有 _开头的目录, 如合并成1个exe无动态库的编译, 您不需要定义 GN_?????_LIB_EXPORTS 系列宏, 而是定义 GN_???_USING_SOURCE_CODE 系列的宏
这个特殊的宏 GN_USING_SDK_INTEGRATION 用于给exe端仅使用1个整合的GN_mini.dll动态库时需要指定的 不指定这个,编译时会默认引擎一个模块对应一个dll

有关Vscode版工程编译工具链的配置和Android版编译打包请参考这里
http://www.img-power.com/tutorial/platform_exp/android/build.html



如何剪裁部分功能使包体变小:
打开GN.h头文件找到 #内嵌功能模块编译配置信息 这栏下面就有很多子功能模块对应的宏, 把不想要的注释掉重新编译项目即可
如您无需播放视频功能, 不想依赖FFmpeg这个大库, 就把项目里的编译文件引用 GN_Media.cpp 移除再重新编译项目即可



如何适配到更多其它平台:
首先您需定义一个平台框架编译路径宏用以区分, 其次可能要修改或添加 _Main, _Sys, _Graphics, _Sound 目录里的文件
_Graphics图形接口和_Sound音频接口用到的OpenGL和OpenAL版本较低, 通常所有平台均有保底支持, 预装在系统里, 找到它编译时连接即可, 无的话您也可以到官方下载源码自己编译出动态库
_Sys 有共同性的系统API接口, 这里您只需要实现目标系统缺失或同功能命名有差异的API转换代码 另外有些系统中文路径是UTF-8编码的, 您需要实现 GN_StrToSysCode 函数里做 UTF-8 转 GB2312编码

_Main平台框架接口为主要修改项:
 由于该模块为 引擎运行框架, 和引擎有更多的接合,相较其它模块来说在开发上需留意比较多的东西, 比如对引擎内部的调用会多一些!
 和对 GNg::sWINDOW 结构的静态成员变量的状态检阅和设置 对 GT_SystemInited 加入已经初始化好某功能的位标记
 关于 引擎提供给系统模块调用的东西都以 GF_... 开头.

 大致步骤如下:
 你先定义一组宏来引导特定平台的入口能调用到 引擎代码
 #define GN_MAIN
  //使 GN 应用程序执行代码由这里开始
 #define GN_EXIT

 在执行引导到 GN_MAIN 这里前 你需指定系统所分配给当前运行程序的标识和调用参数给 引擎 例如: GF_AppArgsSet((int*)程序的标识, 调用参数...);
 如果你所开发的系统有文件路径访问限制或访问文件不能用运行应用程序文件下的相对路径时, 你必需手动调用 GF_AppCurPathChange 来通知引擎更变程序访问路径

 在实现本模块接口:GN_InitGraphics 里需调用检测下图形系统是否OpenGL  if(strncmp(GN_GetInterfaceInfoRender(),"OpenGL",6)), 如是OpenGL的话,
 你需在这为OpenGL使用窗口创建一块绘图缓冲.
 但无论是不是OpenGL你都应该在这调用 GF_InitGraphics 来告知 引擎绘图所使用的窗口和其它一些窗口信息!

 实现本模块接口:GN_GameLoop  你需负责控制好程序不断运行的帧循环! 这个处理不好会使引擎的性能大打节扣, 耗尽CPU性能! 必需要认真处理好这点.
 刚进入还未到循环前你需调用     GF_FrameCycleBeginProc(); 这里面通常是由引擎已经整理好需要初始化内部的一大堆东西
 同样在程序退出循环后你需调用   GF_FrameCycleEndProc();   这里面通常是由引擎已经整理好需要释放内部空间的一大堆东西
 在进入循环中的每帧你都需要调用 GF_FrameCycleProc1(...) 和 GF_FrameCycleProc2(...); 调用顺序不能搞乱, 这里面通常是由引擎已经整理好要在每帧都运行更新的内容, 如图形渲染
 在每帧的结束前你还需要调用 本模块接口的GN_ScreenUpData 以把窗口渲染好的内容提交到前台屏幕.
 在实现GN_ScreenUpData里, 你可先调用GF_ScreenUpData 看看引擎图形模块是否已经有完成该功能, 没有的话你就需要实现该功能, 通常OpenGL图形渲染都是靠系统来提交更新内容的.
 在每帧循环里你还需要做一些时间的控制如调用GF_Sleep 让程序暂时休息一回以节省CPU和接收系统消息内容. 具体你可参考本例里的 GN_GameLoop 实现

 在运行时接收到系统消息后你必需按这些消息来对 引擎的输入状态数据进行更新, 引擎所有需要更新状态的主要调用都在 GN_Input.h 头文件里的 输入状态更改通知 一栏所列出
 根据消息类型来调用相关的 引擎更改状态函数 具体你可参考本例里的 WndProc 实现

 最后你在实现本模块接口:GN_UnInitGraphics 里也需调用 GF_UnInitGraphics 来一起对 引擎中图形的一些东西做释放

 总结一下要调用到 引擎里的东西:

	//开始执行时
    GF_AppArgsSet(...)
	GF_AppCurPathChange(...)

    //初始化时
	GF_InitGraphics(...)

	//帧循环运行时
	GF_FrameCycleBeginProc()
    GF_FrameCycleProc1(...)
	GF_FrameCycleProc2(...)
    GF_FrameCycleEndProc()
    GF_Sleep()
    GF_ScreenUpData()

	//接收到消息信号时
    GF_ReSetInputState(...)
    GF_SetMouseState(...)
    GF_SetMouseMoveState(...)
    GF_SetTouchState(...)
    GF_SetTouchMoveState(...)
    GF_SetKeyState(...)
    GF_SetInFvState(...)
    GF_SetInPortState(...)
    GF_ReSetViewport(...)

	//退出帧循环时
    GF_UnInitGraphics()

    相关的时候设置和使用它
	GT_SystemInited

    GNg::sWINDOW 结构的静态成员变量
    
    
感谢您阅读到这里, 欢迎各位有才之士能把引擎移植适配到更多平台, 在此也希望您能把这部分代码回馈给社区普度众生!
