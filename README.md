# Sonoma Gigabyte Z390 Aorus Master (OpenCore)

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.2-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOS-Stable](https://img.shields.io/badge/macOS-15.0.1-brightgreen.svg)](https://www.apple.com/macos/macos-sequoia)

![Sequoia1501](https://github.com/user-attachments/assets/8147bd6f-f95e-447c-99f5-386c31d5f145)


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
  | Wifi & BT         | Intel® CNVi 802.11ac 2x2 Wave 2 WIFI & BT5  (on-board)   | I just use bluetooth for JBL FLIP 5 Speaker.          |
  |                   | Include **Intel Wireless-AC 9560** module inside         | If you want native wifi control.                      |
  |                   |                                                          | Use AirportItlwm instead but slow [speed](image)      |
  |                   |                                                          | Use Itlwm and HeliPort for increase wifi speed        |
  
</details>

<details>

<summary><strong>Kernel extensions</strong></summary>
<br>

| Kext                   | Version        |
|:---------------------- | -------------- |
| Lilu                   | 1.6.9          |
| VirtualSMC             | 1.3.4          |
| WhateverGreen          | 1.6.8          |
| AppleALC               | 1.9.2          |
| IntelBluetoothFirmware | 2.4.0          |
| IntelBTPatcher         | 2.4.0          |
| IntelMausi             | 1.0.7          |
| SMCProcessor           | 1.3.4          |
| SMCSuperIO             | 1.3.4          |
| USBMap                 | Manual         |
| itlwm                  | 2.2.0          |
| BlueToolFixup          | 2.6.9          |


</details>

<details>
  <summary><strong>Working ✅ / Not Working ☑️</strong></summary>
  
  * ✅ Ethernet
  * ✅ Onboard Audio
  * ✅ iMessage
  * ✅ Sleep/Wake
  * ✅ Bluetooth & Wi-Fi
  * ☑️ Airdrop
  * ☑️ Handoff
  
</details>

## History
<details>
  <summary><strong>Changes</strong></summary>
  * 2024-10-20: Updated to macOS 15.0.1, fix bluetooth broken
  
      <key>bluetoothInternalControllerInfo</key>
      <data>AAAAAAAAAAAAAAAAAAA=</data>
      <key>bluetoothExternalDongleFailed</key>
      <data>AA==</data>
      
  * remove SSDT-PLUG due to macOS version >= 12.3 [link](https://dortania.github.io/OpenCore-Post-Install/universal/pm.html)
  
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


## Thanks
* [cmer](https://github.com/cmer) : this is the first guide that I followed and try with Catalina 10.15.1
* [AudioGod](https://www.insanelymac.com/forum/topic/340936-audiogods-aorus-z390-master-patched-dsdt-efi-for-catalina-mini-guide-and-discussion/) : Currently, I use from him and change a little bit to make something well.
* [Colin Sullender](https://github.com/shiruken) : Previous, I try many times but cannot boot into Macintosh. Thanks for using Intel CNVI in your system. I just rebuild USB Map kext and everything works.
