<h1 align="center">
<img src="res/logo.png" width="256" alt="Mufiy" />
</h1>

<div align="center">

![Version](https://img.shields.io/badge/version-0.0.4-orange)
![License](https://img.shields.io/badge/license-LGPL--2.1-blue)
![Platforms](https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20macOS%20%7C%20iOS%20%7C%20Android%20%7C%20WASM-lightgrey)
[![NuGet](https://img.shields.io/badge/NuGet-0.0.4-blueviolet)](https://www.nuget.org/packages/Mufiy)

</div>

([中文版](README_ZH.md))

> **Open-source plan**: Mufiy is currently in a pre-release stage before 1.0. The source code is not yet publicly available; the project will be open-sourced when the version reaches 1.0.

> **Compatibility note**: Mufiy is not a 1:1 reimplementation of WinForms; control property and method names differ. A compatibility package will be provided later to achieve a 1:1 WinForms reimplementation.

Mufiy is a cross-platform UI framework written in C#. It keeps the WinForms designer workflow: drag controls onto a form and set properties in the Visual Studio designer, then build applications for Windows, Linux, macOS, iOS, Android, and WebAssembly from the same C# codebase. Rendering and platform capabilities are provided by per-platform providers.

---

## Status

- Current version: **0.0.4**
- Development status: actively developed; APIs may change before 1.0
- Designer support primarily targets Visual Studio 2026 and later
- Visual Studio 2022 only provides WinForms designer support (no MXAML preview) and only supports desktop projects; for full functionality, use Visual Studio 2026 or later, VS Code, or Rider
- VS Code and Rider will provide MXAML preview and a WinForms-like designer after 1.0

## Platforms and Target Frameworks

Mufiy primarily targets modern .NET and provides stable support for .NET 10.0 and later. .NET 6.0 and .NET 8.0 are retained only for special runtime environments and are limited to desktop platforms.

| Platform | Target Framework (TFM) | Notes |
|---|---|---|
| Windows / Linux / macOS | `net6.0` / `net8.0` / `net10.0` | Desktop runtime |
| Android | `net10.0-android` | Requires the .NET Android workload |
| iOS | Not yet released | The platform adapter is still in development |
| WebAssembly | `net10.0` | Runs in the browser |
| Visual Studio designer | `net6.0-windows` / `net8.0-windows` / `net10.0-windows` | Designer-only; not a runtime target |

`net10.0` is the default runtime target. The `*-windows` variants exist solely to support the Visual Studio designer.

## Packages

| Package | Purpose |
|---|---|
| `Mufiy` | Core framework: controls, layout, theming, rendering, platform abstractions |
| `Mufiy.Platform.Desktop.Provider` | Desktop platform adapter (Windows / Linux / macOS) |
| `Mufiy.Platform.Android.Provider` | Android platform adapter |
| `Mufiy.Platform.iOS.Provider` | iOS platform adapter (not yet released) |
| `Mufiy.Platform.WASM.Provider` | WebAssembly platform adapter |
| `Mufiy.Xaml` | XAML runtime and toolchain (language service, live preview, etc.) |
| `Mufiy.Desktop.CEFWebView` | CEF-based WebView with NativeAOT subprocesses for four RIDs |
| `Mufiy.Desktop.NativeWebView` | WebView backed by the platform's native control |
| `Mufiy.Fonts` | Default font assets (Arimo, etc.), referenced automatically by templates |
| `Mufiy.SourceGenerator` | Source generator |
| `Mufiy.ResxAotSupport` | Resource and NativeAOT support (replaces the deprecated ResxMapGenerator packages) |
| `Mufiy.Templates` | `dotnet new` project and item templates (for non-Visual Studio platforms only; in Visual Studio, always use the templates bundled with the extension) |

> `Mufiy.Desktop.CEFWebView` follows the CEF major version (for example `0.156.x`); all other packages follow the framework version.

## Getting Started

### 1. Install the NuGet packages

Starting with 0.0.4, Mufiy is published to the official NuGet feed. A desktop project created from the template references the following packages (versions follow the template as a floating stable version; pinned to 0.0.4 here for illustration):

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

Android projects reference `Mufiy.Platform.Android.Provider` and additionally `SkiaSharp.Views`.

### 2. Create a project

Where you get templates depends on your environment:

| Environment | Template source |
|---|---|
| Visual Studio 2026 | Mufiy Designer extension (`MufiyDesign_*.vsix`) |
| Visual Studio 2022 | Mufiy template extension (`MufiyDesign_Vs2022_*.vsix`) |
| VS Code / Rider / CLI | NuGet package `Mufiy.Templates` |

Always use the templates bundled with the extension inside Visual Studio; `Mufiy.Templates` targets VS Code, Rider, and command-line environments:

```bash
dotnet new install Mufiy.Templates@0.0.4

# Desktop single-project app
dotnet new mufiy-app -n MyApp

# Multi-project app (desktop + Android)
dotnet new mufiy-app-multiplatform -n MyApp
```

The template package contains five templates: `mufiy-app`, `mufiy-app-multiplatform`, `mufiy-form`, `mufiy-xamlform`, and `mufiy-control`. `mufiy-app-multiplatform` generates Desktop and Android projects only; the template package does not yet include a WebAssembly project. For WebAssembly, use the Mufiy App (Multi-Platform) template from the Visual Studio 2026 extension.

### 3. Use the Visual Studio extensions

- **Visual Studio 2026** (`MufiyDesign_*.vsix`):
  - Project templates: Mufiy App, Mufiy App (Multi-Platform)
  - Item templates: Form (Mufiy WinForm Designer), Form (Mufiy XAML Designer), User Control (Mufiy WinForm Designer)
  - Provides syntax diagnostics, IntelliSense, Go to Definition, and in-editor live preview for `.mxaml` documents (live preview requires the .NET 10.0 runtime)
- **Visual Studio 2022** (`MufiyDesign_Vs2022_*.vsix`):
  - Project template: Mufiy App
  - Item templates: Form (Mufiy WinForm Designer), User Control (Mufiy WinForm Designer)

After installation, search for "Mufiy" in the Create a new project dialog.

## Building from Source

### Prerequisites

- [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
- Visual Studio 2026 (designer and VSIX development)
  - Workloads: `.NET desktop development` and `Visual Studio extension development`
  - For Android: `.NET Multi-platform App UI development` plus Android SDK / NDK
- For WebAssembly: the `wasm-tools` workload

### Build

```bash
git clone https://github.com/MufiyUI/Mufiy.git
cd Mufiy
dotnet build src/Mufiy.slnx -c Release
```

Alternatively, open `src/Mufiy.slnx` in Visual Studio 2026, set `Demo` as the startup project, and run it.

### Packaging scripts

| Script | Purpose |
|---|---|
| `build-nugetpackage.ps1` | Packs all NuGet packages into `artifacts/` |
| `build-templates.ps1` | Packs the `dotnet new` template package |
| `build-vsix.ps1` | Rebuilds the preview service and packs the VS2026 / VS2022 extensions |

```powershell
powershell -File build-nugetpackage.ps1 -Configuration Release
powershell -File build-templates.ps1
powershell -File build-vsix.ps1
```

## Fonts and Third-Party Licenses

The default font bundled in `Mufiy.Fonts` is **Arimo**, licensed under the SIL Open Font License 1.1; the license text is included in the package. The framework falls back to bundled fonts when system fonts are unavailable. You can disable copying of bundled fonts and use your own fonts instead.

## License

Mufiy is licensed under the [GNU Lesser General Public License 2.1](LICENSE) (LGPL-2.1). Files copied from other projects remain under their original licenses, as indicated in their file headers.

## Links

- Repository: <https://github.com/MufiyUI/Mufiy>
- Website: <https://www.mufiy.com>
- Documentation: <https://doc.mufiy.com>
- NuGet: <https://www.nuget.org/packages/Mufiy>
