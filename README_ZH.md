<h1 align="center">
<img src="res/logo.png" width="256" alt="Mufiy" />
</h1>

<div align="center">

![Version](https://img.shields.io/badge/version-0.0.4-orange)
![License](https://img.shields.io/badge/license-LGPL--2.1-blue)
![Platforms](https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20macOS%20%7C%20iOS%20%7C%20Android%20%7C%20WASM-lightgrey)
[![NuGet](https://img.shields.io/badge/NuGet-0.0.4-blueviolet)](https://www.nuget.org/packages/Mufiy)

</div>

([English](README.md))

> **开源计划**：Mufiy 目前处于 1.0 之前的预发布阶段，源码暂未对外开放；待版本号达到 1.0 时，本项目将正式开源。

> **兼容性说明**：Mufiy 并不是 WinForms 的 1:1 复刻，控件的属性名与方法名都存在差异。Mufiy 后续将提供兼容包，用于实现对 WinForms 的 1:1 复刻。

Mufiy 是一个使用 C# 编写的跨平台 UI 框架。它沿用 WinForms 设计器的开发体验：在 Visual Studio 设计器中拖放控件、设置属性，即可构建面向 Windows、Linux、macOS、iOS、Android 与 WebAssembly 的应用程序。同一套 C# 代码可以编译到多个平台，界面渲染与平台能力由各平台 Provider 提供。

---

## 当前状态

- 当前版本：**0.0.4**
- 开发状态：积极开发中，API 在 1.0 之前可能发生调整
- 设计器支持主要面向 Visual Studio 2026 及以上版本
- Visual Studio 2022 仅提供 WinForms 设计器支持，不提供 MXAML 预览器支持，并且只支持桌面平台项目；需要完整功能支持请使用 Visual Studio 2026 及以上版本、VS Code 或 Rider
- VS Code 与 Rider 将在 1.0 之后提供 MXAML 预览器和类似 WinForms 的设计器

## 平台与目标框架

Mufiy 主要面向现代 .NET，对 .NET 10.0 及以上版本提供稳定支持；.NET 6.0 / .NET 8.0 是为特殊运行环境保留的兼容目标，仅支持桌面平台。

| 平台 | 目标框架（TFM） | 说明 |
|---|---|---|
| Windows / Linux / macOS | `net6.0` / `net8.0` / `net10.0` | 桌面运行时 |
| Android | `net10.0-android` | 需要 .NET Android 工作负载 |
| iOS | 暂未发布 | 平台适配仍在开发中 |
| WebAssembly | `net10.0` | 运行于浏览器 |
| Visual Studio 设计器 | `net6.0-windows` / `net8.0-windows` / `net10.0-windows` | 仅用于设计器加载，不作为运行目标 |

框架默认以 `net10.0` 作为运行目标；`*-windows` 变体只为 Visual Studio 设计器提供支持。

## 包一览

| 包 | 用途 |
|---|---|
| `Mufiy` | 核心框架：控件、布局、主题、绘制与平台抽象 |
| `Mufiy.Platform.Desktop.Provider` | 桌面平台（Windows / Linux / macOS）适配 |
| `Mufiy.Platform.Android.Provider` | Android 平台适配 |
| `Mufiy.Platform.iOS.Provider` | iOS 平台适配（暂未发布） |
| `Mufiy.Platform.WASM.Provider` | WebAssembly 平台适配 |
| `Mufiy.Xaml` | XAML 运行时与工具链（语言服务、实时预览等） |
| `Mufiy.Desktop.CEFWebView` | 基于 CEF 的 WebView，含四个 RID 的 NativeAOT 子进程 |
| `Mufiy.Desktop.NativeWebView` | 基于系统原生控件的 WebView |
| `Mufiy.Fonts` | 默认字体资源（Arimo 等），由模板自动引用 |
| `Mufiy.SourceGenerator` | 源生成器 |
| `Mufiy.ResxAotSupport` | 资源与 NativeAOT 支持（替代已弃用的 ResxMapGenerator 包） |
| `Mufiy.Templates` | `dotnet new` 项目模板与项模板（仅用于非 Visual Studio 平台；Visual Studio 中始终使用扩展包附带的模板） |

> `Mufiy.Desktop.CEFWebView` 的包版本与 CEF 大版本对齐（例如 `0.156.x`），其余包与框架版本保持一致。

## 快速开始

### 1. 安装 NuGet 包

从 0.0.4 起，Mufiy 已发布到 NuGet 官方源。使用模板创建的桌面项目会自动引用以下包（模板中版本号为浮动的正式版，这里固定为 0.0.4 作示例）：

```xml
<ItemGroup>
  <PackageReference Include="Mufiy" Version="0.0.4" />
  <PackageReference Include="Mufiy.Fonts" Version="0.0.4" />
  <PackageReference Include="Mufiy.Platform.Desktop.Provider" Version="0.0.4" />
  <PackageReference Include="Mufiy.SourceGenerator" Version="0.0.4" />
  <PackageReference Include="Mufiy.ResxAotSupport" Version="0.0.4" />
  <PackageReference Include="System.Resources.Extensions" Version="10.0.8" />
</ItemGroup>
```

Android 项目引用的平台包为 `Mufiy.Platform.Android.Provider`，并额外引用 `SkiaSharp.Views`。

### 2. 创建项目

模板来源取决于开发环境：

| 开发环境 | 模板来源 |
|---|---|
| Visual Studio 2026 | Mufiy Designer 扩展（`MufiyDesign_*.vsix`） |
| Visual Studio 2022 | Mufiy 模板扩展（`MufiyDesign_Vs2022_*.vsix`） |
| VS Code / Rider / 命令行 | NuGet 包 `Mufiy.Templates` |

Visual Studio 中请始终使用扩展自带的模板；`Mufiy.Templates` 面向 VS Code、Rider 与命令行等非 Visual Studio 环境：

```bash
dotnet new install Mufiy.Templates@0.0.4

# 桌面单项目应用
dotnet new mufiy-app -n MyApp

# 多项目应用（桌面 + Android）
dotnet new mufiy-app-multiplatform -n MyApp
```

模板包包含 5 个模板：`mufiy-app`、`mufiy-app-multiplatform`、`mufiy-form`、`mufiy-xamlform`、`mufiy-control`。其中 `mufiy-app-multiplatform` 生成 Desktop 与 Android 两个项目，模板包内暂时不含 WebAssembly 项目；需要 WebAssembly 时请使用 Visual Studio 2026 扩展中的 Mufiy App (Multi-Platform) 模板。

### 3. 使用 Visual Studio 扩展

- **Visual Studio 2026**（`MufiyDesign_*.vsix`）：
  - 项目模板：Mufiy App、Mufiy App (Multi-Platform)
  - 项模板：窗体 (Mufiy WinForm Designer)、窗体 (Mufiy XAML Designer)、用户控件 (Mufiy WinForm Designer)
  - 为 `.mxaml` 文档提供语法诊断、智能提示、转到定义与编辑器内实时预览（实时预览需要安装 .NET 10.0 运行时）
- **Visual Studio 2022**（`MufiyDesign_Vs2022_*.vsix`）：
  - 项目模板：Mufiy App
  - 项模板：窗体 (Mufiy WinForm Designer)、用户控件 (Mufiy WinForm Designer)

安装完成后，在“创建新项目”中搜索 Mufiy 即可。

## 从源码构建

### 先决条件

- [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
- Visual Studio 2026（设计器与 VSIX 开发）
  - 工作负载：`.NET 桌面开发`、`Visual Studio 扩展开发`
  - 目标 Android 时另需 `.NET Multi-platform App UI 开发` 与 Android SDK / NDK
- 目标 WebAssembly 时需安装 `wasm-tools` 工作负载

### 编译

```bash
git clone https://github.com/MufiyUI/Mufiy.git
cd Mufiy
dotnet build src/Mufiy.slnx -c Release
```

或使用 Visual Studio 2026 打开 `src/Mufiy.slnx`，将 `Demo` 设为启动项目后运行。

### 打包脚本

| 脚本 | 作用 |
|---|---|
| `build-nugetpackage.ps1` | 打包全部 NuGet 包并归档到 `artifacts/` |
| `build-templates.ps1` | 打包 `dotnet new` 模板包 |
| `build-vsix.ps1` | 重建预览服务并打包 VS2026 / VS2022 扩展 |

```powershell
powershell -File build-nugetpackage.ps1 -Configuration Release
powershell -File build-templates.ps1
powershell -File build-vsix.ps1
```

## 字体与第三方许可

`Mufiy.Fonts` 包内置的默认字体为 **Arimo**，遵循 SIL Open Font License 1.1，许可文本随包提供。框架在缺少系统字体时会回退到包内字体；你也可以通过项目属性关闭内置字体的复制，改用自有字体。

## 许可证

本项目采用 [GNU Lesser General Public License 2.1](LICENSE)（LGPL-2.1）。部分来自其它项目的文件遵循其原始许可，具体以文件头说明为准。

## 链接

- 仓库：<https://github.com/MufiyUI/Mufiy>
- 官网：<https://www.mufiy.com>
- 文档：<https://doc.mufiy.com>
- NuGet：<https://www.nuget.org/packages/Mufiy>
