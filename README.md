# MultiRoute

[**简体中文**](README.md) | [English](README_EN.md)
[![LSPosed](https://img.shields.io/badge/LSPosed-modules.lsposed.org-orange.svg)](https://modules.lsposed.org/module/io.github.linoleic.multiroute)
[![Release](https://img.shields.io/github/v/release/Linoleic/MultiRoute?label=release)](https://github.com/Linoleic/MultiRoute/releases)
[![Android](https://img.shields.io/badge/Android-11%2B%20%28%20verified%20on%2016%20%2F%2017%20%29-green.svg)](https://developer.android.com)
[![build](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml/badge.svg)](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://github.com/Linoleic/MultiRoute/blob/master/LICENSE)
**为每个应用指定独立的网络出口。**

Android 的网络栈只允许一个「系统默认网络」：即使同时连着双 WLAN 与蜂窝，没主动调用底层 API 的应用也只能
走其中一条。MultiRoute 按应用解开这个限制 —— 把应用分别指派到主 Wi-Fi、副 Wi-Fi（双 WLAN）、移动蜂窝或
有线以太网，让它们**同时**在线、各走各的；出口由内核策略路由（`ip rule`）落实，**不使用 VPN**，因此吞吐与
时延接近原生。

> 本仓库只存放 [modules.lsposed.org](https://modules.lsposed.org/module/io.github.linoleic.multiroute)
> 索引所需的 release。**源码、问题反馈与完整文档在 [Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute)。**

## 功能要点

- **分应用通道指派**：为任意应用指定链路，其流量从该链路出口，其他应用完全不受影响。
- **分身独立配置**：厂商分身空间（如小米 XSpace，用户 999）与工作资料作为独立条目，可与主安装走不同通道。
- **DNS 随通道走**：被指派应用发往 53 端口的查询改写到该通道自己的解析器（853/DoH 不改写）。
- **开机自动恢复**：`service.d` 脚本 + 模块唤醒广播 + 应用自身网络回调三重互补；重启后仍锁屏时由 root 脚本恢复。
- **副 Wi-Fi 息屏保活**（可选，仅小米）：避免息屏后副 WLAN 被 OEM 省电策略拆除。
- **局域网直连放行**：内网设备（NAS、打印机、投屏）在被指派到其他通道的应用里依然可达。
- **诊断可转交**：一键复制含「规则生效对照」与开机恢复日志的快照，报告问题时无需再翻 adb。

## 当前支持的版本

| 项目 | 要求 |
| :-- | :-- |
| Root | KernelSU / Magisk / APatch |
| Xposed 框架 | LSPosed（或实现了 LibXposed API 101+ 的框架） |
| 模块作用域 | **仅系统框架**（`system` / `system_server`） |
| Android | **11 及以上**（`minSdk` 30）；已在 **Android 16 与 17（HyperOS）** 实机验证 |

Android 11–15 理论可行但**尚未实测**；Android 10 及更低**不支持** —— 模块所 hook 的框架接口在那里不存在。

## 使用前说明

1. 安装后在 **LSPosed 管理器**中启用，并将作用域设为**系统框架**，然后重启设备（或
   `su -c 'setprop ctl.restart zygote'`）。
2. **更新模块后需要重启**：模块注入系统框架，无法热重载（界面会提示「已加载旧版本，需软重启」）。
   日常增删改分流规则**无需**重启。
3. 与 VPN 同时使用时：全流量 VPN 下，被指派的应用会**离开隧道**；always-on VPN 且开启「阻止无 VPN 连接」时
   VPN 优先，指派不生效。
4. **从 1.1.x 升级**：包名已由 `com.multiroute` 改为 `io.github.linoleic.multiroute`，系统视其为不同应用 ——
   请先卸载旧版本，再在管理器中启用新包名条目（分流配置不会迁移）。本仓库中带「旧版 / legacy」标注的 release
   即为旧包名版本，一般用户无需安装。

## 反馈

功能请求与 BUG 反馈请到源码仓库：<https://github.com/Linoleic/MultiRoute/issues>。
附上应用内 **设置 → 复制诊断日志** 的内容会快很多。

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