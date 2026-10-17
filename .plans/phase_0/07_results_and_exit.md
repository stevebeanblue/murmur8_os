# 07: Results and exit

## Goal

Turn the raw notes from plans 01 to 06 into a single results record, make the decisions Phase 0 exists to make, and hand clean inputs to Phase 1 and Phase 2.

## Results table

Copy this into `.plans/phase_0/results.md` and fill it in. Use one status per row: **works**, **partial**, **fails** or **not tested**.

| Area | Status | Evidence (log line, command, photo) | Workaround found | Follow-up phase |
| --- | --- | --- | --- | --- |
| Boots from USB | | | | |
| Wi-Fi (built-in) | | | | |
| Wi-Fi firmware needed | | filenames from `dmesg` | | |
| USB ethernet fallback | | adapter and chipset | | |
| Built-in keyboard | | | | |
| Built-in trackpad | | | | |
| External USB keyboard | | | | |
| Intel HD 530 | | | | |
| AMD Radeon Pro (present?) | | | | |
| "Intel only" approach | | | | |
| Built-in display brightness | | | | |
| External display | | | | |
| Audio (observed only) | | | | |
| Touch Bar (observed only) | | | | |
| Suspend/resume (observed only) | | | | |

## Machine facts to record

- Model identifier, CPU, RAM, GPU(s), Wi-Fi chipset (from [01](01_macos_baseline.md)).
- ISO filename and checksum, and the kernel version it ran (from [02](02_live_usb.md) and [03](03_boot_and_inventory.md)).
- Boot parameters needed, if any.
- Date of the test run.

## Decisions to record

Write a sentence for each, with the evidence behind it:

1. **Is an external keyboard required for bring-up?**
2. **Is built-in Wi-Fi viable, or is USB ethernet the working assumption for now?** List the firmware files Phase 2 must package.
3. **What is the GPU strategy?** Intel only (and how the AMD driver is kept quiet), or something else.
4. **Does the live image's kernel get far enough that Phase 2 can start from the stock Arch kernel, or are patches or out-of-tree drivers needed straight away?**
5. **Is the logs-over-USB-C-ethernet debugging path good enough?** (No serial port on this machine, per the vision doc.)

## Follow-up backlog items

For each "partial" or "fails" row, add an entry to `.project_artefacts/backlog.md` with the phase it belongs to. Example shape:

```
- [ ] (Phase 4) Built-in keyboard does not register on the live kernel: investigate SPI driver options. Evidence: phase0-<date>/dmesg.txt
```

Also add items for the things Phase 0 deliberately deferred (audio, Touch Bar, suspend/resume), tagged as deferred rather than as problems.

## Exit criteria

Phase 0 is complete when all of these are true:

- [ ] The live image booted on the MacBook, or the failure is fully documented.
- [ ] The results table has a status for Wi-Fi, keyboard, trackpad and graphics, each with saved evidence.
- [ ] The dGPU question is answered, and the "Intel only" plan has a verdict.
- [ ] Logs from the real machine are saved off it and committed or stored where Phase 1 can use them.
- [ ] The five decisions above are written down.
- [ ] Backlog items exist for every partial or failed row.
- [ ] The results are committed to the repo.

## What Phase 1 takes from here

Phase 1 builds an ISO in a container and boots it in a VM, so every later change has a safe test loop. From Phase 0 it needs:

- The ISO and kernel version that were tested.
- The list of kernel parameters and modules that mattered.
- The firmware filenames and the driver list, so the VM image can be compared with the real machine.
