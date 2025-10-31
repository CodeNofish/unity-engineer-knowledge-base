
This page contains some tips to help you obtain the best performance in Unity, covering the animation system and run-time optimizations.

Note: For tips on modeling your character in a 3d application for best performance in Unity, see Modeling characters for optimal performance.

本页包含一些提示，可帮助您在 Unity 中获得最佳性能，涵盖动画系统和运行时优化。

注意：有关在 3d 应用程序中对角色进行建模以在 Unity 中获得最佳性能的提示，请参阅对角色进行建模以获得最佳性能。

```ad-note

在建模过程中的优化，是第一步，也是最重要的一部

```

---

## Animation system

### Controllers

The Animator doesn’t spend time processing when a Controller is not set to it.

当控制器未设置时，动画器不会花时间进行处理。

### Simple animation

Playing a single Animation Clip with no blending can make Unity slower than the legacy animation system. The old system is very direct, sampling the curve and directly writing into the transform. Unity’s current animation system has temporary buffers it uses for blending, and there is additional copying of the sampled curve and other data. The current system layout is optimized for animation blending and more complex setups.

在不进行混合的情况下播放单个动画剪辑可能会使 Unity 比旧版动画系统慢。旧系统非常直接，对曲线进行采样并直接写入变换。 Unity 当前的动画系统具有用于混合的临时缓冲区，并且还对采样曲线和其他数据进行额外的复制。当前的系统布局针对动画混合和更复杂的设置进行了优化。

```ad-note

简单动画用老的动画系统，可能效果更好。

```

### Scale curves

Animating scale curves is more expensive than animating translation and rotation curves. To improve performance, avoid scale animations.

Note: This does not apply to constant curves (curves that have the same value for the length of the animation clip). Constant curves are optimized, and are less expensive than normal curves. Constant curves that have the same values as the default scene
 values do not write to the scene every frame.

动画缩放曲线比动画平移和旋转曲线更昂贵。为了提高性能，请避免缩放动画。

注意：这不适用于恒定曲线（动画剪辑长度具有相同值的曲线）。恒定曲线经过优化，并且比普通曲线更便宜。与默认场景值具有相同值的恒定曲线不会每帧都写入场景。

```ad-note

缩放动画 比平移动画、旋转曲线更昂贵。

```

### Layers

Most of the time Unity is evaluating animations, and keeps the overhead for animation layers and Animation State Machines to the minimum. The cost of adding another layer to the animator, synchronized or not, depends on what animations and blend trees are played by the layer. When the weight of the layer is zero, Unity skips the layer update.

大多数时候，Unity 都会评估动画，并将动画层和动画状态机的开销保持在最低限度。向动画制作器添加另一个图层（同步与否）的成本取决于该图层播放的动画和混合树。当图层权重为零时，Unity 会跳过图层更新。

```ad-note

新加入的动画层会影响性能，把不使用的动画层 权重设为0。

```

---

### Humanoid vs. Generic animation types

These are tips to help you decide between these types:

- When importing Humanoid animation use an Avatar Mask (class-AvatarMask) to remove IK Goals or finger animation if you don’t need them.
- When you use Generic, using root motion is more expensive than not using it. If your animations don’t use root motion, make sure that you have not specified a root bone.

这些提示可帮助您在这些类型之间做出选择：
* 导入人形动画时，如果不需要，请使用 Avatar Mask（类 AvatarMask）来删除 IK 目标或手指动画。
* 当您使用 Generic 时，使用根运动比不使用它更昂贵。如果您的动画不使用根运动，请确保您没有指定根骨骼。

```ad-note

删除不使用的动画功能，如何 IK 手指动画 根运动。


```

---

### Scene-level optimization

There are many optimizations that can be made, some useful tips include:

- Use hashes instead of strings to query the Animator.
- Implement a small AI Layer to control the Animator. You can make it provide simple callbacks for OnStateChange, OnTransitionBegin, and other events.
- Use State Tags to easily match your AI state machine to the Unity state machine.
- Use additional curves to simulate events.
- Use additional curves to mark up your animations; for example, in conjunction with target matching.

可以进行许多优化，一些有用的提示包括：
* 使用散列而不是字符串来查询动画器。
* 实现一个小的 AI 层来控制动画器。您可以让它为 OnStateChange、OnTransitionBegin 和其他事件提供简单的回调。
* 使用状态标签轻松匹配你的AI状态机到Unity状态机。
* 使用额外的曲线来模拟事件。
* 使用额外的曲线来标记你的动画；例如，结合目标匹配。


```ad-note

场景级优化:
* 用hash查询 Animator

TODO

```


---

## Runtime Optimizations

### Visibility and updates

Always optimize animations by setting the animators’s Culling Mode to Based on Renderers, and disable the skinned mesh renderer’s Update When Offscreen property. This saves Unity from updating animations when the character is not visible.

始终通过将动画师的筛选模式设置为基于渲染器来优化动画，并禁用蒙皮网格渲染器的离线更新属性。这节省了Unity在角色不可见时更新动画的时间。

```ad-note

运行时优化：
基于可见性，减少动画更新，比如 离屏时 关闭 skinned mesh render，设置的动画的 Culling Mode

```