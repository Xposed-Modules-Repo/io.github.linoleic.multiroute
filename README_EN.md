# MultiRoute

[简体中文](README.md) | [**English**](README_EN.md)

[![Release](https://img.shields.io/github/v/release/Linoleic/MultiRoute?label=release)](https://github.com/Linoleic/MultiRoute/releases)
[![Android](https://img.shields.io/badge/Android-11%2B%20%28%20verified%20on%2016%20%2F%2017%20%29-green.svg)](https://developer.android.com)
[![build](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml/badge.svg)](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://github.com/Linoleic/MultiRoute/blob/master/LICENSE)

**Give every app its own network egress.**

Android restricts network traffic to a single default interface. Even when connected to dual Wi-Fi and mobile data simultaneously, applications cannot utilize multiple networks concurrently without custom socket programming. MultiRoute lifts this limitation via Xposed: assign applications individually to Primary Wi-Fi, Secondary Wi-Fi (Dual WLAN), Cellular, or Ethernet. Network routing is handled directly by kernel policy routing (`ip rule`), requiring **no VPN service**, ensuring native throughput and ultra-low latency.

> This repository only hosts the releases indexed by the LSPosed module repository. **For source code, issue tracking, and comprehensive documentation, please visit the main repository: [Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute).**

## Features

- **Per-App Routing**: Route specific apps through dedicated network interfaces while unassigned apps use the default route.
- **Built-in Dual Wi-Fi Manager**: Connect to secondary Wi-Fi manually without OEM restrictions, featuring 4 connection schemes (Auto, OEM/Xiaomi Slave, AOSP Overlay, Root Direct `SYSTEM_UID`), multi-band SSID selection, and BSSID penalty clearing.
- **Automatic Failover**: Automatically reroutes traffic through the default network when the assigned interface disconnects, and seamlessly migrates back once restored.
- **App Clones & Multi-User Support**: Independently configure cloned apps (e.g., Xiaomi Dual Apps) and work profiles.
- **Per-Interface DNS**: Automatically redirects port 53 DNS queries to the assigned interface's upstream resolver (secure DNS preserved).
- **LAN Bypass**: Allows direct access to local subnet devices (NAS, printers, casting) even when an app is routed through mobile data.
- **Automatic Boot Recovery**: Restores routing rules automatically on system boot via root scripts and system broadcasts.
- **System Diagnostics**: One-tap diagnostic log export with route table status, kernel rules, and logcat snapshots via SAF and system sharing.

## Compatibility

| Item | Requirement |
| :-- | :-- |
| Root Solution | KernelSU / Magisk / APatch |
| Xposed Framework | LSPosed (or compatible framework supporting LibXposed API 101+) |
| Module Scope | **System Framework only** (`system` / `system_server`) |
| Android Version | **Android 11+** (`minSdk` 30); verified on **Android 16 and 17 (HyperOS)** |

## Notes

1. **Activation**: Enable MultiRoute in LSPosed Manager, check **System Framework** in scope, then reboot the device (or soft restart Zygote).
2. **Module Updates**: A system reboot is required after updating the module APK to load new hook bytecode. Modifying routing rules in the app does **not** require a reboot.
3. **VPN Coexistence**: With a full-tunnel VPN, assigned apps will bypass the VPN tunnel. If Android's "Always-on VPN with block connections without VPN" is enabled, the VPN takes precedence.

## Links

- Source & Documentation: [Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute)
- Changelog: [CHANGELOG.md](https://github.com/Linoleic/MultiRoute/blob/master/CHANGELOG.md)
- Issues & Support: [GitHub Issues](https://github.com/Linoleic/MultiRoute/issues)
- Module Repository: [LSPosed Repository](https://modules.lsposed.org/module/io.github.linoleic.multiroute)

## License

[GNU General Public License v3.0](https://github.com/Linoleic/MultiRoute/blob/master/LICENSE)
