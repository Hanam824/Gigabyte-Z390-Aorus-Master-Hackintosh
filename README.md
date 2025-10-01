# Sequoia Gigabyte Z390 Aorus Master (OpenCore)

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.5-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOS-Stable](https://img.shields.io/badge/macOS-15.7-brightgreen.svg)](https://www.apple.com/macos/macos-sequoia)

<img width="392" height="655" alt="Sequoia157" src="https://github.com/user-attachments/assets/44e669bc-3df2-43bf-8f99-e0db8ba82def" />

## My PC Build
<details>
  <summary><strong>Hardware</strong></summary>
  
  | Category          | Component                                                | Note                                                  |
  | ----------------- | -------------------------------------------------------  | ----------------------------------------------------- |
  | CPU               | Intel Core i9-9900K                                      |                                                       |
  | GPU               | MSI Radeon RX 5700 XT EVOKE OC Graphics Board            | Native support                                        |
  | Motherboard       | Gigabyte Z390 AORUS MASTER                               |                                                       |
  | Storage (macOS)   | Silicon Power SSD 512GB NVMe 1.3 P34A80 (`M2M` slot)     | Internal NVME                                         |
  | Storage (Windows) | Crucial P1 500GB 3D NAND NVMe PCIe (`M2A` slot)          | Internal NVME                                         |
  | Memory            | Corsair Vengeance LPX 32GB (2x16GB) 3200MHz DDR4         |                                                       |
  | CPU Cooler        | EKWB EK-KIT Performance Series PC Watercooling Kit P360  |                                                       |
  | Power Supply      | Corsair RMX Series 80PLUS Gold 1000W                     |                                                       |
  | Case              | Cooler Master MasterCase H500M ARGB                      |                                                       |
  | Monitor           | Dell Display Monitor SE2416H 23.8inches                  |                                                       |
  | LAN               | Intel® i219v GbE LAN                                     | I use LAN for network                                 |
  | Wifi & BT         |~~Intel® CNVi 802.11ac 2x2 Wave 2 WIFI & BT5  (on-board)~~| ~~I just use bluetooth for JBL FLIP 5 Speaker.~~      |
  |                   |~~Include **Intel Wireless-AC 9560** module inside~~      | ~~If you want native wifi control.~~                  |
  |                   | Replace Intel Wifi Card with BCM943602CS on PCIe port    | ~~Use AirportItlwm instead but slow [speed](image)~~  |
  |                   | and connect on F-USB2 (HS13) for bluetooth               | ~~Use Itlwm and HeliPort for increase wifi speed~~    |

  [For more information](https://pcpartpicker.com/list/twpvrM)
  
</details>
<details>

<summary><strong>Kernel extensions</strong></summary>
<br>

| Kext                   | Version        |
|:---------------------- | -------------- |
| Lilu                   | 1.7.1          |
| VirtualSMC             | 1.3.7          |
| SMCProcessor           | 1.3.7          |
| SMCSuperIO             | 1.3.7          |
| RestrictEvents         | 1.1.6          |
| WhateverGreen          | 1.7.0          |
| AppleALC               | 1.9.5          |
| IntelBluetoothFirmware | 2.4.0          |
| IntelBTPatcher         | 2.4.0          |
| IntelMausi             | 1.0.8          |
| USBMap                 | Manual         |
| AMFIPass               | 1.4.1          |
| IOSkywalkFamily        | 1.0            |
| IO80211FamilyLegacy    | 1200.12.2b1    |
| BlueToolFixup          | 2.7.0          |


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

## Macos Tahoe 26
<details>
  <summary><strong>Change SMBIOS</strong></summary>
  
  * Use MacPro7,1
    * Apple mark this version is the latest verison support Mac Intel.
  * Security
    * use j160
  * [full](https://www.tonymacx86.com/threads/howto-macos-26-tahoe-with-opencore-1-0-5-z390-i9-9900-rx-6600-xt.332345/)
</details>
<details>
  <summary><strong>No Sound</strong></summary>
  
  * Apple drop Apple HDA. inject old from [link](https://github.com/chris1111/Kext-Droplet-macOS?tab=readme-ov-file)
    * [simple loader](https://www.insanelymac.com/forum/topic/361429-simpleloader-kext-installer-utility/)
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

## History
<details>
  <summary><strong>Changes</strong></summary>

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

## Hardware acceleration
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

## USB Port Map
See [USB_MAP.md](USB_MAP.md) for a map of all the ports on the Aorus z390 Master.

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


## Thanks
* [cmer](https://github.com/cmer) : this is the first guide that I followed and try with Catalina 10.15.1
* [AudioGod](https://www.insanelymac.com/forum/topic/340936-audiogods-aorus-z390-master-patched-dsdt-efi-for-catalina-mini-guide-and-discussion/) : Currently, I use from him and change a little bit to make something well.
* [Colin Sullender](https://github.com/shiruken) : Previous, I try many times but cannot boot into Macintosh. Thanks for using Intel CNVI in your system. I just rebuild USB Map kext and everything works.
* [EliteMacx86 Administrator](https://elitemacx86.com/threads/how-to-fix-broadcom-wifi-on-macos-sonoma-and-later.1415/): The guidance is very detailed for rolling back the kext to macOS 13 for the previous native Broadcom Wi-Fi card.
