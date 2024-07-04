# ThinkPad_T480_OpenCore_Thunderbolt

**Supported macOS: 14 Sonoma, should boot 15 Sequoia with minimum kext updates**

Highest tested stable OS: 14.5

**DISCLAIMER:**
Read the entire README and Dortania guides before you start. I am not responsible for any damage.
When you encounter bug or want to improve this repo, consider opening issue or pull request. 
If you find this bootloader configuration useful, consider giving it a star to make it more visible.

Also yes, the README file is not just copy pasted from another repo. It contains information specific to this EFI config.

## Introduction

<details> 

<summary><strong>General knowledge & credits</strong></summary>

- To install macOS follow the guides provided by [Dortania](https://dortania.github.io/getting-started/)

- Useful tools by [CorpNewt](https://github.com/corpnewt) and [headkaze](https://github.com/headkaze/Hackintool)

</details>  

<details>

<summary><strong>Hardware</strong></summary>
<br>


[![UEFI](https://img.shields.io/badge/UEFI-N24UJ38W-lightgrey)](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-t-series-laptops/thinkpad-t480-type-20l5-20l6/downloads/ds502355)
| Category  | Component                            | Note                                                                                                               |
| --------- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| CPU       | Intel Core 8th Gen                   | BIOS 1.51 TB NVM 43                                                                                                |
| GPU       | Intel UHD 620                        |                                                                                                                    |
| SSD       | Dual NVMe                            | Works perfectly with latest NVMeFix.kext                                                                           |
| Memory    | Dual Channel DDR4                    |                                                                                                                    |
| Battery   | Dual battery                         |                                                                                                                    |
| Camera    | Integrated Camera                    |                                                                                                                    |
| Wifi & BT | DW1820A                              | Use AirportItlwm for Intel cards.                                                                                  |                     
| Input     | PS2 Keyboard & Synaptics TrackPad    | Recommended YogaSMC and ThinkpadAssistant (in User Space)                                                          |

</details>  

<details>

<summary><strong>Main software</strong></summary>
<br>

| Component      | Version        |
| -------------- | -------------- |
| OpenCore       | v1.0.0 MOD     |

</details>



## Before installation

<details>  

<summary><strong>UEFI settings</strong></summary>
<br>

**Security**

- `Security Chip` **Disabled**
- `Memory Protection -> Execution Prevention` **Enabled**
- `Virtualization -> Intel Virtualization Technology` **Enabled**
- `Virtualization -> Intel VT-d Feature` **Enabled**
- `Anti-Theft -> Computrace -> Current Setting` **Disabled**
- `Secure Boot -> Secure Boot` **Disabled**
- `Intel SGX -> Intel SGX Control` **Disabled**
- `I/O Port Access -> Thunderbolt` **Disabled**
- `Device Guard` **Disabled**

**Startup**

- `UEFI/Legacy Boot` **UEFI Only**
- `CSM Support` **No**

**Important: Thunderbolt**

- `Thunderbolt BIOS Assist Mode` **Disabled**
- `Wake by Thunderbolt(TM) 3` **Disabled**
- `Security Level` **No Security**
- `Support in Pre Boot Environment -> Thunderbolt(TM) device` **Enabled**

</details>  

<details>

<summary><strong>Own prev-lang-kbd</strong></summary>
<br>

Either add as a string or as a data ( HEX data [(ProperTree)](https://github.com/corpnewt/ProperTree) )

Format is lang-COUNTRY:keyboard

- 🇺🇸 | [0] en_US - U.S --> en-US:0 --> 656e2d55 533a30

etc.

[AppleKeyboardLayouts.txt](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt)

</details>

<details>

<summary><strong>Secure Boot (Optional)</strong></summary>
<br>

1. Set Secure Boot to Setup Mode. Secure Boot should be reported as off by UEFI main tab
2. Create FAT32 formatted USB
3. Create EFI folder in the root of the newly formatted flash drive and move there content of SecureBoot/KeyTool
4. Boot flash drive via F12 boot menu
5. Choose **Edit keys**


<img src="./Other/README_Resources/SecureBoot/MainMenu.png" alt="Main menu">

6. Start by **replacing** Signature Database. Select .auth file


<img src="./Other/README_Resources/SecureBoot/ManipulateKey.png" alt="Select key to manipulate with">
<img src="./Other/README_Resources/SecureBoot/SelectAuth.png" alt="Select .auth file">


7. Do the same for Key Exchange Keys Database (KEK) and Platform Key (PK) **in this order**
8. Exit and shutdown your machine
9. Boot into the UEFI settings and check if Secure Boot is reported as `on`
10. Boot you favorite OS with Secure Boot enabled

[More detailed information here](https://habr.com/en/post/273497)

```diff
! Still quite experimental
```

</details>



## Post-Install

<details>  

<summary><strong>Colour banding</strong></summary>
<br>

If you encounter some serious colour banding issues ( Keep in mind that T480 1080p stock panel colour accuracy is not really good, cca 50-60% sRGB), your only solution is to replace GPU properties as bellow or replace the stock panel with one from T490 (400 nits, Low power).

```
<key>AAPL,ig-platform-id</key>
<data>AAAWGQ==</data>
<key>device-id</key>
<data>FhkAAA==</data>
</dict>
```

Do not use these any additional boot arguments! Get custom WhateverGreen version instead from Other folder

You can check your screen in gradient test [here](https://www.eizo.be/monitor-test/) or just by simple look at Launchpad background.

</details>  

<details>  

<summary><strong>Generate your own SMBIOS</strong></summary>
<br>

[GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)

For the sake of future-proof updates, please use `MacBookAir9,1` (MBA 13 Intel 2020). Otherwise edit USB Map files.

</details>  

<details>  

<summary><strong>CPUFriend power management</strong></summary>
<br>

Generate CPUFriendDataProvider for your machine [here](https://github.com/fewtarius/CPUFriendFriend) or use at your own risk files provided in the Other folder.

</details>  



## Status

<details>  

<summary><strong>What's working ✅</strong></summary>

- [x] Battery percentage

- [X] Broadcom Bluetooth - All works! (native) 

- [x] Boot chime

- [x] Boot menu `OpenCanopy` 

- [x] CPU power management / performance `Now on par with Windows without XTU undervolt.`

- [x] FireVault 2 `No config.plist changes needed` 

- [x] GPU UHD 620 hardware acceleration / performance 

- [x] HDMI `Closed lid. With audio.`

- [x] USB-C Outputs `Both USB-C ports works independently of Thunderbolt status (Non-TB3 EFI only supports DP through the upper port).`

- [x] iMessage, FaceTime, App Store, iTunes Store. **Generate your own SMBIOS**

- [x] Intel I219V Ethernet port `Full 1 Gbps speeds and external eithernet adapter next to TB port also works`

- [x] Keyboard `Volume and brightness hotkeys. Another media keys with YogaSMC.`

- [x] Microphone `With keyboard switch using YogaSMC.`

- [x] Realtek® ALC3287 ("ALC257") Audio

- [x] SD card reader `Fortunately, USB connected.`

- [x] Sidecar wired `Works with 15,2 SMBIOS. Wireless with Broadcom Card`

- [x] Sleep/Wake `Hibernatemode 25 for the best results. `

- [ ] Thunderbolt  `40 Gbps, cold boot only`

- [x] TouchPad `1-5 fingers swipe works. Emulate force touch using longer and more voluminous touch.`

- [x] TrackPoint  `Works perfectly. Edited kext Info.plist for more precise acceleration.`

- [x] USB Ports `USB Map is different for devices with Windows Hello camera.`

- [x] Web camera `Works well with MacOS with no bugs`

- [X] Broadcom Wifi - `For native Broadcom cards disable kext patches. 14.0+ use OCLP.`

- [x] DRM `Widevine, validated on Firefox 82. WhateverGreen's DRM is broken on Big Sur`

</details>  

<details>  

<summary><strong>What's not working ⚠️</strong></summary>
    
- [ ] Thunderbolt Sleep/Wake  `Thunderbolt needs a cold boot to be enabled. If you enter a sleep/wake cycle you will need to shutdown and cold boot again.`

- [ ] 4K Output  `USB-C currently can only support output up to 2K. Culprit is unknown.`

</details>  



## UEFI modding

<details>  

<summary><strong>CFG Lock | Advanced menu</strong></summary>
<br>

It's possible to unlock Advanced menu thus disable CFG Lock natively in UEFI + Other Advanced menu benefits. SPI Programmer CH341a is required

<br>
https://www.reddit.com/r/thinkpad/comments/ffqqx5/currently_testing_skyra1n/

[T480 consuming 60w (~85w total) - unlimited TDP : thinkpad](https://www.reddit.com/r/thinkpad/comments/g8fk51/t480_consuming_60w_85w_total_unlimited_tdp/)

[ThinkPad discord](discord.gg/Ybdz7AS)

</details>  


## Repo-specific notes

<details>  

<summary><strong>There are two EFIs in the repo. Which one should I choose? </strong></summary>
<br>

For Thunderbolt support please download the EFI folder. This one is based on [pierpaolodimarzo's EFI config](https://github.com/pierpaolodimarzo/ThinkPad-T480).

For non-Thunderbolt support please download the zip file. This one is based on [valnoxy's EFI config](https://github.com/valnoxy/t480-oc). One notably benefit of using this EFI instead of valnoxy's one is that their config contains a rather long and redundant kext injection list.

Note that the non-TB config is no longer maintained. You will lose TB port DP output as well as 40Gb TB3 support, but you will gain better battery life. Both should boot macOS Sonoma equally well.

</details>  

