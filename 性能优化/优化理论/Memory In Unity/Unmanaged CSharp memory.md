The C# unmanaged memory layer allows you to access the native memory layer to fine-tune memory allocations, with the convenience of writing C# code.

You can use the Unity.Collections namespace (including NativeArray) in the Unity core API, and the data structures in the Unity Collections package to access C# unmanaged memory. If you use Unity’s job system, or Burst, you must use C# unmanaged memory.

C# 非托管内存层允许您访问本机内存层以微调内存分配，并方便编写 C# 代码。

您可以使用 Unity 核心 API 中的 Unity.Collections 命名空间（包括 NativeArray）以及 Unity Collections 包中的数据结构来访问 C# 非托管内存。如果您使用 Unity 的作业系统或 Burst，则必须使用 C# 非托管内存。

---

## Native container memory allocators overview

Unity’s native code libraries track native memory usage through a system of memory labels, areas, and roots that are managed by Unity’s native memory manager. The Profiler
 uses the native memory manager to monitor native memory usage. The asset garbage collector also tracks types that inherit from UnityEngine.Object so that on calls to Resources.UnloadUnusedAssets it can clean up memory assigned to assets that are no longer referenced.

Unity’s native memory manager uses memory labels to categorize memory usage and to choose which Allocator allocates the memory. Which allocator the memory manager uses for each memory label varies between platforms.

The memory manager uses areas to categorize memory usage for profiling purposes. It uses roots to track related memory allocations under one root. For example, the native memory for each type that inherits from UnityEngine.Object has a root in the area called Object. When the object is deleted, for example via Resources.UnloadUnusedAssets, all allocations associated with that object’s root are also freed.

You can use the Memory Profiler package to see memory usage ordered by memory labels, areas, and roots. You can also see which underlying allocator allocated the memory through region names.

```ad-note



```

Unity 的本机代码库通过由 Unity 的本机内存管理器管理的 内存标签、区域和根系统 来跟踪本机内存使用情况。 Profiler 使用本机内存管理器来监视本机内存使用情况。资产垃圾收集器还跟踪从 UnityEngine.Object 继承的类型，以便在调用 Resources.UnloadUnusedAssets 时，它可以清理分配给不再引用的资产的内存。

Unity 的本机内存管理器使用内存标签对内存使用情况进行分类并选择哪个分配器分配内存。内存管理器为每个内存标签使用哪个分配器因平台而异。

内存管理器使用区域对内存使用情况进行分类以进行分析。它使用根来跟踪一个根下的相关内存分配。例如，从 UnityEngine.Object 继承的每种类型的本机内存在称为 Object 的区域中都有一个根。当对象被删除时，例如通过 Resources.UnloadUnusedAssets，与该对象的根关联的所有分配也将被释放。

您可以使用内存分析器包来查看按内存标签、区域和根排序的内存使用情况。您还可以通过区域名称查看哪个底层分配器分配了内存。


---

## Allocator types

