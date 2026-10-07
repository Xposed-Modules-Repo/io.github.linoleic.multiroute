# MultiRoute

[![LSPosed](https://img.shields.io/badge/LSPosed-modules.lsposed.org-orange.svg)](https://modules.lsposed.org/module/io.github.linoleic.multiroute)
[![Release](https://img.shields.io/github/v/release/Linoleic/MultiRoute?label=release)](https://github.com/Linoleic/MultiRoute/releases)
[![Android](https://img.shields.io/badge/Android-11%2B%20%28%20verified%20on%2016%20%2F%2017%20%29-green.svg)](https://developer.android.com)
[![build](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml/badge.svg)](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://github.com/Linoleic/MultiRoute/blob/master/LICENSE)

**让每个应用走各自的网络通道 —— 一个 LSPosed 模块。**

把应用分别指派到主 Wi-Fi、副 Wi-Fi（双 WLAN）、移动蜂窝或有线以太网，让它们**同时**通信。
出口由内核策略路由（`ip rule`）落实，**不使用 VPN**，吞吐与时延接近原生。

> 本仓库只存放 [modules.lsposed.org](https://modules.lsposed.org/module/io.github.linoleic.multiroute)
> 索引所需的 release。**源码、问题反馈与完整文档在 [Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute)。**

## 功能要点

- **分应用通道指派**：为任意应用指定链路，其流量从该链路出口，其他应用不受影响。
- **分身独立配置**：厂商分身空间（如小米 XSpace，用户 999）与工作资料作为独立条目，可与主安装走不同通道。
- **DNS 随通道走**：被指派应用发往 53 端口的查询会改写到该通道自己的解析器（853/DoH 不改写）。
- **开机自动恢复**：`service.d` 脚本 + 模块唤醒广播 + 应用自身网络回调三重互补；重启后仍锁屏时由 root 脚本恢复。
- **副 Wi-Fi 息屏保活**（可选，仅小米）：避免息屏后副 WLAN 被 OEM 省电策略拆除。
- **局域网直连放行**：内网设备（NAS、打印机、投屏）在被指派到其他通道的应用里依然可达。
- **诊断可转交**：一键复制含「规则生效对照」与开机恢复日志的快照。

## 当前支持的版本

| 项目 | 要求 |
| :-- | :-- |
| Root | KernelSU / Magisk / APatch |
| Xposed 框架 | LSPosed（或实现了 LibXposed API 101+ 的框架） |
| 模块作用域 | **仅系统框架**（`system` / `system_server`） |
| Android | **11 及以上**（`minSdk` 30）；已在 **Android 16 与 17（HyperOS）** 实机验证 |

Android 11–15 理论可行但**尚未实测**；Android 10 及更低**不支持**（缺少模块所 hook 的框架接口）。

## 使用前说明

1. 安装后在 **LSPosed 管理器**中启用，并把作用域设为**系统框架**，然后重启设备（或
   `su -c 'setprop ctl.restart zygote'`）。
2. **更新模块后需要重启**：模块注入系统框架，无法热重载（界面会提示"已加载旧版本，需软重启"）。
   日常增删改分流规则**无需**重启。
3. 与 VPN 同时使用时：全流量 VPN 下，被指派的应用会**离开隧道**；always-on VPN 且开启
   "阻止无 VPN 连接"时 VPN 优先，指派不生效。
4. **从 1.1.x 升级**：包名已由 `com.multiroute` 改为 `io.github.linoleic.multiroute`，两者被系统视为
   不同应用 —— 请先卸载旧版本，再在管理器中启用新包名条目（分流配置不会迁移）。

## 反馈

功能请求与 BUG 反馈请到源码仓库：<https://github.com/Linoleic/MultiRoute/issues>。
附上应用内 **设置 → 复制诊断日志** 的内容会更快定位问题。

## 相关链接

- 源码与完整文档：<https://github.com/Linoleic/MultiRoute>
- 更新日志：<https://github.com/Linoleic/MultiRoute/blob/master/CHANGELOG.md>
- 真机验证记录（含尚未验证项）：<https://github.com/Linoleic/MultiRoute/blob/master/docs/VERIFICATION.md>
- 官方索引页：<https://modules.lsposed.org/module/io.github.linoleic.multiroute>

## 许可

[GNU General Public License v3.0](https://github.com/Linoleic/MultiRoute/blob/master/LICENSE)

## 致谢

- [LibXposed API](https://github.com/libxposed) 与 [LSPosed](https://github.com/LSPosed/LSPosed)：模块所依赖的框架接口。
- Jetpack Compose 与 Material 3：界面实现。

---

## English

**MultiRoute — an LSPosed module that gives every app its own network channel.** Assign each app to
primary Wi-Fi, secondary Wi-Fi (dual Wi-Fi), cellular or Ethernet and let them communicate at the same
time; egress is enforced in the kernel with policy routing, not through a userspace VPN.

This repository only hosts the releases indexed by modules.lsposed.org. Source code, issues and the full
documentation live in <https://github.com/Linoleic/MultiRoute>.

- Requires root (KernelSU / Magisk / APatch) and LSPosed, scoped to the **system framework** only.
- Android 11+ (`minSdk` 30), verified on Android 16 and 17. Android 11–15 is untested; Android 10 and
  below is not supported, because the framework interfaces the module hooks do not exist there.
- Enable the module in LSPosed Manager, set the scope to the system framework, then reboot. Changing app
  assignments never needs a reboot; updating the module always does.
- Upgrading from 1.1.x: the application id changed to `io.github.linoleic.multiroute`, so uninstall the
  older version first and enable the new package, which appears as a new module entry.