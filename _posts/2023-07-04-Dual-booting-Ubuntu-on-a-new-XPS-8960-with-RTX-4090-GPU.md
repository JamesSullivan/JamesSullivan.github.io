---
layout: post
title:  "Dual booting Ubuntu on a new XPS 8960 with RTX-4090 GPU"
excerpt: "Not for the faint of heart"
date: 2023-07-04 09:00:34 +0900
categories: ML

image: assets/images/posts/NVIDIA_Settings.png
image_alt: NVIDIA Settings
image_caption: NVIDIA Settings
---

## Dual booting Ubuntu on a new Dell XPS 8960 with an NVIDIA GeForce RTX-4090 GPU
<br>

After much trial and error, I finally got Linux (Ubuntu 22.04.02) to dual boot on my new Dell XPS 8960. You may only need to do some of what I did, but I have listed everything I had to do to get to a working state below, and I am now reluctant to experiment further.

Even more so than usual, please take heed of the standard warning that there is no guarantee this will work for your particular setup and could brick your computer.


### Preparations before installation:

1. Set up Windows 11. If this is a new computer, go through the Windows 11 installation/setup wizard. 

2. Make a backup Windows install USB. You will likely need this, so don't skip this step. I certainly needed the backup. Use a USB drive with 64GB of storage. Windows 11 displays a message that the USB must have a minimum of 32GB of free space. However, the OS calculates a gig strictly using the larger 2 to the power of 10 definition. At the same time, USB drive advertisements use a smaller 1,000,000,000 bits definition, so I recommend using a larger USB drive. Moreover, Windows 11 seems temperamental, and I had to try several times before I could finally create the backup, even with a 64GB USB drive. See the recovery USB section of [Windows 11 and Ubuntu dual-boot...](https://youtu.be/CjxYxdJb_OA?t=127)

3. If you don't have a partition ready for the Linux installation, prepare one. [Windows 11 and Ubuntu dual-boot...](https://youtu.be/CjxYxdJb_OA?t=255)

