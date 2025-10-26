# 着色器类

在 Unity 中，当您使用属于图形管道一部分的着色器时，通常会使用 Shader 类的实例。 Shader类的实例称为Shader对象。

Shader 对象是 Unity 特定的使用着色器程序的方式；它是着色器程序和其他信息的包装。**它允许您在同一文件中定义多个着色器程序**，并告诉 Unity 如何使用它们。


In Unity, when you work with shaders that are part of the graphics pipeline, you usually work with instances of the Shader class. An instance of the Shader class is called a Shader object.

A Shader object is a Unity-specific way of working with shader programs; it is a wrapper for shader programs and other information. It lets you define multiple shader programs in the same file, and tell Unity how to use them.



## 着色器对象基础知识

Shader 对象包含着色器程序、更改 GPU 设置的指令（统称为渲染状态）以及告诉 Unity 如何使用它们的信息。

您可以使用 Shader 对象和材质来确定场景的外观。



## 资产

您可以通过两种方式创建 Shader 对象。每个都有自己的资产类型：

* 您可以编写代码来创建着色器资源，它是一个扩展名为 .shader 的文本文件。
* 您可以使用 Shader Graph 创建 Shader Graph 资源。

无论您以哪种方式创建 Shader 对象，Unity 在内部都会以相同的方式表示结果。



## 在 Shader 对象内部

Shader 对象具有嵌套结构。它将信息组织成称为 SubShaders 和 Pass 的结构。它将着色器程序组织成着色器变体。

#### Shader object

一个 Shader 对象包含：

* 有关其自身的信息，例如其名称
* 一个可选的 fallback Shader 对象，如果 Unity 无法使用该对象，则使用该对象
* 一个或多个 SubShaders

您还可以定义其他信息，例如共享着色器代码，或是否使用自定义编辑器。有关定义 Shader 对象的信息，请参阅 ShaderLab：定义 Shader 对象。

#### SubShaders

**SubShaders 允许您将 Shader 对象分成与不同硬件、渲染管道和运行时设置兼容的部分**。

子着色器包含：

* 有关此 SubShader 与哪些硬件、渲染管道和运行时设置兼容的信息
* SubShader 标签，它们是提供有关 SubShader 信息的键值对
* 一张或多张 Passes

#### Passes

Pass包含：

* Pass Tags，是提供有关 Pass 信息的键值对
* 在运行着色器程序之前更新渲染状态的说明
* 着色器程序，组织成一个或多个着色器变体

#### Shader variants

Pass 包含的着色器程序被组织成着色器变体。

着色器变体共享通用代码，但在启用或禁用给定关键字时具有不同的功能。

Pass 中着色器变体的数量取决于您在着色器代码中定义的关键字数量以及目标平台。每个Pass至少包含一个变体。

