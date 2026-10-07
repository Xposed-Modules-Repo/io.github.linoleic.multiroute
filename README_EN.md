# MultiRoute

[简体中文](README.md) | [**English**](README_EN.md)
[![LSPosed](https://img.shields.io/badge/LSPosed-modules.lsposed.org-orange.svg)](https://modules.lsposed.org/module/io.github.linoleic.multiroute)
[![Release](https://img.shields.io/github/v/release/Linoleic/MultiRoute?label=release)](https://github.com/Linoleic/MultiRoute/releases)
[![Android](https://img.shields.io/badge/Android-11%2B%20%28%20verified%20on%2016%20%2F%2017%20%29-green.svg)](https://developer.android.com)
[![build](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml/badge.svg)](https://github.com/Linoleic/MultiRoute/actions/workflows/build.yml)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://github.com/Linoleic/MultiRoute/blob/master/LICENSE)
**Give every app its own network egress.**

Android allows exactly one system-wide default network, so an app that does not bind to a network itself
can only ever use that one - even with dual Wi-Fi and cellular connected at the same time. MultiRoute
lifts that per app: assign each app to primary Wi-Fi, secondary Wi-Fi (dual Wi-Fi), cellular or Ethernet
and let them stay online **at the same time**, each through its own link. Egress is enforced by kernel
policy routing (`ip rule`), with **no VPN**, so throughput and latency stay close to native.

> This repository only hosts the releases that
> [modules.lsposed.org](https://modules.lsposed.org/module/io.github.linoleic.multiroute) indexes.
> **Source code, issues and the full documentation live in
> [Linoleic/MultiRoute](https://github.com/Linoleic/MultiRoute).**

## Highlights

- **Per-app channel assignment** - pick a link for any app; its traffic egresses there while other apps
  keep using theirs, untouched.
- **Independent configuration for cloned apps** - OEM clone spaces (e.g. Xiaomi XSpace, user 999) and work
  profiles are listed separately, so a clone and its primary install can use different channels.
- **DNS follows the channel** - queries to port 53 of an assigned app are rewritten to that channel's own
  resolver (853/DoH are deliberately left alone).
- **Automatic boot recovery** - a generated `service.d` script, a wake-up broadcast from the module and the
  app's own network callback cover each other; the root script is what restores rules on a locked device.
- **Screen-off secondary Wi-Fi keep-alive** (optional, Xiaomi only) - stops the OEM power policy from
  tearing the secondary WLAN down when the screen turns off.
- **LAN bypass** - intranet devices (NAS, printers, casting) stay reachable from apps assigned elsewhere.
- **A diagnostic snapshot you can hand over** - rule effectiveness (configured vs. actually in the kernel)
  and the boot-recovery log, copied in one tap.

## Compatibility

| Item | Requirement |
| :-- | :-- |
| Root | KernelSU / Magisk / APatch |
| Xposed | LSPosed (or any framework implementing LibXposed API 101+) |
| Module scope | **System framework only** (`system` / `system_server`) |
| Android | **11 or newer** (`minSdk` 30); verified on device with **Android 16 and 17 (HyperOS)** |

Android 11-15 is plausible but **untested**; Android 10 and below is **not supported** - the framework
interfaces the module hooks do not exist there.

## Before you start

1. After installing, enable the module in **LSPosed Manager**, set the scope to the **system framework**,
   then reboot (or `su -c 'setprop ctl.restart zygote'`).
2. **Updating the module needs a reboot**: it injects into the system framework, which cannot hot-reload
   (the UI says "loaded older build - soft reboot required"). Changing app assignments never needs one.
3. With a VPN: under a full-tunnel client an assigned app **leaves the tunnel**; with an always-on VPN that
   blocks connections without VPN, the VPN wins and the assignment has no effect.
4. **Upgrading from 1.1.x**: the application id changed from `com.multiroute` to
   `io.github.linoleic.multiroute`, which Android treats as a different app - uninstall the old version and
   enable the new package, which appears as a new module entry (assignments are not carried over). Releases
   marked "旧版 / legacy" in this repository are those old-id builds; most users do not need them.

## Feedback

Feature requests and bug reports: <https://github.com/Linoleic/MultiRoute/issues>.
Attaching the app's **Settings - Copy diagnostic log** output speeds things up a lot.

## Links

- Source and full documentation: <https://github.com/Linoleic/MultiRoute>
- Changelog: <https://github.com/Linoleic/MultiRoute/blob/master/CHANGELOG.md>
- On-device verification record (including what is not covered): <https://github.com/Linoleic/MultiRoute/blob/master/docs/VERIFICATION.md>
- Listing: <https://modules.lsposed.org/module/io.github.linoleic.multiroute>

## License

[GNU General Public License v3.0](https://github.com/Linoleic/MultiRoute/blob/master/LICENSE)

## Acknowledgements

- [LibXposed API](https://github.com/libxposed) and [LSPosed](https://github.com/LSPosed/LSPosed) for the
  framework interfaces this module builds on.
- Jetpack Compose and Material 3 for the UI.