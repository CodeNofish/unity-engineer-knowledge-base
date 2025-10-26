# 着色器资产

着色器资源是 Unity 项目中定义 Shader 对象的资源。它是一个扩展名为 .shader 的文本文件。它包含着色器代码。



#### 创建新的着色器资源

To create a new shader asset, use **Assets**  \> **Create** > **Shader**  from the main menu or the **Project View** context menu.



#### Inspector reference

当您在 Unity 项目中选择着色器资源时，检查器会显示有关其定义的着色器对象的基本信息。它还提供用于编译和检查编译后的代码的控件。



#### 导入设置

检查器的导入设置部分允许您设置着色器资源的默认纹理。

每当使用此着色器创建新材质时，都会自动分配这些纹理。



#### 导入对象

检查器的导入对象部分允许您查看和编辑与着色器对象本身相关的设置，以及着色器编译器如何处理它。

![Shader compilation popup menu.](Inspector-ShaderCompilePopup.png)

```ad-note
需要对编译出来的平台Shader代码，进行优化；
可以放到GPU性能分析工具上，进行测试，如 AMD GPU ShaderAnalyzer 或 PVRShaderEditor。
```

对于表面着色器，显示生成的代码按钮显示 Unity 从简化的源文件生成的代码。如果您想自定义生成的代码，只需将其复制并粘贴到着色器源文件中即可。

您可以使用“编译并显示代码”弹出菜单来：

* Manually compile all shader variants for a selected graphics API. In the Unity Editor, the shader does not compile all shader variants for all graphics APIs until build time. You can manually force this to check for errors. For more information on what the shader compiler does and when, see Shader compilation

  手动编译选定图形 API 的所有着色器变体。在 Unity 编辑器中，着色器在构建时才会编译所有图形 API 的所有着色器变体。您可以手动强制执行此操作以检查错误。有关着色器编译器的作用和时间的更多信息，请参阅着色器编译。

* Inspect the compiled shader code for selected platforms. This is mostly useful while optimizing shaders for performance; often you do want to know how many low-level instructions are generated. You can paste the generated code into GPU shader performance analysis tools (like AMD GPU ShaderAnalyzer or PVRShaderEditor.

  检查所选平台的已编译着色器代码。这在优化着色器性能时非常有用；通常您确实想知道生成了多少低级指令。您可以将生成的代码粘贴到 GPU 着色器性能分析工具（例如 AMD GPU ShaderAnalyzer 或 PVRShaderEditor）中。