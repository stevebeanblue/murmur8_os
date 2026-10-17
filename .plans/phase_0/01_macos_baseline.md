# 01: macOS baseline

## Goal

Capture hardware facts while macOS still boots, so the Linux results have something to be compared against. This also settles the open question in the vision doc: does this machine have a discrete AMD Radeon Pro?

## Prerequisites

- The MacBook boots into macOS.
- A place to save text output (a notes file on the second computer or a USB stick).

## Steps

- [ ] Open **About This Mac** and record the model name, year, processor, memory and graphics line(s). Two graphics entries means a discrete GPU is present.
- [ ] In **System Information**, record the **Model Identifier** (expected to be `MacBookPro13,3` for the 15-inch 2016; confirm it).
- [ ] Capture graphics details:
  ```
  system_profiler SPDisplaysDataType
  ```
- [ ] Capture the Wi-Fi card details:
  ```
  system_profiler SPAirPortDataType
  ```
- [ ] Capture the PCI device list:
  ```
  system_profiler SPPCIDataType
  ```
- [ ] Capture USB devices and the power/battery summary:
  ```
  system_profiler SPUSBDataType SPPowerDataType
  ```
- [ ] Record the macOS version and the firmware (boot ROM) version from System Information.
- [ ] Check whether a **firmware password** is set (hold Option at boot; a lock icon prompt means it is). If it is, you will need it to boot from USB.
- [ ] Note the current battery cycle count and health for later reference.
- [ ] Take a Time Machine backup or confirm the machine holds nothing you cannot lose. Phase 0 should not touch the internal disk, but a backup is cheap insurance.

## Record

Save the output as `baseline_macos.txt` in the logs location (see [03](03_boot_and_inventory.md)). Fill in the first rows of the results table in [07](07_results_and_exit.md): model identifier, GPU(s) present, Wi-Fi chipset.

## Done when

- The model identifier, the GPU list and the Wi-Fi chipset name are written down.
- The dGPU question is answered: present or absent, and which Radeon Pro model.

## If it goes wrong

- If `system_profiler` output is too long to read, save it to a file (`system_profiler SPDisplaysDataType > ~/Desktop/displays.txt`) and read it on the second computer.
