# Kirara_Server_APP

> 一款基于 WinUI 3 的 Windows 桌面客户端，用于统一使用与管理 Kirara_Server 上的各类应用服务。
> 通过 **新建实例 → 选择方案 → 链接令牌** 三步模型，实现一次配置、多服务直达、自动认证登录。

<div align="center">

![Version](https://img.shields.io/badge/version-0.9.5-blue) ![Platform](https://img.shields.io/badge/platform-Windows%2010%2B-lightgrey) ![Framework](https://img.shields.io/badge/.NET-10-purple) ![License](https://img.shields.io/badge/license-All%20Rights%20Reserved-red)

</div>

---

## ✨ 功能特性

- 🗂️ **实例化管理**：区分用户实例与服务实例，支持重命名、收藏、颜色标签、导入/导出，多实例灵活切换
- 🧩 **插件化方案体系**：20+ 内置方案，覆盖媒体、直播、云盘、邮件、团队协作、虚拟化、远程桌面、OpenVPN、证书签发、游戏服务器等场景；亦支持导入第三方可执行文件作为方案
- 🔑 **令牌管理器**：集中管理账户与证书，敏感字段经 **DPAPI + AES-256-GCM 双重加密**后落盘，支持导出/导入
- 🔐 **自动登录引擎**：基于 WebView2 的自动化登录框架，内置 Emby/Jellyfin、群晖 DSM、ESXi、爱快、OpenWrt、联想 IMM 等 13+ 方案适配，支持单步/分步/延迟登录与失败降级检测
- 🛡️ **自研 OpenVPN 管理面板**：集成 openvpn.exe 引擎，实时状态、流量、日志展示，.ovpn 配置由服务端统一同步分发
- 🔔 **实时通知中心**：SignalR WebSocket 实时推送 + 断线离线补拉 + 已读回执，通知带有信息流卡片轮播（图片/视频/HTML）
- 💬 **议题评论系统**：方案页内嵌评论区，支持回复、点赞，多端同步
- 🌈 **外观主题系统**：跟随系统 / 手动明暗 / 48 色 Win11 风格主题色，三级联动即时生效
- 🚀 **快速开始模板**：内置常见服务组模板一键套用，封面图与模板由服务端管理
- 🖥️ **沉浸式界面**：Fluent 2 设计语言、自绘标题栏、DPI 自适应（PerMonitorV2）、全局忙状态遮罩与页面转场动画


## 📋 系统要求

- **操作系统**：Windows 10 1809（10.0.17763）及以上，或 Windows 11
- **架构**：x64
- **运行时**：.NET 10（安装包通常已内置或自动引导）
- **管理员权限**：使用 OpenVPN 方案时需以管理员身份运行（创建 TAP/Wintun 虚拟网卡）

## ⬇️ 安装与更新

1. 前往本仓库 **Releases** 页面下载最新安装包（`Kirara_Server_APP_Setup_x64.exe`）
2. 运行安装程序，按向导完成安装
3. 首次启动后注册/登录 Kirara_Server 账号，即可开始创建你的第一个实例
4. 程序有自动更新程序，可以实现自动卸载旧版本安装更新


## 🚀 快速开始

### 三步上手

```
① 新建实例  →  ② 选择方案（可多选）  →  ③ 链接令牌
```

1. **新建实例**：在左侧功能栏点击「创建新用户实例」或「创建新服务实例」，命名并选择方案
2. **链接令牌**：为每个方案链接对应的账户令牌（在令牌管理器中预先创建）
3. **一键直达**：应用实例后，程序自动与服务端协商通信方式、自动填充凭据并尝试登录，成功后即可在标签页中直接使用各方案服务

> 💡 也可以使用「快速开始」模板，直接套用官方配置好的常用服务组，省去手动配置。

### 常用操作

- **实例管理**：主界面左栏集中管理；实例支持导入/导出 `.kirara` 文件分享
- **令牌管理**：创建/编辑/删除令牌，支持批量导出与导入；令牌关联的应用凭据全程加密存储
- **方案操作**：标签页内可刷新（保留会话）、硬重载（清除 Cookie/缓存）、重新链接令牌
- **通知中心**：右下角角标展开，包含「信息流 / 服务项 / 日志项」三个标签页
- **外观设置**：设置页可切换明暗主题与主题色，支持跟随系统

核心设计：

- **插件式方案**：`IScheme` 接口 + 编译期安全的 Guid→Type 映射，支持 WebView2 / RDP / CustomControl / Overlay / ExternalProcess 五种呈现方式
- **远程服务替换式接入**：所有远程服务（认证、通知、评论、模板、信息流等）均实现既有接口，通过 DI 替换注册即可接入，离线时自动降级
- **增量同步**：方案 URL、信息流卡片、.ovpn 配置均采用版本号 + 客户端上报的增量同步，网络开销极小
- **凭据安全**：RefreshToken 等敏感凭据仅以 DPAPI + AES-256-GCM 加密形式落盘，AccessToken 仅存内存并在过期前自动刷新

## ❓ 常见问题

<details>
<summary><b>提示「需要管理员权限」？</b></summary>

OpenVPN 方案需要创建虚拟网卡，应用声明了 `requireAdministrator`。请右键安装包/快捷方式选择「以管理员身份运行」。
</details>

<details>
<summary><b>网络异常时还能使用吗？</b></summary>

应用支持离线降级：本地缓存、本地认证与本地评论可作为兜底；联网后自动恢复远程同步与实时推送。
</details>

<details>
<summary><b>令牌/实例数据存在哪里？</b></summary>

数据保存在 `%APPDATA%/Kirara/` 目录下，令牌等敏感字段均已加密，请勿直接拷贝该目录给他人。
</details>

## 📄 许可证

本软件为**闭源专有软件，保留所有权利（All Rights Reserved）**。

- 未获得授权，禁止对软件进行反编译、分发、修改或再发布
- 软件使用的第三方开源组件版权归其各自作者所有

## 🙏 致谢

- [Windows App SDK / WinUI 3](https://github.com/microsoft/WindowsAppSDK)
- [CommunityToolkit.Mvvm](https://github.com/CommunityToolkit/dotnet)
- [LiteDB](https://github.com/lbedford/litedb)
- [SignalR](https://github.com/dotnet/aspnetcore)
- [OpenVPN](https://openvpn.net/)

---

<div align="center">Made with by SenVenth AC</div>
