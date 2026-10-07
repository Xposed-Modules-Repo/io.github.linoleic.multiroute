# MultiRoute

Per-app network channels for Android: assign each app to primary Wi-Fi, secondary Wi-Fi (dual Wi-Fi),
cellular or Ethernet — enforced in the kernel with policy routing, not through a userspace VPN.

This repository only hosts the releases that
[modules.lsposed.org](https://modules.lsposed.org/module/io.github.linoleic.multiroute) indexes.
**Source code, issues and the full documentation live in
[Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute).**

- Root (KernelSU / Magisk / APatch) and LSPosed are required; the module is scoped to the system framework.
- Android 11+ (`minSdk` 30); verified on Android 16 and 17.
- Install it from the LSPosed manager, or from the APK attached to a release.

---

分应用网络通道：把每个应用指派到主 Wi-Fi、副 Wi-Fi（双 WLAN）、蜂窝或有线以太网，出口由内核策略路由落实，
不使用 VPN。本仓库只存放官方索引所需的 release；源码、问题反馈与完整文档见
[Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute)。