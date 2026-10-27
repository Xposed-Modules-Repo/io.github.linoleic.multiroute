# MultiRoute

[简体中文](README.md) | [**English**](README_EN.md)

[![Release](https://img.shields.io/github/v/release/Xposed-Modules-Repo/io.github.linoleic.multiroute?label=release)](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/releases)
[![Android](https://img.shields.io/badge/Android-11%2B-green.svg)](https://developer.android.com)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](LICENSE)

MultiRoute is an Android LSPosed module for per-app routing, secondary Wi-Fi connection management and hotspot upstream selection. Routing uses Linux kernel policy rules without userspace VPN forwarding.

> This repository hosts module index information and releases. Visit [Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute) for source code, full documentation and issue reports.

## Features

- **Per-app routing:** Select primary Wi-Fi, secondary Wi-Fi, cellular or Ethernet for an app, including clones and work profiles in separate Android user spaces.
- **Secondary Wi-Fi management:** Provide Auto, Xiaomi, AOSP and Root Direct schemes, distinguishing same-SSID access points by BSSID and band.
- **Hotspot upstream selection:** Choose cellular, secondary Wi-Fi or the system default network, and display the actual upstream.
- **Scenario plans:** Select plans by network conditions or manually, with priority-based layering.
- **Network policies:** Support fallback when a channel is unavailable, recovery delays and on-link LAN bypass.
- **DNS redirection:** Redirect assigned apps' IPv4 UDP/TCP port 53 traffic to the selected channel's resolver.
- **Configuration backup and restore:** Preview imports, restore selected categories and migrate older files. Exports omit Wi-Fi passwords.
- **Status and diagnostics:** Display network and module status, and export or share diagnostic logs.

## Requirements

| Item | Requirement |
| :-- | :-- |
| Root | KernelSU / Magisk / APatch |
| Xposed framework | LSPosed, or a compatible framework implementing LibXposed API 101+ |
| Android | Android 11 or later; the AOSP Multi-Internet scheme requires Android 13 or later |
| Module scope | System framework; hotspot features also require the device's NetworkStack / Tethering packages |

NetworkStack / Tethering packages include `com.android.networkstack`, `com.android.networkstack.tethering`, `com.google.android.networkstack` and `com.google.android.networkstack.tethering`. Enable the corresponding scopes that exist on the device.

## Usage and limitations

1. Enable the module and required scopes in LSPosed, then reboot. Reboot after updating the module APK to load new code; changing app routing rules does not require a reboot.
2. AOSP / generic Multi-Internet schemes connect only to system-saved networks. Save a new network in system Wi-Fi settings first.
3. Dual Wi-Fi and hotspot concurrency depend on hardware, firmware and ROM. Root cannot add hardware concurrency, and screen-off connections remain subject to system policy.
4. Android reselects the hotspot upstream asynchronously and manages forwarding, NAT, IPv6 and hardware offload. Results depend on device and carrier policy.
5. IPv6 DNS, Private DNS (DoT) and DoH are not rewritten. Traffic and DNS paths with a VPN depend on its mode and policy.
6. Boot recovery rebuilds routing rules without establishing secondary Wi-Fi connections; timing depends on network availability.

## Links

- [Downloads and releases](https://github.com/Xposed-Modules-Repo/io.github.linoleic.multiroute/releases)
- [Full English documentation](https://github.com/Linoleic/MultiRoute/blob/master/README.md)
- [Changelog](https://github.com/Linoleic/MultiRoute/blob/master/CHANGELOG.md)
- [Issue reports](https://github.com/Linoleic/MultiRoute/issues)
- [LSPosed module index](https://modules.lsposed.org/module/io.github.linoleic.multiroute)

## License

[GNU General Public License v3.0](LICENSE)
