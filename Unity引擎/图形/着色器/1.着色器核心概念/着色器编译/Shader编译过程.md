
# 着色器编译

## Overview 概述

Every time you build your project, the Unity Editor compiles all the shaders that your build requires: every required shader variant, for every required graphics API.

When you’re working in the Unity Editor, the Editor does not compile everything upfront. This is because compiling every variant for every graphics API can take a very long time.

Instead, Unity Editor does this:
* When it imports a shader asset, it performs some minimal processing (such as Surface Shader generation).
* When it needs to show a shader variant, it checks the Library/ShaderCache folder.
* If it finds a previously compiled shader variant that uses identicial source code, it uses that.
* If it does not find a match, it compiles the required shader variant and saves the result to the cache.
	* Note: If you enable Asynchronous shader compilation, it does this in the background and shows a placeholder shader in the meantime.

每次构建项目时，Unity 编辑器都会编译构建所需的所有着色器：每个所需的着色器变体，以及每个所需的图形 API。

当您在 Unity 编辑器中工作时，编辑器不会预先编译所有内容。这是因为编译每个图形 API 的每个变体可能需要很长时间。

相反，Unity 编辑器会执行以下操作：
* 当它导入着色器资源时，它会执行一些最小的处理（例如表面着色器生成）。
* 当需要显示着色器变体时，它会**检查 Library/ShaderCache 文件夹**。
* 如果它发现先前编译的着色器变体使用相同的源代码，则会使用该变体。
* 如果找不到匹配项，它将编译所需的着色器变体并将结果保存到缓存中。
	* 注意：如果启用异步着色器编译，它会在后台执行此操作，同时显示占位符着色器。

Shader compilation is carried out using a process called UnityShaderCompiler. Multiple UnityShaderCompiler processes can be started (generally one per CPU core in your machine), so that at player build time shader compilation can be done in parallel. While the Editor is not compiling shaders, the compiler processes do nothing and do not consume computer resources.

着色器编译是使用称为 UnityShaderCompiler 的进程进行的。
可以启动多个UnityShaderCompiler 进程（通常是计算机中的每个 CPU 核心一个进程），以便在玩家构建时可以并行完成着色器编译。
当编辑器不编译着色器时，编译器进程不执行任何操作，也不消耗计算机资源。


The shader cache folder can become quite large, if you have a lot of shaders that are changed often. It is safe to delete this folder; it just causes Unity to recompile the shader variants.

如果您有大量经常更改的着色器，则着色器缓存文件夹可能会变得相当大。删除该文件夹是安全的；它只会导致 Unity 重新编译着色器变体。

At player build time, all the “not yet compiled” shader variants are compiled, so that they are in the game data even if the editor did not happen to use them.

在玩家构建时，所有“尚未编译”的着色器变体都会被编译，因此即使编辑器没有碰巧使用它们，它们也会出现在游戏数据中。

---

## Different shader compilers 不同的着色器编译器

Different platforms use different shader compilers for shader program compilation as follows:
* Platforms that use DirectX use Microsoft’s FXC HLSL compiler.
* Platforms that use OpenGL (Core & ES) use Microsoft’s FXC HLSL compiler, followed by bytecode translation into GLSL using HLSLcc.
* Platforms that use Metal use Microsoft’s FXC HLSL compiler, followed by bytecode translation into Metal, using HLSLcc.
* Platforms that use Vulkan use Microsoft’s FXC HLSL compiler, followed by bytecode translation into SPIR-V, using HLSLcc.
* Other platforms, such as console platforms, use their respective compilers.
* Surface Shaders use HLSL and MojoShader for code generation analysis step.
You can configure various shader compiler settings using pragma directives.

```ad-note
都在用 FXC HLSL 编译器，编译代码，然后通过字节码翻译 转到GL Vulkan这种平台。
这也是为什么 unity推荐用HLSL写Shader的原因之一。
```


不同平台使用不同的shader编译器进行shader程序编译，如下：
* 使用 DirectX 的平台使用 Microsoft 的 FXC HLSL 编译器。
* 使用 OpenGL（Core 和 ES）的平台使用 Microsoft 的 FXC HLSL 编译器，然后使用 HLSLcc 将字节码转换为 GLSL。
* 使用 Metal 的平台使用 Microsoft 的 FXC HLSL 编译器，然后使用 HLSLcc 将字节码转换为 Metal。
* 使用 Vulkan 的平台使用 Microsoft 的 FXC HLSL 编译器，然后使用 HLSLcc 将字节码转换为 SPIR-V。
* 其他平台，例如控制台平台，使用各自的编译器。
* Surface Shaders 使用 HLSL 和 MojoShader 进行代码生成分析步骤。

您可以使用 pragma 指令配置各种着色器编译器设置。

---


## The Caching Shader Preprocessor 缓存着色器预处理器

Shader compilation involves several steps. One of the first steps is preprocessing. During this step, a program called a **preprocessor** prepares the shader source code for the compiler.

In previous versions of Unity, the Editor used the preprocessor provided by the shader compiler for the current platform. Now, Unity uses its own preprocessor, also called the Caching Shader Preprocessor.

The Caching Shader Preprocessor is optimized for faster shader import and compilation. It works by caching intermediate preprocessing data, so the Editor only needs to parse include files when their contents change, which makes compiling multiple variants of the same shader more efficient.

