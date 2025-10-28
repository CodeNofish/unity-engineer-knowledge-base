
To ensure your application runs with no performance issues, it’s important to understand how Unity uses and allocates memory.

Unity uses the following memory management layers to handle memory in your application:

* Managed memory: A controlled memory layer that uses a managed heap and a garbage collector to automatically allocate and assign memory.
* C# unmanaged memory: A layer of memory management that you can use with the Unity.Collections namespace, the Collections package, and via `UnsafeUtility.Malloc` and `UnsafeUtility.Free`. This memory type is called unmanaged because it doesn’t use a garbage collector to track and manage when memory stops being used, but requires your code to explicitly manage and release the memory itself, by calling Dispose on the collection or Free on memory allocated with `UnsafeUtility.Malloc`.
* Native memory: C++ memory that Unity uses to run the engine. It contains memory related to areas such as memory used for the assets in your project, and managers for Unity’s different native subsystems like the rendering or animation systems. In most situations, you can’t access this memory via your C# code, but because it’s usually the biggest chunk of your application’s memory footprint, it’s useful to be aware of it if you want to optimize your application’s memory usage.

```ad-note

三个内存管理层：
1. C# Managed Memory，GC管理。
2. C# Unmanaged Memory，Unsafe创建 Unsafe释放。
3. Native Memory，引擎运行时的C++内存。

```


为了确保您的应用程序运行时不会出现性能问题，了解 Unity 如何使用和分配内存非常重要。

Unity 使用以下内存管理层来处理应用程序中的内存：

* Managed Memory: 受控内存层，使用托管堆和垃圾收集器自动分配和指派内存。
* C# Unmanaged Memory：可以与 Unity.Collections 命名空间、Collections 包以及通过 UnsafeUtility.Malloc 和 UnsafeUtility.Free 结合使用的内存管理层。这种内存类型称为非托管内存，因为它不使用垃圾收集器来跟踪和管理内存何时停止使用，而是要求您的代码通过调用集合上的 Dispose 或调用使用 UnsafeUtility.Malloc 分配的内存上的 Free 来显式管理和释放内存本身。
* Native Memory：Unity 用于运行引擎的 C++ 内存。它包含与项目中资源使用的内存等区域相关的内存，以及 Unity 不同本机子系统（如渲染或动画系统）的管理器。在大多数情况下，您无法通过 C# 代码访问此内存，但由于它通常是应用程序内存占用的最大块，因此如果您想优化应用程序的内存使用情况，了解它会很有用。

---

## Managed memory

Mono and IL2CPP’s scripting virtual machines (VM) implement the managed memory system, which is sometimes referred to as the scripting memory system. The VM offer a controlled memory environment divided into the following different types:

* The managed heap: A section of memory that the VM automatically controls with a garbage collector.
* The scripting stack: This is built up and unwound as your application steps into and out of any code scopes.
* Native VM memory: Contains memory related to Unity’s scripting layer.

Using managed memory in Unity is the easiest way to manage the memory in your application, but it has some disadvantages. The garbage collector is convenient to use, but it’s also unpredictable in how it releases and allocates memory, which might lead to performance issues such as stuttering, which happens when the garbage collector has to stop to release and allocate memory. To work around this unpredictability, you can use the C# unmanaged memory layer.

For more information on how managed memory works, refer to the section on Managed memory.

```ad-note

Mono和IL2CPP都有脚本虚拟机，
包含 堆内存、调用栈、nativeVM内存

```

Mono 和 IL2CPP 的脚本虚拟机 (VM) 实现了托管内存系统，有时也称为脚本内存系统。 VM 提供受控内存环境，分为以下不同类型：

* **the managed heap**：VM 使用垃圾收集器自动控制的内存部分。
* **the scripting stack**: 当您的应用程序进入和退出任何代码范围时，脚本堆栈就会建立和展开。
* **native VM memory**: 包含与Unity脚本层相关的内存。

在 Unity 中使用托管内存是管理应用程序内存的最简单方法，但它有一些缺点。垃圾收集器使用起来很方便，但它如何释放和分配内存也是不可预测的，这可能会导致性能问题，例如当垃圾收集器必须停止释放和分配内存时就会出现卡顿现象。**要解决这种不可预测性，您可以使用 C# 非托管内存层**。

有关托管内存如何工作的更多信息，请参阅托管内存部分。


---

## C# unmanaged memory

The **C# unmanaged memory layer** allows you to access the native memory layer to fine-tune memory allocations, with the convenience of writing C# code.

You can use the Unity.Collections namespace (including NativeArray) in the Unity core API, the data structures in the Unity Collections package, and UnsafeUtility.Malloc and UnsafeUtility.Free to access C# unmanaged memory.

If you use Unity’s job system, or Burst, you must use C# unmanaged memory. Additionally, to be able to use Burst you can only use the containers in the Unity.Collections namespace or types that fulfill the C# unmanaged constraint.

Unity’s native code, and sometimes its native allocators, allocate the memory that’s accessible to C# via these APIs. Because of this, tools like the Memory Profiler group C/# unmanaged memory in with the native memory.

```ad-note

C#非托管内存，可以微调内存分配。

```

C# 非托管内存层允许您访问本机内存层以微调内存分配，并方便编写 C# 代码。

您可以使用Unity核心API中的Unity.Collections命名空间（包括NativeArray）、Unity Collections包中的数据结构以及UnsafeUtility.Malloc和UnsafeUtility.Free来访问C#非托管内存。

如果您使用 Unity 的作业系统或 Burst，则必须使用 C# 非托管内存。此外，为了能够使用 Burst，您只能使用 Unity.Collections 命名空间中的容器或满足 C# 非托管约束的类型。

Unity 的本机代码（有时是其本机分配器）通过这些 API 分配 C# 可以访问的内存。因此，像 Memory Profiler 这样的工具将 C/# 非托管内存归入本机内存。


---

## Native memory

The Unity engine’s internal C/C++ core has its own memory management system, called native memory. In most situations, you can’t directly access or modify this memory type.

Unity stores the scenes in your project, assets, graphics APIs, graphics drivers, subsystems, plug-in buffers, and allocations inside native memory. This means that you can indirectly access the native memory via Unity’s C# API and manipulate your application’s data in a safe way, and get the benefits of the native and efficient code that’s in the internal C/C++ core.

Most of the time, you won’t need to interact with Unity’s native memory, but you can check how it affects the performance of your application whenever you use the Profiler
, through Profiler markers. You can also adjust the performance of your application by changing the settings on your assets and via configurable settings for Unity’s native memory allocators. For more information, refer to Native memory allocators.

```ad-note

C++内存，不方便直接访问，可以配置。

```

Unity引擎的内部C/C++核心有自己的内存管理系统，称为本机内存。在大多数情况下，您无法直接访问或修改此内存类型。

Unity 将场景、资源、图形 API、图形驱动程序、子系统、插件缓冲区以及本机内存中的分配存储在您的项目中。这意味着您可以通过 Unity 的 C# API 间接访问本机内存，并以安全的方式操作应用程序的数据，并获得内部 C/C++ 核心中的本机高效代码的好处。

大多数时候，您不需要与 Unity 的本机内存交互，但您可以在使用 Profiler 时通过 Profiler 标记检查它如何影响应用程序的性能。您还可以通过更改资源的设置以及 Unity 本机内存分配器的可配置设置来调整应用程序的性能。有关更多信息，请参阅本机内存分配器。