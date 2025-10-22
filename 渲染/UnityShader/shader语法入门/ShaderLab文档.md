---
tags:
  - Shader
---
# 参考链接
https://docs.unity3d.com/cn/2023.2/Manual/SL-Reference.html

---
# ShaderLab

ShaderLab是一种 在Unity着色器源文件中 声明性语言，用来描述Shader对象，使用大括号语法。

可以定义如下内容：
* 定义Shader对象的整体结构
* 使用代码块添加HLSL编写的着色器程序
* 使用命令设置GPU的渲染状态
* 从着色器公开属性
* 设置 SubShader 和 Pass 的包要求
* 定义回退行为

---
# ShaderLab：定义Shader对象

## 定义Shader对象



Shader代码块中，
Properties 定义 材质属性，
SubShader 定义 子着色器程序，
CustomEditor 指定 Inspector编辑器，
Fallback 指定 回退Shader对象。


```ShaderLab
Shader "<name>"
{
    <optional: Material properties>
    <One or more SubShader definitions>
    <optional: custom editor>
    <optional: fallback>
}
```

```ShaderLab
Shader "Examples/ShaderSyntax"
{
    CustomEditor = "ExampleCustomEditor"
    Properties
    {
        // 此处是材质属性声明
    }
    SubShader
    {
        // 此处是定义子着色器的其余代码
        Pass
        {
           // 此处是定义通道的代码
        }
    }
    Fallback "ExampleFallbackShader"
}
```

注意，在Unity 5.0之前，着色器的一些功能是由它的路径和名称决定的。这仍然是Unity的遗留着色器的工作方式。更改这些着色器的名称会影响它们的功能。


## 定义材质属性

#### 概况

材质属性，unity可以作为材质资产的数据存储，方便美术区创建、编辑、配置材质。

使用材质属性时：
* 调用类似Material.SetFloat之类的方法，可以设置获取材质的参数。
* 可以使用Inspector随时修改材质属性。
* 作为材质资产的数据存储在`.mat`文件中。

不使用材质属性时：
* 仍然可以使用材质API修改Shader对象属性
* 没有Inspector可视化编辑
* 无法存储在`.mat`文件中
#### 管线兼容性

在URP/HDRP/SRP中，在HLSL代码中，必须使用将材质属性放在同一个CBUFFER，方便SRP批处理。
#### 语法

```Shader
Properties
{
    <Material property declaration>
    <Material property declaration>
}
```

```
[optional: attribute] name("display text in Inspector", type name) = default value
```

#### 材质属性类型定义

#### 保留的材质属性名

#### 材质属性的Attribute

#### 使用脚本操控材质属性

#### 使用材质属性 设置ShaderLab的变量

#### 使用材质属性 设置HLSL的变量