For detailed information on the differences between the Caching Shader Preprocessor and the previous behavior, see the Unity forum: [New shader preprocessor](https://forum.unity.com/threads/new-shader-preprocessor.790328/).

```ad-note
Unity在用新的 Caching Shader Preprocessor，存储了中间生成的代码和Shader元数据，可以只针对发生变化的代码片段编译，优化了编译效率。
```

着色器编译涉及几个步骤。第一步是预处理。在此步骤中，称为预处理器的程序为编译器准备着色器源代码。

在以前版本的 Unity 中，编辑器使用着色器编译器为当前平台提供的预处理器。现在，Unity 使用自己的预处理器，也称为缓存着色器预处理器。

缓存着色器预处理器针对更快的着色器导入和编译进行了优化。它通过缓存中间预处理数据来工作，因此编辑器只需要在内容发生更改时解析包含文件，这使得编译同一着色器的多个变体更加高效。

有关缓存着色器预处理器与之前行为之间差异的详细信息，请参阅 Unity 论坛：新着色器预处理器。


---


## AssetBundles and shaders 资源捆绑包和着色器

If you use [AssetBundles](https://docs.unity.cn/2023.2/Documentation/Manual/AssetBundlesIntro.html), Unity might compile duplicate shaders if you reference one shader in two or more objects. For example:

* A material in an AssetBundle and a material in a built scene reference the same shader.
* Multiple AssetBundles contain materials that reference the same shader outside an AssetBundle.

This can increase the memory and storage space shaders use, and break [draw call batching](https://docs.unity.cn/2023.2/Documentation/Manual/DrawCallBatching.html).

To avoid this, you can use the following approaches:

* Load an AssetBundle that contains all your shaders first, then load and instantiate AssetBundle assets that reference the shaders. See [AssetBundle Dependencies](https://docs.unity.cn/2023.2/Documentation/Manual/AssetBundles-Dependencies.html) for more information.
* Structure your AssetBundles to minimise duplication. See [Asset Duplication](https://docs.unity.cn/2023.2/Documentation/Manual/AssetBundles-Troubleshooting.html#asset-duplication) for more information.

You can add materials and [shader variant collections](https://docs.unity.cn/2023.2/Documentation/Manual/shader-variant-collections.html) to an AssetBundle to specify which shader variants to include.

If you create a single AssetBundle, some shaders might stay in memory even if they’re no longer needed, because you cannot partially unload an AssetBundle. You can avoid this by creating a separate AssetBundle for each group of shaders you use together, for example a ‘forest’ AssetBundle and a ‘desert’ AssetBundle. See [Managing loaded AssetBundles](https://docs.unity.cn/2023.2/Documentation/Manual/AssetBundles-Native.html#managing-loaded-assetbundles), or [Memory management in the Addressables system](https://docs.unity.cn/Packages/com.unity.addressables@1.21/manual/MemoryManagement.html#understanding-when-memory-is-cleared) if you use Addressables.

You can use the [Asset Bundle Browser](https://docs.unity.cn/2023.2/Documentation/Manual/AssetBundles-Browser) to check which assets in AssetBundles depend on other assets, and find out if any assets are duplicated.

```ad-note
打AB包时，打包Shader 需要注意的事项。

**打包问题**
不同AB包 引用相同Shader时，Shader可能会重复编译。
解决方案，先加载着色器包，再加载资源包，AB包中不要包含重复Shader。

**卸载包问题**
AB包粒度大时，Shader不使用时，还会占用内存。
可以把AB包粒度小一些，和模型场景绑定，随时卸载。

```

如果您使用 AssetBundles，并且在两个或多个对象中引用一个着色器，则 Unity 可能会编译重复的着色器。例如：

* AssetBundle 中的材质和构建场景中的材质引用相同的着色器。
* 多个 AssetBundle 包含在 AssetBundle 外部引用相同着色器的材质。

这可以增加着色器使用的内存和存储空间，并打破 DrawCall批处理。

为了避免这种情况，您可以使用以下方法：

* 首先加载包含所有着色器的 AssetBundle，然后加载并实例化引用着色器的 AssetBundle 资源。有关更多信息，请参阅 AssetBundle 依赖项。
* 构建您的 AssetBundle 以最大程度地减少重复。有关详细信息，请参阅资产复制。

您可以将材质和着色器变体集合添加到 AssetBundle 以指定要包含哪些着色器变体。

如果您创建单个 AssetBundle，某些着色器即使不再需要，也可能会保留在内存中，因为您无法部分卸载 AssetBundle。您可以通过为一起使用的每组着色器创建单独的 AssetBundle 来避免这种情况，例如“森林”AssetBundle 和“沙漠”AssetBundle。请参阅管理加载的 AssetBundles，或者如果您使用 Addressables，请参阅Addressables 系统中的内存管理。

您可以使用Asset Bundle Browser来检查AssetBundles中哪些资源依赖于其他资源，并找出是否有重复的资源。

---

## Build time stripping 构建时间剥离

While building the game, Unity can detect that some of the internal shader variants are not used by the game, and exclude (“strip”) them from build data. For more information, see Shader variants.

```ad-note
未被使用的着色器变体，在构建时会 被排除。
```

在构建游戏时，Unity可以检测到一些内部着色器变量未被游戏使用，并从构建数据中排除（“剥离”）它们。有关更多信息，请参阅着色器变体．