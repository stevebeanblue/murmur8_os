# Phase 0: Hardware check

Boot a live USB on the 2016 MacBook Pro and note what works: Wi-Fi, input and graphics.

Phase 0 is reconnaissance. Nothing is installed, nothing on the internal disk is changed, and nothing is fixed. The output is evidence that decides what Phase 1 (test VM) and Phase 2 (pinned base) have to deal with.

## Scope

In scope, matching the vision doc:

- **Wi-Fi** (Broadcom, proprietary firmware), with USB ethernet as the fallback.
- **Keyboard and trackpad** (SPI, not USB), with an external USB keyboard during bring-up.
- **Graphics** (Intel HD 530, plus a discrete AMD Radeon Pro on most 15-inch 2016 models).

Out of scope: audio, Touch Bar and suspend/resume. The vision doc defers these. Note anything obvious you see, but do not spend time on it.

## Plans

Work through them in order.

| # | Plan | Outcome |
| --- | --- | --- |
| 01 | [macOS baseline](01_macos_baseline.md) | Hardware facts captured while macOS still works, including whether the dGPU exists |
| 02 | [Live USB](02_live_usb.md) | A verified Arch live USB and a second stick (or share) for logs |
| 03 | [Boot and inventory](03_boot_and_inventory.md) | The machine boots the live image and a full hardware dump is saved off-machine |
| 04 | [Wi-Fi](04_wifi.md) | Wi-Fi verdict, firmware needs, and ethernet fallback confirmed |
| 05 | [Keyboard and trackpad](05_keyboard_trackpad.md) | Verdict on built-in SPI input versus external keyboard |
| 06 | [Graphics](06_graphics.md) | Intel and AMD behaviour recorded; "Intel only" plan tested |
| 07 | [Results and exit](07_results_and_exit.md) | Results table filled, decisions recorded, follow-up backlog items drafted |

Results are tracked in [results.md](results.md), which already holds the macOS baseline facts.

## Equipment

- The MacBook Pro (15-inch, 2016), charged or on power.
- Two USB sticks: one for the live image, one for logs. A single stick can work if you partition it, but two is simpler.
- A USB-C ethernet adapter and a network cable. The machine only has USB-C ports, so this is also how logs can leave the machine.
- An external USB keyboard (and a USB-C to USB-A adapter if needed).
- A second computer to build the stick and read the logs.
- A phone for photographing screens when something fails before logging works.

## Ground rules

1. Do not touch the internal disk. Boot from USB only, and do not run any installer.
2. Save logs off the machine before powering down, because the live system's journal is lost on reboot.
3. Record failures as carefully as successes. "Does not work, here is the dmesg line" is a good result.
4. Do not fix anything in Phase 0. Fixes belong in later phases; write them down as backlog items.

## Done when

See [07_results_and_exit.md](07_results_and_exit.md) for the exit criteria.
