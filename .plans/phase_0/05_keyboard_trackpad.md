# 05: Keyboard and trackpad

## Goal

Find out whether the built-in keyboard and trackpad work under the live kernel, and what is needed if they do not.

## Background

On this generation the keyboard and trackpad are connected over SPI, not USB. The vision doc's early plan is to use an external USB keyboard during bring-up. Mainline Linux has an `applespi` driver for SPI keyboards and trackpads on Macs of this era, but whether it loads and works on this exact model with the ISO's kernel is what Phase 0 needs to find out. Do not assume either way.

**Known from the macOS baseline** ([01](01_macos_baseline.md)): the keyboard and trackpad appear as a single SPI device, "Apple Internal Keyboard / Trackpad", vendor `0x05ac`, product `0x0278`. Use those values when checking which driver claims it and whether the driver lists that product ID. The Apple T1 controller (`05ac:8600`) also shows up over USB; it drives the Touch Bar, which is deferred, so ignore it here.

## Prerequisites

- [03](03_boot_and_inventory.md) done. The external USB keyboard stays connected throughout so you are never locked out.

## Steps

- [ ] Look for the SPI devices and drivers in the inventory:
  ```
  lsmod | grep -iE 'spi|apple|lpss'
  dmesg | grep -iE 'spi|applespi|lpss|keyboard|trackpad'
  ```
- [ ] List input devices the kernel created:
  ```
  cat /proc/bus/input/devices
  ls /dev/input
  ```
- [ ] Test the **built-in keyboard** in the shell: type, and check the common keys (letters, Shift, Ctrl, Alt/Option, Command, Fn, arrows, Escape, Backspace). Note any that do nothing or produce wrong characters.
- [ ] Test the **trackpad**: use `libinput debug-events` (if available) or `evtest` to see whether motion, clicks and taps register, then note multi-touch if it shows up.
- [ ] Check the **function keys** (brightness and volume). These may behave differently from the media keys; one line each is enough.
- [ ] If the built-in input does not work, try loading the driver by hand and note the result:
  ```
  modprobe applespi
  dmesg | tail
  ```
- [ ] Check that the **external USB keyboard and a USB mouse** work too, because they are the fallback.
- [ ] Note the keyboard layout the ISO assumes versus the physical layout (ISO versus ANSI, and where the keys land).

## Record

- Built-in keyboard: works / partial / fails, with the evidence.
- Trackpad: works / partial / fails, with the evidence.
- Which kernel modules were involved and whether any had to be loaded by hand.
- Whether the external keyboard is required to boot and log in.

## Done when

- You know whether the SPI keyboard and trackpad work with the stock ISO kernel, and have the relevant log lines saved.
- You have a clear statement: "external keyboard required for bring-up: yes or no".

## If it goes wrong

- Nothing registers on the built-in keyboard: confirm the external keyboard still works, save `dmesg`, and mark it "fails". The fix (kernel config, a module, or an out-of-tree driver) is a Phase 4 backlog item, not a Phase 0 task.
- A key is missing or wrong: record the key and what it produced; do not remap anything now.
