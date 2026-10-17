# Phase 0 results

Working record for the hardware check. Fill it in as plans 02 to 06 are carried out; the exit criteria are in [07_results_and_exit.md](07_results_and_exit.md).

Statuses describe **Linux** behaviour on the live image, so everything is "not tested" until the first boot. The evidence column carries the macOS baseline where it is already known.

## Machine facts

Source: `.context/about_this_mac.json` (macOS baseline).

| Item | Value |
| --- | --- |
| Model | MacBook Pro (15-inch, 2016), `MacBookPro13,3` |
| CPU | Quad-core Intel Core i7, 2.7 GHz, 4 cores, Hyper-Threading |
| Memory | 16 GB |
| GPUs | Intel HD Graphics 530 (1536 MB dynamic) and AMD Radeon Pro 455 (2 GB, PCIe x8) |
| GPU switching | Supported in macOS, gMux 4.0.29 |
| Wi-Fi | Broadcom (vendor `0x14E4`, card ID `0x15A`); exact chip to confirm with `lspci -nn` |
| Keyboard / trackpad | SPI, Apple Internal Keyboard / Trackpad (`05ac:0278`) |
| Other | Apple T1 controller over USB (`05ac:8600`); Intel xHCI `8086:a12f`; USB 3.1 bus `8086:15d4` |
| ISO and checksum | not yet recorded |
| Kernel version on the ISO | not yet recorded |
| Boot parameters needed | not yet recorded |
| Date of test run | not yet recorded |

## Results table

| Area | Status | Evidence (log line, command, photo) | Workaround found | Follow-up phase |
| --- | --- | --- | --- | --- |
| Boots from USB | not tested | | | |
| Wi-Fi (built-in) | not tested | macOS baseline: Broadcom, 802.11 a/b/g/n/ac | | |
| Wi-Fi firmware needed | not tested | filenames from `dmesg` | | |
| USB ethernet fallback | not tested | adapter and chipset | | |
| Built-in keyboard | not tested | macOS baseline: SPI device `05ac:0278` | | |
| Built-in trackpad | not tested | macOS baseline: same SPI device | | |
| External USB keyboard | not tested | | | |
| Intel HD 530 | not tested | macOS baseline: present, drives the built-in display | | |
| AMD Radeon Pro (present?) | not tested | macOS baseline: present, Radeon Pro 455, 2 GB | | |
| "Intel only" approach | not tested | | | |
| Built-in display brightness | not tested | | | |
| External display | not tested | | | |
| Audio (observed only) | not tested | | | |
| Touch Bar (observed only) | not tested | macOS baseline: Apple T1 present | | |
| Suspend/resume (observed only) | not tested | | | |

## Decisions

Fill these in once the evidence exists.

1. **External keyboard required for bring-up?** pending
2. **Is built-in Wi-Fi viable, or is USB ethernet the working assumption?** pending (list the firmware files Phase 2 must package)
3. **GPU strategy:** pending
4. **Can Phase 2 start from the stock Arch kernel?** pending
5. **Is logs-over-USB-C-ethernet debugging good enough?** pending
