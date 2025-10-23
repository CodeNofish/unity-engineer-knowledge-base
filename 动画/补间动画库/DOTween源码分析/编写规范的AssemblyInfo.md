---
tags:
  - AssemblyInfo
---


```C#
using System.Reflection;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;

// 允许指定的友元程序集访问当前程序集的 internal类型和成员。
// 主要使用场景: 单元测试、Unity编辑器扩展、模块间内部协作。
[assembly: InternalsVisibleTo("DOTweenEditor")]
[assembly: InternalsVisibleTo("DOTweenPro")]
[assembly: InternalsVisibleTo("DOTweenProEditor")]

// 程序集显示名称
[assembly: AssemblyTitle("DOTween")]
// 程序集描述
[assembly: AssemblyDescription("高效的Unity补间动画库")]
// 指定程序集的生成配置，区分Debug/Release等不同构建版本，用于条件编译和部署管理
// Debug Release Development Production Testing
[assembly: AssemblyConfiguration("Release")]
// 开发公司
[assembly: AssemblyCompany("Demigiant")]
// 产品名称
[assembly: AssemblyProduct("DOTween")]
// 版权信息
[assembly: AssemblyCopyright("Copyright © Daniele Giardini, 2014")]
// 商标，用于法律保护和品牌识别，如 DOTween™ 、Microsoft® .NET
[assembly: AssemblyTrademark("DOTween™")]
// 指定程序集支持的区域性，zh-CN、en-US
[assembly: AssemblyCulture("")]

// COM (Component Object Model):
// 是一种二进制接口标准，允许不同编程语言编写的组件相互通信。
// .NET框架与COM组件之间的桥梁技术，让托管代码（如C#）能够调用非托管的COM组件。

// Windows注册表中使用Guid标识COM组件，可以用生成网站
[assembly: Guid("27F1777E-C441-201B-3489-E1589CDD1311")]
// ComVisibleAttribute 控制.NET类型和成员是否对COM客户端可见。
// Unity通常不需要COM互操作
[assembly: ComVisible(false)] 

// 主版本.次版本.修订号.构建号
// CLR使用的版本号，用于程序集绑定
[assembly: AssemblyVersion("1.0.0.0")]
// 文件版本号，显示在文件属性中
[assembly: AssemblyFileVersion("1.0.0.0")]
```