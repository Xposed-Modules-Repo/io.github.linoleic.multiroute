# MultiRoute

[**简体中文**](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/blob/main/README.md) | [English](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/blob/main/README_EN.md)

[![Release](https://img.shields.io/github/v/release/Xposed-Modules-Repo/io.github.linoleic.multiroute?label=release)](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/releases)
[![Android](https://img.shields.io/badge/Android-11%2B-green.svg)](https://developer.android.com)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/blob/main/LICENSE)

MultiRoute 是一个 Android LSPosed 模块，提供分应用路由、副 Wi-Fi 连接管理和个人热点上游选择。分流使用 Linux 内核策略路由，不使用用户态 VPN 转发。

> 本仓库存放模块索引说明和发布资源。源码、完整文档与问题反馈请访问 [Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute)。

## 功能

- **分应用路由**：为应用选择主 Wi-Fi、副 Wi-Fi、蜂窝或以太网；支持独立用户空间中的分身和工作资料。
- **副 Wi-Fi 管理**：提供自动、小米专用、AOSP 和 Root 直连方案，按 BSSID 与频段区分同名接入点。
- **个人热点上游选择**：选择蜂窝、副 Wi-Fi 或系统默认网络，并显示实际出口。
- **场景方案**：按网络条件或手动选择分流方案，支持按优先级叠加。
- **网络策略**：提供通道不可用时的回退、恢复延迟和局域网直连放行。
- **DNS 改写**：将被指派应用的 IPv4 UDP/TCP 53 端口流量改写到所选通道的解析器。
- **配置备份与恢复**：支持导入预览、按类别恢复和旧版配置迁移；导出不含 Wi-Fi 密码。
- **状态与诊断**：显示网络通道和模块状态，支持诊断日志导出与分享。

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
4. 热点上游由 Android 异步重选；转发、NAT、IPv6 和硬件卸载由系统管理，实际行为受设备和运营商策略影响。
5. IPv6 DNS、Private DNS（DoT）和 DoH 不改写。VPN 下的流量与 DNS 路径取决于 VPN 模式和策略。
6. 开机恢复用于重建路由规则，不会建立副 Wi-Fi 连接；恢复时间取决于网络是否就绪。

## 相关链接

- [下载与版本发布](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/releases)
- [完整中文说明](https://github.com/Linoleic/MultiRoute/blob/master/README_CN.md)
- [版本更新日志](https://github.com/Linoleic/MultiRoute/blob/master/CHANGELOG.md)
- [问题反馈](https://github.com/Linoleic/MultiRoute/issues)
- [LSPosed 模块索引](https://modules.lsposed.org/module/io.github.linoleic.multiroute)

## 许可协议

[GNU General Public License v3.0](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/blob/main/LICENSE)
