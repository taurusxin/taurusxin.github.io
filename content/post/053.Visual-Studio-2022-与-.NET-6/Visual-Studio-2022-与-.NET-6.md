---
title: "Visual Studio 2022 与 .NET 6"
categories: [ "前沿技术探索" ]
tags: [  ]
draft: false
slug: "vs2022-dotnet6"
date: "2021-11-10 06:13:00"
---

北京时间 2021 年 11 月 8 日晚，微软正式发布了 Visual Studio 2022 Current 和 .NET 6 框架，推出了 C# 10、F# 6 和 PowerShell 7.2

# .NET 6

 .NET 6 支持 Windows, Linux, macOS，原生支持苹果 M1 系列芯片。在各个平台上将获得 3 年的 LTS 长期支持。

![.NET 6 语言家族][2]

可以从下面的链接查看完整的更新日志

> [.NET 6 完整更新内容][1]

### .NET 6 亮点

 - 使用微软服务、其他公司运行的云应用程序和开源项目进行了生产压力测试。
   
 - 作为最新的长期支持 (LTS) 版本支持三年。
   
 - 跨浏览器、云、桌面、IoT 和移动应用程序的统一平台，所有应用程序都使用相同的 .NET 库和轻松共享代码的能力。
   
 - 性能全面提升，尤其是文件 I/O，减少了执行时间、延迟和内存使用。
   
 - C# 10 提供了语言改进，例如记录结构、隐式使用和新的 lambda 功能，同时编译器添加了增量源生成器。
   
 - F# 6 添加了新功能，包括基于任务的异步、管道调试和众多性能改进。
   
 - Visual Basic 在 Visual Studio 体验和 Windows 窗体项目打开体验方面进行了改进。
   
 - 热重载使用户可以跳过重新构建和重新启动应用程序以查看新更改 —— 在 Visual Studio 2022 和 .NET CLI 中支持，适用于 C# 和 Visual Basic。
   
 - 云诊断已通过 OpenTelemetry 和 dotnet 监视器得到改进，现在在生产中得到支持，并且可用于 Azure 应用服务。
   
 - JSON API 更强大，具有更高的性能，带有序列化程序的源生成器。
   
 - ASP.NET Core 中引入了最少的 API，以简化入门体验并提高 HTTP 服务的性能。
   
 - Blazor 组件现在可以从 JavaScript 呈现并与现有的基于 JavaScript 的应用程序集成。
   
 - 用于 Blazor WebAssembly (Wasm) 应用程序的 WebAssembly AOT 编译，以及对运行时重新链接和本机依赖项的支持。
   
 - 使用 ASP.NET Core 构建的单页应用程序现在使用更灵活的模式，可以与 Angular、React 和其他流行的前端 JavaScript 框架一起使用。
   
 - 添加了 HTTP/3，以便 ASP.NET Core、HttpClient 和 gRPC 都可以与 HTTP/3 客户端和服务器交互。
   
 - File IO 现在支持符号链接，并通过重新编写的 FileStream 大大提高了性能。
   
 - 通过支持 OpenSSL 3、ChaCha20Poly1305 加密方案和运行时深度防御缓解措施，特别是 W^X 和 CET，安全性得到了提高。
   
 - 可以为 Linux、macOS 和 Windows（以前仅适用于 Linux）发布单文件应用程序（免提取）。
   
 - IL 修整现在更加强大和有效，新的警告和分析器可确保正确的最终结果。
   
 - 添加了源代码生成器和分析器，可帮助用户生成更好、更安全和更高性能的代码。
   
 - 源代码构建使 Red Hat 等组织能够从源代码构建 .NET，并向其用户提供自己的构建版本。

该版本包括大约一万个 git 提交。但更新内容实在是太多，必须下载并试用 .NET 6 才能看到所有新内容。

# Visual Studio 2022

同时还发布了 Visual Studio 2022 正式版，终于将多年的 VS 主程序 从 32 位 改为 64 位，其内部版本号为 17.0.0

![Visual Studio 2022][3]

在 Visual Studio 2022 正式版中，微软专注于增强编辑和调试周期。

### IntelliCode

Visual Studio 2022 配备了 IntelliCode，它是一个 AI 辅助的代码伴侣，可让开发者输入更少的代码，提升效率。IntelliCode 可以完成整行代码，用户只需按两次 Tab 键即可编写可靠的代码。IntelliCode 还可以发现重复的编辑，并在整个代码库中存在类似模式的地方提出修复建议。

![IntelliCode][4]

### 热重载

当开发者对应用进行调试运行时，适用于 .NET 和 C++ 的热重载可以让开发者实时更新代码并立即查看更改，开发者无需重新部署和启动应用程序。

![Hot Reload][5]

### 性能提升

Visual Studio 2022 是第一个 64 位版本的 Visual Studio。它现在可以充分利用现代硬件，以便可靠地扩展到更大、更复杂的项目。此外，Visual Studio 2022 还专注于提高开发者每天使用的常见场景的性能。

![64位 VS 加载大型工程][6]

### 现代化图标

除此之外，Visual Studio 2022 还对一系列图标进行了更新，使其更加现代化（右侧）。

![图标变更前后对比][7]

# 总结

目前 TaurusXin 已经上手体验了 Visual Studio 2022 的正式版，热重载功能非常不错，对于提升开发效率十分重要。

Visual Studio 下载地址：https://visualstudio.microsoft.com/zh-hans/downloads/
.NET 下载地址：https://dotnet.microsoft.com/download

  [1]: https://devblogs.microsoft.com/dotnet/announcing-net-6/
  [2]: https://img.ithome.com/newsuploadfiles/2021/11/399c0960-0bcd-4678-9a54-5a6b6afbf34d.png
  [3]: https://img.ithome.com/newsuploadfiles/2021/11/9792d300-0ee0-4b22-9148-b209a80a087f.png
  [4]: https://img.ithome.com/newsuploadfiles/2021/4/74ed7324-88e3-4519-b173-882e5b890db3.gif
  [5]: https://img.ithome.com/newsuploadfiles/2021/4/ac3f0b6d-abeb-41f6-8247-e8aa9a67d0ad.gif
  [6]: https://img.ithome.com/newsuploadfiles/2021/4/f99855ce-539e-4205-8748-9900cfd3d5ce.gif
  [7]: https://img.ithome.com/newsuploadfiles/2021/11/e9d5deb8-ad65-4f16-9db4-3057b70a87e5.png