# MultiRoute

[![Release](https://img.shields.io/github/v/release/Xposed-Modules-Repo/io.github.linoleic.multiroute?label=release)](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/releases)
[![Android](https://img.shields.io/badge/Android-11%2B-green.svg)](https://developer.android.com)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/blob/main/LICENSE)

MultiRoute 是一个 Android LSPosed 模块，提供分应用策略路由（支持主副 Wi-Fi、副卡数据与以太网）、个人热点上游选择与无线调试辅助。分流使用 Linux 内核策略路由，不使用用户态 VPN 转发。

> 本仓库存放模块索引说明和发布资源。源码、完整文档与问题反馈请访问 [Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute)。

## 功能

- **分应用策略路由**：为每个应用独立指定网络通道（主 Wi-Fi、副 Wi-Fi、蜂窝移动数据、以太网），支持应用分身与工作资料独立分流；基于 Linux 内核策略路由实现，不走用户态 VPN。
- **副 Wi-Fi 接入支持**：提供自动、小米专用、AOSP 与 Root 直连方案辅助连接副 Wi-Fi，按 BSSID 与频段区分同名接入点，作为独立分流通道。
- **副卡数据通道**：在双卡设备上激活非默认数据卡通道，支持将应用或场景按稳定 SIM 标识绑定分流。
- **个人热点上游选择**：重定向个人热点出口至蜂窝数据、副 Wi-Fi 或系统默认网络，并显示实际出口。
- **场景方案与策略**：按网络条件或手动切换分流方案，支持优先级叠加、通道故障自动降级、恢复延迟与局域网直连放行。
- **DNS 改写与诊断**：将被指派应用的 IPv4 UDP/TCP 53 端口流量改写到所选通道解析器；支持通道与模块状态诊断及日志导出。
- **无线调试辅助**：提供原生 TLS 与传统 TCP 双模式无线调试控制、自定义端口、电脑连接状态感知与安全开机恢复。
- **配置备份与恢复**：支持导入预览、按类别恢复与旧版配置迁移；导出不含 Wi-Fi 密码。

## 运行环境

| 项目 | 要求 |
| :-- | :-- |
| Root | KernelSU / Magisk / APatch |
| Xposed 框架 | LSPosed，或实现 LibXposed API 101+ 的兼容框架 |
| Android | Android 11 及以上；AOSP Multi-Internet 连接方案要求 Android 13 及以上 |
| 模块作用域 | 系统框架；热点功能还需设备对应的 NetworkStack / Tethering 包 |

NetworkStack / Tethering 包包括 `com.android.networkstack`、`com.android.networkstack.tethering`、`com.google.android.networkstack` 和 `com.google.android.networkstack.tethering`，仅启用设备中存在的对应作用域。

## 使用说明与限制

1. 在 LSPosed 中启用模块及所需作用域，然后重启设备。更新模块 APK 后也需重启以加载新代码；修改应用分流规则无需重启。
2. AOSP / 通用 Multi-Internet 方案仅连接系统已保存的网络；首次连接前，请先在系统 Wi-Fi 设置中保存。
3. 双 Wi-Fi 与热点并发取决于硬件、固件和 ROM。Root 不会增加硬件并发能力，息屏连接也受系统策略影响。
4. 副卡数据通道与双数据并发取决于芯片平台、Modem 固件与运营商网络支持（DSDA 双卡双通）。
5. 热点上游由 Android 异步重选；转发、NAT、IPv6 和硬件卸载由系统管理，实际行为受设备和运营商策略影响。
6. IPv6 DNS、Private DNS（DoT）和 DoH 不改写。VPN 下的流量与 DNS 路径取决于 VPN 模式和策略。
7. 开机恢复用于重建路由规则，不会建立副 Wi-Fi 连接；恢复时间取决于网络是否就绪。无线调试开机恢复受限于首次解锁保护与指定 Wi-Fi 环境，且不参与配置备份。

## 相关链接

- [下载与版本发布](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/releases)
- [完整中文说明](https://github.com/Linoleic/MultiRoute/blob/master/README_CN.md)
- [Full English Documentation](https://github.com/Linoleic/MultiRoute/blob/master/README.md)
- [版本更新日志](https://github.com/Linoleic/MultiRoute/blob/master/CHANGELOG.md)
- [问题反馈](https://github.com/Linoleic/MultiRoute/issues)
- [LSPosed 模块索引](https://modules.lsposed.org/module/io.github.linoleic.multiroute)

---

## English

MultiRoute is an Android LSPosed module providing per-app policy routing (primary/secondary Wi-Fi, secondary SIM data channels, and Ethernet), hotspot upstream selection, and a wireless debugging helper. Routing uses Linux kernel policy rules without userspace VPN forwarding.

> This repository hosts module index information and releases. Visit [Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute) for source code, full documentation and issue reports.

### Features

- **Per-app policy routing:** Select primary Wi-Fi, secondary Wi-Fi, cellular, or Ethernet for each app, including clones and work profiles; uses Linux kernel policy routing without userspace VPN forwarding.
- **Secondary Wi-Fi support:** Provide Auto, Xiaomi, AOSP, and Root Direct schemes to connect to secondary Wi-Fi, distinguishing same-SSID access points by BSSID and band as an independent routing channel.
- **Secondary SIM data channel:** Activate non-default data SIM channels on dual-SIM devices, routing assigned apps or scenarios by stable in-device SIM identity.
- **Hotspot upstream selection:** Redirect hotspot upstream traffic to cellular, secondary Wi-Fi, or the system default network, displaying the actual upstream.
- **Scenario plans and policies:** Switch routing plans based on network conditions or manually; support priority layering, fallback on channel unavailability, recovery delay, and on-link LAN bypass.
- **DNS redirection and diagnostics:** Redirect assigned apps' IPv4 UDP/TCP port 53 traffic to the selected channel's resolver; display channel and module status, and export diagnostic logs.
- **Wireless debugging helper:** Provide Native TLS and Legacy TCP dual-mode controls, custom listening ports, connected workstation detection, and secure boot recovery.
- **Configuration backup and restore:** Preview imports, restore by category, and migrate older configs; exported files omit Wi-Fi passwords.

### Requirements

| Item | Requirement |
| :-- | :-- |
| Root | KernelSU / Magisk / APatch |
| Xposed framework | LSPosed, or a compatible framework implementing LibXposed API 101+ |
| Android | Android 11 or later; the AOSP Multi-Internet scheme requires Android 13 or later |
| Module scope | System framework; hotspot features also require the device's NetworkStack / Tethering packages |

NetworkStack / Tethering packages include `com.android.networkstack`, `com.android.networkstack.tethering`, `com.google.android.networkstack` and `com.google.android.networkstack.tethering`. Enable the corresponding scopes that exist on the device.

### Usage and Limitations

1. Enable the module and required scopes in LSPosed, then reboot. Reboot after updating the module APK to load new code; changing app routing rules does not require a reboot.
2. AOSP / generic Multi-Internet schemes connect only to system-saved networks. Save a new network in system Wi-Fi settings first.
3. Dual Wi-Fi and hotspot concurrency depend on hardware, firmware and ROM. Root cannot add hardware concurrency, and screen-off connections remain subject to system policy.
4. Secondary SIM data channels and dual-data concurrency depend on SoC, modem firmware, and carrier network capabilities (DSDA).
5. Android reselects the hotspot upstream asynchronously and manages forwarding, NAT, IPv6 and hardware offload. Results depend on device and carrier policy.
6. IPv6 DNS, Private DNS (DoT) and DoH are not rewritten. Traffic and DNS paths with a VPN depend on its mode and policy.
7. Boot recovery rebuilds routing rules without establishing secondary Wi-Fi connections; timing depends on network availability. Wireless debugging boot recovery is subject to first-unlock protection and designated Wi-Fi networks, and is excluded from configuration backups.

### Links

- [Downloads and Releases](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/releases)
- [Full English Documentation](https://github.com/Linoleic/MultiRoute/blob/master/README.md)
- [完整中文说明](https://github.com/Linoleic/MultiRoute/blob/master/README_CN.md)
- [Changelog](https://github.com/Linoleic/MultiRoute/blob/master/CHANGELOG.md)
- [Issue Reports](https://github.com/Linoleic/MultiRoute/issues)
- [LSPosed Module Index](https://modules.lsposed.org/module/io.github.linoleic.multiroute)

## License

[GNU General Public License v3.0](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/blob/main/LICENSE)
