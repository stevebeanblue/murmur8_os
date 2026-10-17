# 06: Graphics

## Goal

Record how the Intel HD 530 and (if present) the discrete AMD Radeon Pro behave under the live kernel, and test whether the "Intel only" early plan is practical.

## Background

The vision doc plans to use Intel graphics only. Most 15-inch 2016 models also carry a discrete AMD Radeon Pro, and the macOS baseline ([01](01_macos_baseline.md)) confirms this one does: an **AMD Radeon Pro 455** (2 GB, PCIe x8) alongside the Intel HD Graphics 530. macOS reports automatic graphics switching and a gMux (version 4.0.29), and it lists the built-in Retina display under the Intel GPU. Both GPUs will likely be visible to Linux regardless of which one you intend to use, so Phase 0 records what each one does by default.

## Prerequisites

- [03](03_boot_and_inventory.md) done and the GPU list from [01](01_macos_baseline.md).
- Ethernet or Wi-Fi from [04](04_wifi.md), only needed for the optional tests at the end.

## Steps

### Identify and bind

- [ ] List the display controllers and the driver bound to each:
  ```
  lspci -nnk | grep -A3 -E 'VGA|Display|3D'
  ```
  Expect `i915` for the Intel GPU. Record what binds to the AMD GPU, if it appears (`amdgpu`, `radeon`, or nothing).
- [ ] List the graphics devices the kernel exposes:
  ```
  ls /sys/class/drm
  ```
- [ ] Check which GPU is driving the built-in display, and the native resolution on the console (the Retina panel should look sharp and correctly sized, not stretched).
- [ ] Look for relevant kernel messages:
  ```
  dmesg | grep -iE 'i915|amdgpu|radeon|drm|gmux|vga'
  ```

### Display behaviour

- [ ] **Backlight:** check `/sys/class/backlight`, and try changing brightness by writing to the device (for example `echo <value> > /sys/class/backlight/<name>/brightness`) or with the brightness keys.
- [ ] **External display:** connect a monitor over USB-C (DisplayPort alt mode or a USB-C to HDMI/DisplayPort adapter) and record whether it is detected and which GPU drives it. Skip if you have no adapter.
- [ ] **Console stability:** switch between virtual consoles (`Ctrl+Alt+F2`, `F1`) and note any flicker, corruption or hangs.

### "Intel only" test

- [ ] Note the baseline first: is the AMD GPU bound and active by default? Is the fan spinning up or the machine getting hot at idle?
- [ ] Reboot and add `modprobe.blacklist=amdgpu` (and `modprobe.blacklist=radeon` if that driver bound) to the kernel command line from the boot menu.
- [ ] Repeat the checks above. Record whether the built-in display still works, whether the external display still works, and whether idle heat or fan noise changes.
- [ ] If `vga_switcheroo` is available, note whether the GPU switching interface is present (`/sys/kernel/debug/vgaswitcheroo/switch` after mounting debugfs). Observe only; do not rely on it.

### Optional: acceleration check

Only if you have a working network and the live system has room (the live image is RAM-backed, so installs are limited and are lost on reboot):

- [ ] Install a small tool that reports the render and video-decode stack (for example a VA-API info utility) and record whether hardware video decode is reported for the Intel GPU.
- [ ] Do not set up a desktop environment in Phase 0. A graphical session test belongs in a later phase.

## Record

- Intel HD 530: works / partial / fails, with the driver and any parameters needed.
- AMD GPU: present / absent, driver bound, behaviour when left alone.
- Whether blacklisting the AMD driver is a viable "Intel only" approach, and its effect on heat and fan noise.
- External display and backlight results.

## Done when

- Both GPUs are identified, with the driver bound to each.
- The built-in display works at native resolution, or the workaround is recorded.
- You have a verdict on the "Intel only" approach (works, works with caveats, or does not work).

## If it goes wrong

- A blank screen after boot: use `nomodeset` to reach a shell, save the logs, and mark graphics "partial" with the details.
- Blacklisting the AMD driver gives a black screen: reboot without it and record that the muxing needs more investigation (a Phase 2 or Phase 4 backlog item).
