# 03: Boot and inventory

## Goal

Boot the live image on the MacBook, capture a complete hardware inventory and the kernel log, and save everything off the machine.

## Prerequisites

- [01](01_macos_baseline.md) and [02](02_live_usb.md) done.
- An **external USB keyboard** connected. The built-in keyboard is on SPI and may not work in the live environment, so do not rely on it yet (see [05](05_keyboard_trackpad.md)).
- The logs stick (`M8LOGS`) ready, and the USB-C ethernet adapter on hand.

## Steps

### Boot

- [ ] Shut the MacBook down fully.
- [ ] Plug in the external keyboard and the live stick.
- [ ] Power on while holding **Option (⌥)** to open the Startup Manager. With an external keyboard, hold Option on that keyboard as soon as you press power.
- [ ] Choose the entry for the stick (typically labelled "EFI Boot").
- [ ] At the Arch boot menu, pick the default entry. If the screen is blank or garbled, note exactly what you see (photograph it), then reboot and try again.
- [ ] If the display fails, try the same boot with the `nomodeset` kernel parameter (press `e` on the boot entry and add it). Record that you needed it, because that is a graphics finding for [06](06_graphics.md).

### Inventory

Run these in the live shell and save the output. Use whichever tools exist on the ISO; if one is missing, note that and move on.

- [ ] Kernel and system identity:
  ```
  uname -a
  dmidecode -t system
  ```
- [ ] PCI devices with the driver in use:
  ```
  lspci -nnk
  ```
- [ ] USB devices:
  ```
  lsusb -t
  ```
- [ ] Loaded modules and the full kernel log:
  ```
  lsmod
  dmesg
  dmesg --level=err,warn
  ```
- [ ] The boot journal:
  ```
  journalctl -b --no-pager
  ```
- [ ] Block devices (confirming the internal disk is visible but **not mounted**):
  ```
  lsblk
  ```
- [ ] Anything obviously working or broken that is out of scope (audio, Touch Bar, lid, brightness keys). One line each is enough.

### Save the logs

- [ ] Mount the logs stick:
  ```
  mkdir -p /mnt/logs && mount /dev/sdX1 /mnt/logs
  ```
  (Find the right device with `lsblk`; do not use the internal disk or the live stick.)
- [ ] Copy every output into a dated folder, for example `/mnt/logs/phase0-YYYY-MM-DD/`.
- [ ] `sync`, then unmount before removing the stick.
- [ ] Alternatively, once ethernet works (see [04](04_wifi.md)), copy the logs to the second computer over the network.

## Record

- Whether the machine booted at all, and any boot parameters needed.
- Which kernel version the ISO ran.
- The saved log folder.

## Done when

- The live image booted to a shell.
- The inventory files are on the logs stick (or the second computer).
- You have a list of every PCI and USB device with the driver bound to it.

## If it goes wrong

- The stick does not appear in the Startup Manager: re-check the write in [02](02_live_usb.md), try the other stick, plug it in directly rather than via a hub.
- The system hangs at boot: photograph the last screen, retry with `nomodeset`, and record the result either way.
- You cannot save logs: photograph the screen output as a last resort, and use the `less` pager so you can capture the key lines.