You can use certain native [Allocator types](https://docs.unity3d.com/6000.2/Documentation/ScriptReference/Unity.Collections.Allocator.html) with native container types in the `Unity.Collections` namespace. The allocator types available in this way are:

* Temp: A temporary allocator type for short-lived allocations.
* TempJob: A temporary allocator type for longer-lived allocations passed from job to job.
* Persistent: An allocator without any temporal restrictions.
* AudioKernel: An allocator for DSP audio kernel allocations.
* Domain: An allocator that lasts the lifetime of the domain.

您可以在 Unity.Collections 命名空间中将某些本机分配器类型与本机容器类型一起使用。这种方式可用的分配器类型有：
* Temp：用于短期分配的临时分配器类型。
* TempJob：一种临时分配器类型，用于在作业之间传递长期生存的分配。
* Persistent：没有任何时间限制的分配器。
* AudioKernel：DSP 音频内核分配的分配器。
* Domain：持续域生命周期的分配器。

### Temp

Temp is Unity’s temporary allocator type. It’s designed for small short lived allocations that can’t cross a frame boundary, such as temporary allocations made within a single job. The Temp type is a stack type allocator for small amounts of memory which Unity allocates from the main memory pool.

There are multiple Temp allocators, and each thread that needs an allocator has one. Unity recycles Temp allocators each frame ensuring a clean memory usage pattern. However, if memory spills over into the fallback allocators, their memory might get fragmented.

You can use Profiler.GetTempAllocatorSize and Profiler.SetTempAllocatorRequestedSize to get and set the size for Temp allocators.

- Editor: 16 MB main thread, 256 KB worker threads.
- Players: 4 MB main thread, 256 KB worker threads.

If these sizes are exceeded, allocations on the main thread fall back to the `TempJob` allocator, and on other threads they fall back to the Temp Overflow allocator. For more information on temporary native allocations, refer to the [Thread storage local (TLS) allocator](https://docs.unity3d.com/6000.2/Documentation/Manual/performance-native-allocators.html#tls) documentation.

```ad-note



```

Temp 是 Unity 的临时分配器类型。它专为无法跨越框架边界的小型短期分配而设计，例如在单个作业中进行的临时分配。 Temp 类型是一个堆栈类型分配器，用于 Unity 从主内存池分配的少量内存。

有多个 Temp 分配器，每个需要分配器的线程都有一个。 Unity 每帧回收临时分配器，确保干净的内存使用模式。但是，如果内存溢出到后备分配器中，它们的内存可能会出现碎片。

您可以使用 Profiler.GetTempAllocatorSize 和 Profiler.SetTempAllocatorRequestedSize 来获取和设置临时分配器的大小。

临时分配器的大小默认为：
* Editor: 16 MB main thread, 256 KB worker threads.
* Players: 4 MB main thread, 256 KB worker threads.

如果超出这些大小，主线程上的分配将回退到 TempJob 分配器，而在其他线程上，它们会回退到 Temp Overflow 分配器。有关临时本机分配的更多信息，请参阅线程本地存储 (TLS) 分配器文档。

---

### TempJob

This type uses a linear style allocator which Unity stores in a TempJob area that it allocates from main memory. It’s designed for larger temporary allocations that are passed from job to job, and those that might carry data between frames. If the allocations aren’t disposed of within 4 frames of their creation, a warning is displayed in the Unity Editor.

Note: If the memory available is exhausted, Unity makes the overspill allocations in main memory (malloc). Most platforms can have up to 64 MB of TempJob memory, but on smaller memory systems this limit can be as low as 16 MB.

If this allocator is full, allocations fall back to the Temp Job Overflow Allocator. For more information on temporary job allocations, refer to the [Thread safe linear allocator](https://docs.unity3d.com/6000.2/Documentation/Manual/performance-native-allocators.html#thread-safe-linear) documentation.

```ad-note



```

此类型使用线性样式分配器，Unity 将其存储在从主内存分配的 TempJob 区域中。它专为在作业之间传递的较大临时分配以及可能在帧之间传输数据的分配而设计。如果分配在创建后 4 帧内未处理，Unity 编辑器中会显示一条警告。

注意：如果可用内存耗尽，Unity 会在主内存 (malloc) 中进行溢出分配。大多数平台最多可拥有 64 MB 的 TempJob 内存，但在较小的内存系统上，此限制可能低至 16 MB。

如果此分配器已满，则分配将回退到临时作业溢出分配器。有关临时作业分配的更多信息，请参阅线程安全线性分配器文档。


---

### Persistent

This allocator allocates memory with no time restrictions, is only limited by the amount of free memory on a device, and therefore has no fallback. It can exist for as long as you need it, but this also means that only the DisposeSentinel class warns you if you don’t free the allocated memory again, and then only if your handle to the memory was garbage collected. Allocating persistent memory is slower than allocating temporary memory.

此分配器分配内存没有时间限制，仅受设备上的可用内存量限制，因此没有回退。只要您需要它，它就可以存在，但这也意味着只有 DisposeSentinel 类在您不再次释放分配的内存时才会警告您，并且只有当您的内存句柄被垃圾收集时才会发出警告。分配持久内存比分配临时内存慢。