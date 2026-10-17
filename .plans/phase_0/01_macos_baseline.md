# 01: macOS baseline

**Status: mostly done.** The hardware facts came from `.context/about_this_mac.json`. Four items that file does not cover are still open (see [Still open](#still-open)).

## Goal

Capture hardware facts while macOS still boots, so the Linux results have something to be compared against. This also settles the open question in the vision doc: does this machine have a discrete AMD Radeon Pro? **Answer: yes, a Radeon Pro 455.**

## Prerequisites

- The MacBook boots into macOS.
- A place to save text output (a notes file on the second computer or a USB stick).

## Steps

- [x] Open **About This Mac** and record the model name, year, processor, memory and graphics line(s). Two graphics entries means a discrete GPU is present. *(Two GPUs reported.)*
- [x] In **System Information**, record the **Model Identifier**. *(`MacBookPro13,3`, confirmed. The JSON shows it as `MacBookPro13 3`, with the comma lost in the export.)*
- [x] Capture graphics details (`system_profiler SPDisplaysDataType`).
- [x] Capture the Wi-Fi card details (`system_profiler SPAirPortDataType`).
- [x] Capture the PCI device list (`system_profiler SPPCIDataType`). *(Nothing reported: the file says `"PCI": "None"`. Linux `lspci` in [03](03_boot_and_inventory.md) is the real source for PCI.)*
- [x] Capture USB devices (`system_profiler SPUSBDataType`).
- [ ] Capture the power/battery summary (`system_profiler SPPowerDataType`).
- [ ] Record the macOS version and the firmware (boot ROM) version from System Information.
- [ ] Check whether a **firmware password** is set (hold Option at boot; a lock icon prompt means it is). If it is, you will need it to boot from USB.
- [ ] Note the current battery cycle count and health for later reference.
- [ ] Take a Time Machine backup or confirm the machine holds nothing you cannot lose. Phase 0 should not touch the internal disk, but a backup is cheap insurance.

## Findings

Source: `.context/about_this_mac.json`.

| Area | Value |
| --- | --- |
| Model | MacBook Pro, identifier `MacBookPro13,3` |
| CPU | Quad-core Intel Core i7, 2.7 GHz, 4 cores, Hyper-Threading enabled, 256 KB L2 per core, 8 MB L3 |
| Memory | 16 GB |
| GPU 1 | Intel HD Graphics 530, built-in, 1536 MB dynamic VRAM. The JSON lists the built-in display (Retina LCD, 2880x1800) under this GPU |
| GPU 2 | AMD Radeon Pro 455, 2 GB, PCIe x8, vendor `0x1002` |
| GPU switching | "Automatic Graphics Switching: Supported", gMux version 4.0.29 [3.2.8] |
| Wi-Fi | Broadcom (vendor `0x14E4`), card ID shown as `0x15A`, 802.11 a/b/g/n/ac, locale ETSI, 2.4 GHz channels 1-13 and 5 GHz channels 36-165 |
| Wi-Fi firmware (macOS) | Broadcom BCM43xx 1.0 (7.77.111.1, AirPortDriverBrcmNIC-1710.4). This is Apple's driver and firmware, so it does not say what Linux will load |
| Keyboard / trackpad | One SPI device, "Apple Internal Keyboard / Trackpad", vendor `0x05ac`, product `0x0278` |
| USB host 1 | Intel xHCI (PCI `8086:a12f`), with the Apple T1 controller (`05ac:8600`) attached |
| USB host 2 | USB 3.1 bus (PCI `8086:15d4`). Likely the USB side of the Thunderbolt 3 controller; confirm with `lspci` |
| PCI | Nothing reported |

What this means for later plans:

- The "Intel only" graphics plan in [06](06_graphics.md) is live, because a discrete GPU really is present.
- The Apple T1 (not a T2) appears over USB. It handles the Touch Bar and Touch ID, which are deferred.
- The exact Wi-Fi chip is still unconfirmed. The file gives the vendor and a card ID, not the chip's PCI device ID, so confirm with `lspci -nn` in [03](03_boot_and_inventory.md).

## Record

The baseline is saved as `.context/about_this_mac.json` rather than `baseline_macos.txt`. The model identifier, GPUs and Wi-Fi vendor are filled into [results.md](results.md).

## Still open

- Power/battery summary and battery cycle count.
- macOS version and boot ROM version.
- Firmware password check.
- Backup confirmation.

None of these block the live USB work, but the firmware password check is worth doing before you try to boot from USB, since it would stop the Startup Manager.

## Done when

- [x] The model identifier, the GPU list and the Wi-Fi vendor are written down.
- [x] The dGPU question is answered: present, AMD Radeon Pro 455.
- [ ] The exact Wi-Fi chipset name is confirmed (needs `lspci -nn` on Linux).
- [ ] The firmware password check is done.

## If it goes wrong

- If `system_profiler` output is too long to read, save it to a file (`system_profiler SPDisplaysDataType > ~/Desktop/displays.txt`) and read it on the second computer.
