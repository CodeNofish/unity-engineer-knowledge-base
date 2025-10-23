---
tags:
  - netframework
  - 项目迁移
---


#### DLL缺失：引用的unity相关dll导致的问题

修改相关csproject，将dll改为你的本地路径

```xml
<ItemGroup>  
  <Reference Include="System" />  
  <Reference Include="UnityEngine">  
    <HintPath>C:\Program Files\Tuanjie\Hub\Editor\2022.3.61t8\Editor\Data\Managed\UnityEngine.dll</HintPath>  
    <!--False表示不复制到输出目录-->
    <Private>False</Private>  
  </Reference>
</ItemGroup>
```

#### DLL冲突：framework3.5导致的问题

使用匹配 UnityEngine.dll 版本的的 framework。

使用现代C# sdk，在project标签上，加上 `Sdk="Microsoft.NET.Sdk"`，将多余的Compile指令注释掉。
```xml
Project Sdk="Microsoft.NET.Sdk"
```

修改相关csproject，使用是适配版本的framework

```xml
<!-- 避免自动生成程序集信息 -->
<GenerateAssemblyInfo>false</GenerateAssemblyInfo>
<LangVersion>9.0</LangVersion>
<Nullable>enable</Nullable>
<TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion>
<TargetFramework>netstandard2.1</TargetFramework>
```

Build成功即可。

#### 参考链接

**处理MSB3268**：
问题描述：UnityEngine.dll无法加载，因为它引用的各种基础程序集找不到。
解决方案：用匹配UnityEngine.dll的Framework版本。
https://blog.51cto.com/u_15127652/4036291
https://blog.csdn.net/phmatthaus/article/details/49923743