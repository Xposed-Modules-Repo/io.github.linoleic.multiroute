# MultiRoute

[**简体中文**](README.md) | [English](README_EN.md)

[![Release](https://img.shields.io/github/v/release/Linoleic/MultiRoute?label=release)](https://github.com/Linoleic/MultiRoute/releases)
[![Android](https://img.shields.io/badge/Android-11%2B%20%28%20verified%20on%2016%20%2F%2017%20%29-green.svg)](https://developer.android.com)
[![build](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml/badge.svg)](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://github.com/Linoleic/MultiRoute/blob/master/LICENSE)

**为每个应用指定独立的网络出口。**

Android 默认只允许一个全局默认网络，即使同时连接主副 Wi-Fi 与蜂窝数据，普通应用也只能走其中一条。MultiRoute 基于 Xposed 框架解开这一限制 —— 将应用分别指派到主 Wi-Fi、副 Wi-Fi（双 WLAN）、移动蜂窝或有线以太网，实现多网络并行出口。流量由内核策略路由（`ip rule`）接管，**不使用 VPN**，保持原生的吞吐与极低时延。

> 本仓库仅存放 LSPosed 模块仓库索引所需的 Release 资源。**源码、问题反馈与完整文档请访问主仓库：[Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute)。**

## 核心功能

- **分应用通道指派**：自由指定应用出口链路，未指派应用保持系统默认路由。
- **内置双 Wi-Fi 助手**：打破系统对副 WLAN 的手动连接限制，支持自动探测、厂商专用、系统覆盖及 Root 直连等多种方案；支持同名热点分频选择与异常惩罚清除。
- **自动容灾回退**：指派链路断开时自动切换至默认网络借道保活，链路恢复后自动迁回。
- **分身与多用户支持**：厂商双开（如小米应用双开）与工作资料独立列出，可分别配置不同出口。
- **DNS 随通道走**：指派应用的基础 DNS 查询重定向至对应链路的解析器（保留安全 DNS）。
- **局域网直连放行**：内网设备（NAS、打印机、本地服务）在被指派到蜂窝等通道时仍可直连。
- **开机自动恢复**：开机自启脚本与系统广播协同恢复分流规则，重启后在锁屏状态即可生效。
- **系统诊断日志**：一键生成路由表、规则生效状态与日志快照，支持系统分享与本地文件导出。

## 运行环境

| 项目 | 要求 |
| :-- | :-- |
| Root 环境 | KernelSU / Magisk / APatch |
| Xposed 框架 | LSPosed（或实现 LibXposed API 101+ 的兼容框架） |
| 模块作用域 | **仅系统框架**（`system` / `system_server`） |
| 系统版本 | **Android 11 及以上**（`minSdk` 30）；已在 **Android 16 与 17（HyperOS）** 实机验证 |

## 使用须知

1. **激活模块**：在 LSPosed 管理器中勾选本模块，作用域勾选**系统框架**，然后重启系统（或软重启 Zygote）。
2. **模块更新**：更新模块 APK 后必须重启系统框架才能加载新代码；增删或修改应用分流规则**无需**重启。
3. **VPN 兼容性**：在全局 VPN 模式下，被指派的应用将绕过 VPN 隧道；若开启了系统的「始终开启的 VPN 并阻止没有 VPN 的连接」，系统将优先由 VPN 接管。

## 相关链接

- 源码与完整文档：[Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute)
- 版本更新日志：[CHANGELOG.md](https://github.com/Linoleic/MultiRoute/blob/master/CHANGELOG.md)
- 问题与反馈：[GitHub Issues](https://github.com/Linoleic/MultiRoute/issues)
- 模块索引：[LSPosed 模块仓库](https://modules.lsposed.org/module/io.github.linoleic.multiroute)

## 许可协议

[GNU General Public License v3.0](https://github.com/Linoleic/MultiRoute/blob/master/LICENSE)
