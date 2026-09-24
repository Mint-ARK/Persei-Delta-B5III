# Persei-δ-B5III

<div align="center">

[简体中文](./README.md) | **English**

![Platform](https://img.shields.io/badge/Platform-Windows%2010%2B%20%7C%20x64-0078D6?style=flat-square&logo=windows)
![.NET Version](https://img.shields.io/badge/.NET-10.0%20(WinUI%203)-512BD4?style=flat-square&logo=dotnet)
![Windows App SDK](https://img.shields.io/badge/Windows%20App%20SDK-1.6%2B-blue?style=flat-square)
![License](https://img.shields.io/badge/License-Non--Commercial%20Research-grey?style=flat-square)

<p align="center">
  <b>Aether Gazer Desktop Host & Process Supervisor Launcher (WinUI 3 / .NET 10)</b>
</p>

</div>

> 📖 **Developer's Note / Behind the Scenes**:  
> Curious about the story, motivations, and journey behind this project? Check out the author's blog post: [About LocalServer - MoriaRuRuka (Chinese)](https://moriaruruka.com/2026/09/24/411/).

---

## Module Scope

`Persei-δ-B5III` (Delta Persei) serves as the desktop host and process supervisor repository within the **Alpha Persei Cluster** ecosystem.

Built with **Windows App SDK (WinUI 3) + .NET 10**, this application handles local environment preflight checks, port conflict detection, Python child process lifecycle supervision, and dashboard rendering via an embedded WebView2 view.

---

## Key Technical Features

1. **Kernel-Level Process Lifecycle Management (Job Object Supervisor)**:
   * Uses Win32 `CreateJobObject` and `SetInformationJobObject` with `JOB_OBJECT_LIMIT_KILL_ON_JOB_CLOSE`;
   * Binds the spawned Python server process and all its child workers to the Windows Job Object;
   * Guarantees that if the desktop GUI exits (whether through normal closure, system logoff, or unexpected crashes), the operating system kernel automatically terminates the attached backend processes, preventing orphan processes from locking network ports (such as 443, 8102, and 8105).
2. **Embedded WebView2 Dashboard**:
   * Integrates Microsoft Edge WebView2 to render the management web interface immediately upon backend readiness, providing a native, self-contained desktop experience.
3. **Asynchronous Stream Redirection**:
   * Asynchronously redirects Python stdout and stderr streams into an in-app terminal window, enabling real-time operation monitoring without external console windows.
4. **Port & Environment Scanner**:
   * Scans target ports (443, 80, 8102, 8105, 6105) prior to launch, assisting users in identifying and resolving local port conflicts.

---

## Build & Run

### Prerequisites
* **Operating System**: Windows 10 (Build 19041+) or Windows 11;
* **Development Environment**:
  * [.NET 10.0 SDK](https://dotnet.microsoft.com/)
  * [Windows App SDK 1.6+](https://learn.microsoft.com/en-us/windows/apps/windows-app-sdk/)
  * Microsoft Edge WebView2 Runtime (pre-installed on Windows 11)

### Build & Debug
```powershell
# Restore dependencies and run with Release configuration
dotnet run -c Release
```

To build a standalone single-file binary:
```powershell
dotnet publish -c Release -r win-x64 --self-contained true -p:PublishSingleFile=true
```

---

## Disclaimer

This project is intended strictly for personal research and educational study in Windows desktop software engineering, system job object process management, and reverse engineering. Commercial use is strictly prohibited.
