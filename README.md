# 天船五 (Persei-δ-B5III)

<div align="center">

**简体中文** | [English](./README_EN.md)

![Platform](https://img.shields.io/badge/Platform-Windows%2010%2B%20%7C%20x64-0078D6?style=flat-square&logo=windows)
![.NET Version](https://img.shields.io/badge/.NET-10.0%20(WinUI%203)-512BD4?style=flat-square&logo=dotnet)
![Windows App SDK](https://img.shields.io/badge/Windows%20App%20SDK-1.6%2B-blue?style=flat-square)
![License](https://img.shields.io/badge/License-Non--Commercial%20Research-grey?style=flat-square)

<p align="center">
  <b>Aether Gazer 本地仿真服务桌面启动宿主与看门狗系统 (Desktop Host & Process Supervisor)</b>
</p>

</div>

> 📖 **项目杂谈 / 开发者手记**：  
> 想了解本项目立项背后的心路历程、开发故事与作者的碎碎念？欢迎阅读博文：[《关于LocalServer》- MoriaRuRuka](https://moriaruruka.com/2026/09/24/411/)。

---

## 模块定位

`Persei-δ-B5III`（天船五）是 **Alpha Persei Cluster** 体系中的桌面宿主与进程看门狗仓库。采用 **Windows App SDK (WinUI 3) + .NET 10** 构建，主要负责对底层的 Python 服务端进行环境检测、端口扫描、子进程生命周期托管与管理面板内嵌展示。

---

## 核心技术特性

1. **Windows 作业对象内核级生命周期托管 (Job Object Supervisor)**：
   * 基于 Win32 原生 `CreateJobObject` 与 `SetInformationJobObject`，配置 `JOB_OBJECT_LIMIT_KILL_ON_JOB_CLOSE` 标志位；
   * 将启动的 Python 服务端主进程及衍生子进程关联绑定至 Job Object；
   * 当桌面宿主无论由于正常关闭、系统注销或异常崩溃退出时，Windows 内核会自动终止并回收绑定的子进程，有效防止孤儿进程长期驻留占用 443、8102、8105 等网络端口。
2. **内嵌式 WebView2 运维视窗**：
   * 集成 Microsoft Edge WebView2 组件，在后端服务健康就绪后自动加载管理控制面板，提供免浏览器弹出的原生集成体验。
3. **异步标准流重定向与管道节流**：
   * 异步捕获 Python 子进程的标准输出（stdout）与标准错误（stderr），并在 UI 文本控件中流式滚动呈现服务运行动向。
4. **服务环境与端口冲突扫描**：
   * 启动前预检 443、80、8102、8105、6105 等端口占用情况，辅助用户识别本地占用冲突并执行回收。

---

## 构建与运行

### 前置要求
* **操作系统**：Windows 10 (Build 19041+) 或 Windows 11；
* **开发环境**：
  * [.NET 10.0 SDK](https://dotnet.microsoft.com/)
  * [Windows App SDK 1.6+](https://learn.microsoft.com/en-us/windows/apps/windows-app-sdk/)
  * Microsoft Edge WebView2 Runtime (Windows 11 默认已集成)

### 编译与启动
```powershell
# 还原依赖并以 Release 配置编译运行
dotnet run -c Release
```

如需发布为单文件便携绿色版：
```powershell
dotnet publish -c Release -r win-x64 --self-contained true -p:PublishSingleFile=true
```

---

## 免责声明

本项目仅供 Windows 桌面应用程序开发、系统作业对象进程托管及逆向工程学习与研究使用。严禁用于任何商业目的。
