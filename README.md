# CUI

面向 **桌面、移动与跨平台客户端应用** 的 .NET UI 框架规划（**非游戏引擎**）。设计目标：在 **.NET 10+** 上实现 **完整 Native AOT**、**Full Trim** 与可控的应用体积；**设计阶段**用 XAML 编辑与预览，**编译发布**时转为链式 C#，运行时无 XAML。

## 文档

| 文档 | 说明 |
|------|------|
| [文档索引](docs/README.md) | 阅读顺序与术语 |
| [愿景与目标](docs/01-vision-and-goals.md) | 定位、非目标、成功标准 |
| [架构总览](docs/02-architecture.md) | 分层、数据流、渲染与宿主 |
| [平台矩阵](docs/03-platform-matrix.md) | 桌面/移动/跨平台支持与 AOT 状态 |
| [Native AOT 与体积](docs/04-native-aot-and-size.md) | 裁剪、发布、体积预算 |
| [模块与仓库结构](docs/05-module-structure.md) | 程序集划分与依赖规则 |
| [API 设计原则](docs/06-api-design-principles.md) | 绑定、样式、控件扩展 |
| [路线图](docs/07-roadmap.md) | 阶段交付与里程碑 |
| [AOT 替代方案](docs/08-aot-friendly-alternative.md) | 样式/绑定/扩展相对 Avalonia 的换路方案 |
| [NanoVG 渲染 CuiVG](docs/09-rendering-nanovg.md) | 托管 NanoVG 移植与 GPU 后端 |
| [XAML→链式 C#](docs/10-xaml-to-csharp-markup.md) | XamlC 产物规范（参照 MewUI） |
| [控件动画](docs/11-animation.md) | 平移/缩放/旋转等（参照 WPF，AOT 友好） |
| [**完整推进计划（对标 Avalonia 12）**](docs/12-master-plan-avalonia12-parity.md) | 分阶段控件表、能力表、人力与 KPI |
| [**设计参照画廊（HTML）**](UISampleHTML/index.html) | 零构建控件样例，对齐 ThemePalette，对标主计划控件表 |

## 快速结论

- **跨平台**：统一 `Core` + 按平台拆分的 `Host.*` / `Rendering.*`，应用只引用目标平台包。
- **必须 AOT**：禁止运行时 XAML、反射绑定、反射 DI；见 [AOT 替代方案](docs/08-aot-friendly-alternative.md)（`StyleSheet`、委托绑定、编译期模块）。
- **渲染**：默认 **CuiVG**（NanoVG API 的 C# 移植），按 RID 选择 OpenGL / Metal。
- **标记**：**设计期**编辑 `.cui.xaml` 并预览；**编译期** XamlC 转为与 [MewUI](https://github.com/aprillz/MewUI) 同风格的链式 C#（详见 [XAML 三阶段模型](docs/10-xaml-to-csharp-markup.md#三阶段模型编辑--设计期预览--编译发布)）。
- **场景**：客户端 GUI 工具与 App；**不**面向游戏开发（无场景循环/精灵/物理等）。
- **动画**：控件级 **平移/缩放/旋转/透明度** 与 Storyboard（参照 WPF）；默认动画渲染属性，不触发布局（见 [控件动画](docs/11-animation.md)）。
- **体积小**：`TrimMode=full` + `CuiBackend` 后端裁剪 + 未使用控件消除。
- **.NET 10 起**：所有库 `IsTrimmable` + `IsAotCompatible`，CI 中 `PublishAot` 警告视为失败。

## 快速开始（P0 代码）

```bash
cd d:\开发\CUI
dotnet build CUI.sln -c Release
dotnet run --project samples/HelloDesktop/HelloDesktop.csproj -c Release
dotnet test tests/CUI.Core.Tests/CUI.Core.Tests.csproj -c Release
```

当前已实现：

- **P0**：Core 布局、Window/Border/TextBlock/Button、CuiVG 软件光栅、Win32 宿主、链式 Markup  
- **P1（进行中）**：`CheckBox`、`ScrollViewer`、`Image`、`ThemeManager`（Light/Dark）、滚轮滚动、主题切换示例  

```bash
dotnet run --project samples/HelloDesktop/HelloDesktop.csproj -c Release
```

## 许可

待定（实现阶段补充）。
