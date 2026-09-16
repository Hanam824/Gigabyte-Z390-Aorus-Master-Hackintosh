# Tahoe Gigabyte Z390 Aorus Master (OpenCore)

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.7-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOS-Stable](https://img.shields.io/badge/macOS-26.6-brightgreen.svg)](https://www.apple.com/macos/macos-tahoe)

<img width="392" height="660" alt="Tahoe266" src="https://github.com/user-attachments/assets/7eaae57f-1dae-40dd-bf61-d7af6cbf7731" />

## Table of Contents
- [Quick Start](#quick-start)
- [My PC Build](#my-pc-build)
- [Installation Guide](#installation-guide)
- [USB Port Map](#usb-port-map)
- [Hardware Acceleration](#hardware-acceleration)
- [Resources](#resources)
- [Tools](#tools)
- [History](#history)
- [Thanks](#thanks)

## Quick Start
New to this repo? Follow these steps in order:

1. **Check compatibility** — compare your parts against [My PC Build](#my-pc-build). The closer your hardware matches, the fewer changes you'll need.
2. **Configure BIOS** — see [BIOS Setup](#bios-setup) below (VT-d on, Secure Boot/Security off).
3. **Build a USB installer** — follow [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) to create the installer, then replace its `EFI` folder with the one from this repo. Adjust `EFI/OC/config.plist` (SMBIOS, USB map) for your own machine before booting — see [USB Port Map](USB_MAP.md) if your port layout differs.
4. **Install macOS** — boot from the USB. If you hit a reboot loop, see [During Installation](#during-installation).
5. **Finish post-install setup** — [SMBIOS](#change-smbios), [audio](#no-sound), [Wi-Fi/Bluetooth](#wifibluetooth), and [power management](#power-management).
6. **Confirm what works** — check [Working ✅ / Not Working ☑️](#my-pc-build) and the [Kernel extensions](#my-pc-build) list against your own EFI.

If something doesn't match your hardware (Wi-Fi card, GPU, storage), read the relevant subsection below before assuming the default config will just work.

## My PC Build
<details>
  <summary><strong>Hardware</strong></summary>
  
  | Category          | Component                                                | Note                                                  |
  | ----------------- | -------------------------------------------------------  | ----------------------------------------------------- |
  | CPU               | Intel Core i9-9900K                                      |                                                       |
  | GPU               | ASUS TUF GAMING Radeon™ RX 6900 XT OC Edition            | Native support, WhateverGreen kext not needed         |
  | Motherboard       | Gigabyte Z390 AORUS MASTER                               |                                                       |
  | Storage (Windows) | Crucial P1 500GB 3D NAND NVMe PCIe (`M2M` slot)          | Internal NVME                                         |
  | Storage (macOS)   | Toshiba BG4 KBG40ZNT512G NVMe      (`M2A` slot)          | Internal NVME                                         |
  | Memory            | Corsair Vengeance LPX 64GB (4x16GB) 3200MHz DDR4         |                                                       |
  | CPU Cooler        | EKWB EK-KIT Performance Series PC Watercooling Kit P360  |                                                       |
  | Power Supply      | Corsair RMX Series 80PLUS Gold 1000W                     |                                                       |
  | Case              | Cooler Master MasterCase H500M ARGB                      |                                                       |
  | Monitor           | LG 27UP600K-W 27inch 4k                                  |                                                       |
  | LAN               | Intel® i219v GbE LAN                                     | I use LAN for network                                 |
  | Wifi & BT         |~~Intel® CNVi 802.11ac 2x2 Wave 2 WIFI & BT5  (on-board)~~| ~~I just use bluetooth for JBL FLIP 5 Speaker.~~      |
  |                   |~~Include **Intel Wireless-AC 9560** module inside~~      | ~~If you want native wifi control.~~                  |
  |                   | Replace Intel Wifi Card with BCM943602CS on PCIe port    | ~~Use AirportItlwm instead but slow [speed](image)~~  |
  |                   | and connect on F-USB2 (HS13) for bluetooth               | ~~Use Itlwm and HeliPort for increase wifi speed~~    |

  [For more information](https://pcpartpicker.com/list/4F9K2k)
  
</details>
<details>

<summary><strong>Kernel extensions</strong></summary>
<br>

| Kext                   | Version        |
|:---------------------- | -------------- |
| Lilu                   | 1.7.2          |
| VirtualSMC             | 1.3.7          |
| SMCProcessor           | 1.3.7          |
| SMCSuperIO             | 1.3.7          |
| RestrictEvents         | 1.1.6          |
| ~~WhateverGreen~~      | ~~1.7.0~~      |
| AppleALC               | 1.9.7          |
| IntelBluetoothFirmware | 2.4.0          |
| IntelBTPatcher         | 2.4.0          |
| IntelMausiEthernet     | 3.0.3          |
| USBMap                 | Manual         |
| AMFIPass               | 1.4.1          |
| IOSkywalkFamily        | 1.0            |
| IO80211FamilyLegacy    | 1200.12.2b1    |
| BlueToolFixup          | 2.7.0          |
| BroadcomVTD ⚠️ EXPERIMENTAL | 0.2.17    |

</details>
<details>
  <summary><strong>Working ✅ / Not Working ☑️</strong></summary>
  
  * ✅ Ethernet
  * ✅ Onboard Audio
  * ✅ iMessage
  * ✅ Sleep/Wake
  * ✅ Bluetooth & Wi-Fi
  * ✅ Airdrop
  * ✅ Handoff
  
</details>
<details>
  <summary><strong>Known Issues</strong></summary>

  * **AppleVTD (VT-d) + legacy Wi-Fi/BT rollback can cause Ethernet connect/disconnect loops on Tahoe.** This EFI rolls back `IOSkywalkFamily.kext` (via OCLP-CustoMac's Modern Wireless patch) to keep the on-board Broadcom Wi-Fi/BT working, while also keeping AppleVTD enabled (`DisableIoMapper=false`) with a patched `SSDT-DMAR.aml`. The `IntelMausiEthernet` maintainer confirms this exact combination is a known trigger for Ethernet flakiness under Tahoe, on hardware nearly identical to this build (Z390 Designare, i9/i7-9900K, Intel I219 LAN, Broadcom Wi-Fi via OCLP) — see [Mieze/IntelMausiEthernet#50](https://github.com/Mieze/IntelMausiEthernet/issues/50) (unresolved, auto-closed as stale).
  * `BroadcomVTD.kext` ([kgp-macPro/BroadcomVTD-Tahoe](https://github.com/kgp-macPro/BroadcomVTD-Tahoe)) is included as an **experimental** attempt to let legacy Broadcom Wi-Fi coexist with AppleVTD without disabling it — it has no Z390 validation yet and does not address the Ethernet issue above. See [History](#history) for test results as they come in.
  * If Ethernet instability persists, the documented fallback is setting `DisableIoMapper=true` (disables AppleVTD/IOMMU entirely) — this is the only combination multiple reporters confirm as reliably stable, at the cost of VT-d.

</details>

## Installation Guide
Tested on macOS Tahoe 26. Steps are in the order you'll hit them during a fresh install.

<details open>
  <summary><strong>BIOS Setup (F13a)</strong></summary>
  
  * Enable VT-d
  * Disable Security

</details>
<details>
  <summary><strong>During Installation</strong></summary>
  
  * Unplug wired ethernet if you get a reboot loop like below
    * <img width="750" height="1000" alt="patch-macos-increament" src="https://github.com/user-attachments/assets/8cb95a92-7caa-443e-b5ef-e9bd18d70a75" />

</details>
<details>
  <summary><strong>Change SMBIOS</strong></summary>
  
  * Use MacPro7,1
    * Apple marks this as the latest version supporting Mac Intel.
  * Security
    * use j160
  * [Full guide](https://www.tonymacx86.com/threads/howto-macos-26-tahoe-with-opencore-1-0-5-z390-i9-9900-rx-6600-xt.332345/)
</details>
<details>
  <summary><strong>No Sound</strong></summary>
  
  * Apple dropped Apple HDA. Inject the old one from [this link](https://github.com/chris1111/Kext-Droplet-macOS?tab=readme-ov-file)
    * [Simple loader](https://www.insanelymac.com/forum/topic/361429-simpleloader-kext-installer-utility/)
</details>
<details>
  <summary><strong>Wifi/BlueTooth</strong></summary>
  
  * [reddit](https://www.reddit.com/r/hackintosh/comments/1gvu5n1/broadcom_wifi_on_macos_sonoma_and_sequoia_fenvi/)
    * [reference](https://www.tonymacx86.com/threads/asus-z690-proart-creator-wifi-thunderbolt-4-i7-12700k-amd-rx-6800-xt.318311/page-458#post-2426553)
</details>
<details>
  <summary><strong>Power Management</strong></summary>
  
  * [link](https://basic.heavietnam.com/universal/fix-power-management)
    * [link](https://vnohackintosh.com/docs/post-install/fixing-power-management/)
</details>

## USB Port Map
See [USB_MAP.md](USB_MAP.md) for a map of all the ports on the Aorus z390 Master.

## Hardware Acceleration
Optional, for iGPU + dGPU hybrid setups (not required for the default build above, which uses a native dGPU only).

<details>
  <summary><strong>iMac19.1</strong></summary>
  
  * This iMac model appeared in 2019. There are 3 technical details that make it very similar to my PC:
    * Intel 9th generation Coffee Lake Refresh processor
    * iGPU Intel UHD Graphics 630
    * dGPU AMD Radeon Pro 570X / 575X / 580X.
  * On this real Mac the dGPU can be used to display the main graphics with good performance while the iGPU can contribute hardware video encoding and decoding tasks, releasing the CPU from these tasks. This is what you are looking for when selecting this SMBIOS: dGPU graphics / iGPU encoding. To achieve this you have to:
    * enable iGPU in BIOS
    * put the dGPU as main card
    * cable to monitor from the dGPU
    * recent versions of Lilu and WhateverGreen
    * SMBIOS from iMac19,1
    * iGPU in headless mode in config.plist, adding these lines in DeviceProperties / Add (OpenCore)
</details>
<details>
  <summary><strong>iMacPro1,1</strong></summary>

  * This iMac model appeared in 2017. It has a processor from a different family than my PC, it is Intel Xeon with 8, 10, 14 or 18 cores. But being a Mac without iGPU (it only has a Radeon Pro Vega 56 dGPU), it allows us to disable our iGPU in BIOS to obtain an equivalent system in which the dGPU serves both to bring graphics to the monitor and for video encoding and decoding tasks. This is what you are looking for when selecting this SMBIOS: dGPU graphics and encoding. To achieve this you have to:
    * disable iGPU in BIOS
    * cable to monitor from the dGPU
    * recent versions of Lilu and WhateverGreen
    * SMBIOS from iMacPro1,1.
</details>

## Resources
* [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
* [Wifi-Bluetooth](https://openintelwireless.github.io/General/Installation.html)
* [Dortania build-repo](https://github.com/dortania/build-repo/releases)

## Tools
* [Hackintool](https://github.com/headkaze/Hackintool)
* [OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools)
* [Wifi-Bluetooth kext](https://github.com/OpenIntelWireless)
* [OpenCore Configurator](https://mackie100projects.altervista.org/opencore-configurator/)
* [OC X Gen](https://github.com/Pavo-IM/OC-Gen-X)
* [HiDPI](https://github.com/xzhih/one-key-hidpi)
* [OpenCore-Legacy-Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases)
* [OpenCore-Legacy-Patcher-Custom-Tahoe](https://github.com/kgp-macPro/OCLP-CustoMac)
* [HDAUniversal](https://github.com/cmalf/HP-EliteDesk-800-G4-G5-Hackintosh/releases)
* [Codec-Info](https://olarila.com/topic/46666-codec-info-for-macos-analog-hda-codec-detector-for-applehda-applealc-voodoohda-and-hdauniversal/)

## History
<details>
  <summary><strong>Changes</strong></summary>
  * 2026-09-16: [experiment/broadcomvtd-ioMapperMapping] Built on top of `experiment/broadcomvtd`, additionally sets `DisableIoMapperMapping=true` to test the OpenCore quirk that replaced `CaseySJ/Ventura-AppleVTD-Patch`'s manual `IOPCIBridge` patch, documented to fix WiFi/Ethernet dying under AppleVTD specifically on Z390 Designare/Z490 Vision D. Attempts to resolve the Ethernet connect/disconnect loop while keeping AppleVTD and Wi-Fi. Pending real-hardware test results.

  * 2026-09-16: [experiment/broadcomvtd] Added `BroadcomVTD.kext` 0.2.17 (experimental) to test whether legacy Broadcom Wi-Fi/BT can coexist with AppleVTD enabled on Tahoe, per [Mieze/IntelMausiEthernet#50](https://github.com/Mieze/IntelMausiEthernet/issues/50) and [kgp-macPro/BroadcomVTD-Tahoe](https://github.com/kgp-macPro/BroadcomVTD-Tahoe). Config otherwise unchanged (`DisableIoMapper=false`, patched `SSDT-DMAR.aml`). Pending real-hardware test results.

  * 2026-08-16: update macOS 26.6.2

  * 2025-09-20: change SMBIOS to MacPro7,1. Preparing for macOS Tahoe 26.

  * 2025-05-24: remove Intel Wifi Card, Installed BCM943602CS Follow this [video](https://youtu.be/d7F5d7EF334?t=713) for adjusting.

        remap USBMap.Kext for disable HS14(Intel Wifi Card).
        Keep 15 port below:
        HS01 HS03 HS04 HS05 HS09 HS10 HS11 HS12 HS13
        SS01 SS03 SS04 SS05 SS09 SS10
  
  * 2024-10-20: Updated to macOS 15.0.1, fix bluetooth broken

        <key>bluetoothInternalControllerInfo</key>
        <data>AAAAAAAAAAAAAAAAAAA=</data>
        <key>bluetoothExternalDongleFailed</key>
        <data>AA==</data>
      
  * remove SSDT-PLUG due to macOS version >= 12.3 [link](https://dortania.github.io/OpenCore-Post-Install/universal/pm.html)
  
</details>

## Thanks
* [cmer](https://github.com/cmer) : this is the first guide that I followed and try with Catalina 10.15.1
* [AudioGod](https://www.insanelymac.com/forum/topic/340936-audiogods-aorus-z390-master-patched-dsdt-efi-for-catalina-mini-guide-and-discussion/) : Currently, I use from him and change a little bit to make something well.
* [Colin Sullender](https://github.com/shiruken) : Previous, I try many times but cannot boot into Macintosh. Thanks for using Intel CNVI in your system. I just rebuild USB Map kext and everything works.
* [EliteMacx86 Administrator](https://elitemacx86.com/threads/how-to-fix-broadcom-wifi-on-macos-sonoma-and-later.1415/): The guidance is very detailed for rolling back the kext to macOS 13 for the previous native Broadcom Wi-Fi card.
* [lzhoang2801](https://github.com/kgp-macPro/OCLP-CustoMac): A custom from from original OpenCore-Legacy-Patcher, this is bring back AppleHDA for sound and BCM94360CS2 for bluetooth + wifi.