4. Disable fast startup. [Windows 11 and Ubuntu dual-boot...](https://youtu.be/CjxYxdJb_OA?t=322)

5. Prepare a bootable USB with Ubuntu 22.04. See [create a bootable USB stick on Ubuntu](https://ubuntu.com/tutorials/create-a-usb-stick-on-ubuntu#1-overview) or [Windows 11 and Ubuntu dual-boot..](https://youtu.be/CjxYxdJb_OA?t=353).

 
### Turn off disk encryption and make BIOS changes

Enter into bios by pressing F2 repeatedly after turning on the power.

I had to save and reboot the computer each time for the following three steps.

1. Turn off BitLocker . [More details and background](https://discourse.ubuntu.com/t/ubuntu-installation-on-computers-running-windows-and-bitlocker-turned-on/15338). The following link may also be helpful - [Finding your BitLocker recovery key in Windows](https://support.microsoft.com/en-us/windows/finding-your-bitlocker-recovery-key-in-windows-6b71ad27-0b89-ea08-f143-056f5ab347d6)

2. Under the Secure Boot menu, turn off Secure Boot Mode.

3. Under Storage, changed SATA/NVMe Operation from RAID to AHCI/NVMe. If you don't want to reinstall Windows after changing RAID to AHCI, you should set Windows 11 to Safe Mode beforehand. In Windows, run msconfig.exe, and from the Boot tab, enable Safe Boot (Minimal). This change should require a restart. Then in the BIOS, change to AHCI, and Windows should still boot up. Then you can disable Safe Boot, and in theory, Windows 11 should work the same when you reboot. Check Windows 11 is working properly. Warning: although Windows mostly worked after this, I still had to reinstall Windows 11 using my previously created Windows recovery USB drive to get everything working properly.


### Installation

Insert your Ubuntu USB drive, then turn on your computer and repeatedly click F12 until you get a boot menu. The USB drive should be one of the choices, and select it. 

At this point, it should be a normal Ubuntu LTS install, so follow the Wizard. However, after you have finished installing Ubuntu to disk and rebooting, you will likely run into the following error:

`nouveau 0000:01:00.0  unknown chipset (192000a1)`

Don't panic; instead, reboot, except this time, press `e` at boot up to enter the bootloader and edit the kernel options. Do this by adding `nomodeset rdblacklist=nouveau` to the end of the line that begins with Linux. Then continue to boot the system. <a href="https://askubuntu.com/questions/38780/how-do-i-set-nomodeset-after-ive-already-installed-ubuntu">Further nomodeset info</a>.

Now that you are in Ubuntu, go to Software & Updates -> Additional Drivers and select Using NVIDIA driver meta package from Nvidia-driver-535 (proprietary, tested). After applying and saving the changes, you should be good for your next restart.

The installer defaults to x11 rather than Wayland display server, but at this point, I am just happy to have a working installation.


### Helpful Links:

[Windows 11 and Ubuntu dual-boot on a Dell Inspiron desktop - with Secure Boot & application install](https://www.youtube.com/watch?v=CjxYxdJb_OA)

[How to Install Ubuntu and Windows 8 Through Windows 11 as a Dual Boot on your Dell Computer](https://www.dell.com/support/kbdoc/en-ca/000131253/how-to-install-ubuntu-and-windows-8-or-10-as-a-dual-boot-on-your-dell-pc)

[Dual Boot Linux XPS 9520 Guide](https://www.reddit.com/r/DellXPS/comments/1313xgw/dual_boot_linux_xps_9520_guide/)

[nouveau "unknown chipset" On Installer](https://askubuntu.com/questions/598417/nouveau-unknown-chipset-on-installer/608532#608532)

<br><br>

<img class="cc-icon css-11y11pk" width="32" height="32" src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAxMy4wLjIsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDE0OTQ4KSAgLS0+DQo8IURPQ1RZUEUgc3ZnIFBVQkxJQyAiLS8vVzNDLy9EVEQgU1ZHIDEuMC8vRU4iICJodHRwOi8vd3d3LnczLm9yZy9UUi8yMDAxL1JFQy1TVkctMjAwMTA5MDQvRFREL3N2ZzEwLmR0ZCI+DQo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4Ig0KCSB3aWR0aD0iNjRweCIgaGVpZ2h0PSI2NHB4IiB2aWV3Qm94PSI1LjUgLTMuNSA2NCA2NCIgZW5hYmxlLWJhY2tncm91bmQ9Im5ldyA1LjUgLTMuNSA2NCA2NCIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+DQo8Zz4NCgk8Y2lyY2xlIGZpbGw9IiNGRkZGRkYiIGN4PSIzNy43ODUiIGN5PSIyOC41MDEiIHI9IjI4LjgzNiIvPg0KCTxwYXRoIGQ9Ik0zNy40NDEtMy41YzguOTUxLDAsMTYuNTcyLDMuMTI1LDIyLjg1Nyw5LjM3MmMzLjAwOCwzLjAwOSw1LjI5NSw2LjQ0OCw2Ljg1NywxMC4zMTQNCgkJYzEuNTYxLDMuODY3LDIuMzQ0LDcuOTcxLDIuMzQ0LDEyLjMxNGMwLDQuMzgxLTAuNzczLDguNDg2LTIuMzE0LDEyLjMxM2MtMS41NDMsMy44MjgtMy44Miw3LjIxLTYuODI4LDEwLjE0Mw0KCQljLTMuMTIzLDMuMDg1LTYuNjY2LDUuNDQ4LTEwLjYyOSw3LjA4NmMtMy45NjEsMS42MzgtOC4wNTcsMi40NTctMTIuMjg1LDIuNDU3cy04LjI3Ni0wLjgwOC0xMi4xNDMtMi40MjkNCgkJYy0zLjg2Ni0xLjYxOC03LjMzMy0zLjk2MS0xMC40LTcuMDI3Yy0zLjA2Ny0zLjA2Ni01LjQtNi41MjQtNy0xMC4zNzJTNS41LDMyLjc2Nyw1LjUsMjguNWMwLTQuMjI5LDAuODA5LTguMjk1LDIuNDI4LTEyLjINCgkJYzEuNjE5LTMuOTA1LDMuOTcyLTcuNCw3LjA1Ny0xMC40ODZDMjEuMDgtMC4zOTQsMjguNTY1LTMuNSwzNy40NDEtMy41eiBNMzcuNTU3LDIuMjcyYy03LjMxNCwwLTEzLjQ2NywyLjU1My0xOC40NTgsNy42NTcNCgkJYy0yLjUxNSwyLjU1My00LjQ0OCw1LjQxOS01LjgsOC42Yy0xLjM1NCwzLjE4MS0yLjAyOSw2LjUwNS0yLjAyOSw5Ljk3MmMwLDMuNDI5LDAuNjc1LDYuNzM0LDIuMDI5LDkuOTEzDQoJCWMxLjM1MywzLjE4MywzLjI4NSw2LjAyMSw1LjgsOC41MTZjMi41MTQsMi40OTYsNS4zNTEsNC4zOTksOC41MTUsNS43MTVjMy4xNjEsMS4zMTQsNi40NzYsMS45NzEsOS45NDMsMS45NzENCgkJYzMuNDI4LDAsNi43NS0wLjY2NSw5Ljk3My0xLjk5OWMzLjIxOS0xLjMzNSw2LjEyMS0zLjI1Nyw4LjcxMy01Ljc3MWM0Ljk5LTQuODc2LDcuNDg0LTEwLjk5LDcuNDg0LTE4LjM0NA0KCQljMC0zLjU0My0wLjY0OC02Ljg5NS0xLjk0My0xMC4wNTdjLTEuMjkzLTMuMTYyLTMuMTgtNS45OC01LjY1NC04LjQ1OEM1MC45ODQsNC44NDQsNDQuNzk1LDIuMjcyLDM3LjU1NywyLjI3MnogTTM3LjE1NiwyMy4xODcNCgkJbC00LjI4NywyLjIyOWMtMC40NTgtMC45NTEtMS4wMTktMS42MTktMS42ODUtMmMtMC42NjctMC4zOC0xLjI4Ni0wLjU3MS0xLjg1OC0wLjU3MWMtMi44NTYsMC00LjI4NiwxLjg4NS00LjI4Niw1LjY1Nw0KCQljMCwxLjcxNCwwLjM2MiwzLjA4NCwxLjA4NSw0LjExM2MwLjcyNCwxLjAyOSwxLjc5MSwxLjU0NCwzLjIwMSwxLjU0NGMxLjg2NywwLDMuMTgxLTAuOTE1LDMuOTQ0LTIuNzQzbDMuOTQyLDINCgkJYy0wLjgzOCwxLjU2My0yLDIuNzkxLTMuNDg2LDMuNjg2Yy0xLjQ4NCwwLjg5Ni0zLjEyMywxLjM0My00LjkxNCwxLjM0M2MtMi44NTcsMC01LjE2My0wLjg3NS02LjkxNS0yLjYyOQ0KCQljLTEuNzUyLTEuNzUyLTIuNjI4LTQuMTktMi42MjgtNy4zMTNjMC0zLjA0OCwwLjg4Ni01LjQ2NiwyLjY1Ny03LjI1N2MxLjc3MS0xLjc5LDQuMDA5LTIuNjg2LDYuNzE1LTIuNjg2DQoJCUMzMi42MDQsMTguNTU4LDM1LjQ0MSwyMC4xMDEsMzcuMTU2LDIzLjE4N3ogTTU1LjYxMywyMy4xODdsLTQuMjI5LDIuMjI5Yy0wLjQ1Ny0wLjk1MS0xLjAyLTEuNjE5LTEuNjg2LTINCgkJYy0wLjY2OC0wLjM4LTEuMzA3LTAuNTcxLTEuOTE0LTAuNTcxYy0yLjg1NywwLTQuMjg3LDEuODg1LTQuMjg3LDUuNjU3YzAsMS43MTQsMC4zNjMsMy4wODQsMS4wODYsNC4xMTMNCgkJYzAuNzIzLDEuMDI5LDEuNzg5LDEuNTQ0LDMuMjAxLDEuNTQ0YzEuODY1LDAsMy4xOC0wLjkxNSwzLjk0MS0yLjc0M2w0LDJjLTAuODc1LDEuNTYzLTIuMDU3LDIuNzkxLTMuNTQxLDMuNjg2DQoJCWMtMS40ODYsMC44OTYtMy4xMDUsMS4zNDMtNC44NTcsMS4zNDNjLTIuODk2LDAtNS4yMDktMC44NzUtNi45NDEtMi42MjljLTEuNzM2LTEuNzUyLTIuNjAyLTQuMTktMi42MDItNy4zMTMNCgkJYzAtMy4wNDgsMC44ODUtNS40NjYsMi42NTgtNy4yNTdjMS43Ny0xLjc5LDQuMDA4LTIuNjg2LDYuNzEzLTIuNjg2QzUxLjExNywxOC41NTgsNTMuOTM4LDIwLjEwMSw1NS42MTMsMjMuMTg3eiIvPg0KPC9nPg0KPC9zdmc+DQo="><img class="cc-icon css-11y11pk" width="32" height="32" src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz4NCjwhLS0gR2VuZXJhdG9yOiBBZG9iZSBJbGx1c3RyYXRvciAxMy4wLjIsIFNWRyBFeHBvcnQgUGx1Zy1JbiAuIFNWRyBWZXJzaW9uOiA2LjAwIEJ1aWxkIDE0OTQ4KSAgLS0+DQo8IURPQ1RZUEUgc3ZnIFBVQkxJQyAiLS8vVzNDLy9EVEQgU1ZHIDEuMC8vRU4iICJodHRwOi8vd3d3LnczLm9yZy9UUi8yMDAxL1JFQy1TVkctMjAwMTA5MDQvRFREL3N2ZzEwLmR0ZCI+DQo8c3ZnIHZlcnNpb249IjEuMCIgaWQ9IkxheWVyXzEiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHg9IjBweCIgeT0iMHB4Ig0KCSB3aWR0aD0iNjRweCIgaGVpZ2h0PSI2NHB4IiB2aWV3Qm94PSI1LjUgLTMuNSA2NCA2NCIgZW5hYmxlLWJhY2tncm91bmQ9Im5ldyA1LjUgLTMuNSA2NCA2NCIgeG1sOnNwYWNlPSJwcmVzZXJ2ZSI+DQo8Zz4NCgk8Y2lyY2xlIGZpbGw9IiNGRkZGRkYiIGN4PSIzNy42MzciIGN5PSIyOC44MDYiIHI9IjI4LjI3NiIvPg0KCTxnPg0KCQk8cGF0aCBkPSJNMzcuNDQzLTMuNWM4Ljk4OCwwLDE2LjU3LDMuMDg1LDIyLjc0Miw5LjI1N0M2Ni4zOTMsMTEuOTY3LDY5LjUsMTkuNTQ4LDY5LjUsMjguNWMwLDguOTkxLTMuMDQ5LDE2LjQ3Ni05LjE0NSwyMi40NTYNCgkJCUM1My44NzksNTcuMzE5LDQ2LjI0Miw2MC41LDM3LjQ0Myw2MC41Yy04LjY0OSwwLTE2LjE1My0zLjE0NC0yMi41MTQtOS40M0M4LjY0NCw0NC43ODQsNS41LDM3LjI2Miw1LjUsMjguNQ0KCQkJYzAtOC43NjEsMy4xNDQtMTYuMzQyLDkuNDI5LTIyLjc0MkMyMS4xMDEtMC40MTUsMjguNjA0LTMuNSwzNy40NDMtMy41eiBNMzcuNTU3LDIuMjcyYy03LjI3NiwwLTEzLjQyOCwyLjU1My0xOC40NTcsNy42NTcNCgkJCWMtNS4yMiw1LjMzNC03LjgyOSwxMS41MjUtNy44MjksMTguNTcyYzAsNy4wODYsMi41OSwxMy4yMiw3Ljc3LDE4LjM5OGM1LjE4MSw1LjE4MiwxMS4zNTIsNy43NzEsMTguNTE0LDcuNzcxDQoJCQljNy4xMjMsMCwxMy4zMzQtMi42MDcsMTguNjI5LTcuODI4YzUuMDI5LTQuODM4LDcuNTQzLTEwLjk1Miw3LjU0My0xOC4zNDNjMC03LjI3Ni0yLjU1My0xMy40NjUtNy42NTYtMTguNTcxDQoJCQlDNTAuOTY3LDQuODI0LDQ0Ljc5NSwyLjI3MiwzNy41NTcsMi4yNzJ6IE00Ni4xMjksMjAuNTU3djEzLjA4NWgtMy42NTZ2MTUuNTQyaC05Ljk0NFYzMy42NDNoLTMuNjU2VjIwLjU1Nw0KCQkJYzAtMC41NzIsMC4yLTEuMDU3LDAuNTk5LTEuNDU3YzAuNDAxLTAuMzk5LDAuODg3LTAuNiwxLjQ1Ny0wLjZoMTMuMTQ0YzAuNTMzLDAsMS4wMSwwLjIsMS40MjgsMC42DQoJCQlDNDUuOTE4LDE5LjUsNDYuMTI5LDE5Ljk4Niw0Ni4xMjksMjAuNTU3eiBNMzMuMDQyLDEyLjMyOWMwLTMuMDA4LDEuNDg1LTQuNTE0LDQuNDU4LTQuNTE0czQuNDU3LDEuNTA0LDQuNDU3LDQuNTE0DQoJCQljMCwyLjk3MS0xLjQ4Niw0LjQ1Ny00LjQ1Nyw0LjQ1N1MzMy4wNDIsMTUuMywzMy4wNDIsMTIuMzI5eiIvPg0KCTwvZz4NCjwvZz4NCjwvc3ZnPg0K">  This article is licensed under a <a href="https://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International</a> license.<br>
