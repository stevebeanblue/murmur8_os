# 04: Wi-Fi

## Goal

Find out whether the built-in Broadcom Wi-Fi works under Linux, what firmware it needs, and confirm USB ethernet works as the fallback.

## Background

The vision doc expects Broadcom hardware that needs proprietary firmware, so the early plan is "use Linux's Broadcom support, with USB ethernet as a fallback". The chip is expected to be a Broadcom PCIe part handled by the `brcmfmac` driver; confirm that with `lspci -nn` rather than assuming it.

## Prerequisites

- [03](03_boot_and_inventory.md) done (booted into the live shell, inventory saved).
- Wi-Fi network credentials.
- The USB-C ethernet adapter and a cable.

## Steps

### Identify

- [ ] From the inventory, note the Wi-Fi device's PCI ID and which driver is bound (`lspci -nnk`). Compare with the chipset recorded in [01](01_macos_baseline.md).
- [ ] Check the kernel log for the driver and firmware:
  ```
  dmesg | grep -iE 'brcm|firmware|wlan'
  ```
- [ ] Note the **exact firmware filenames** the driver requests, and whether each load succeeded or failed. These names go straight into the Phase 2 packaging backlog.

### Test

- [ ] Check the interface and radio state:
  ```
  ip link
  rfkill list
  ```
- [ ] Scan and connect with the ISO's tooling (`iwctl`):
  ```
  iwctl
  station wlan0 scan
  station wlan0 get-networks
  station wlan0 connect <SSID>
  ```
  (The interface name may differ; use what `ip link` shows.)
- [ ] Confirm connectivity: `ping -c 5 archlinux.org`.
- [ ] Soak test: leave a ping or a large download running for about 10 minutes and note drops, stalls or disconnects.
- [ ] Try both 2.4 GHz and 5 GHz if your router offers both, and note any difference.
- [ ] Note signal strength compared with another device in the same spot, if convenient.

### Fallback

- [ ] Plug in the USB-C ethernet adapter and confirm it appears in `ip link` with a working driver.
- [ ] Get a DHCP lease and ping out. Record the adapter's chipset (`lsusb`), because adapters differ in driver support.
- [ ] If Wi-Fi does not work, use ethernet to copy logs and to download packages for [06](06_graphics.md).

## Record

- Wi-Fi: works / partial / fails, with the evidence (log lines, firmware filenames).
- Whether it needed anything beyond the stock ISO.
- The ethernet adapter model and chipset that worked.

## Done when

- The Wi-Fi chipset, driver and firmware filenames are written down.
- You have a verdict on Wi-Fi and a confirmed working wired fallback.

## If it goes wrong

- The firmware load fails: record the exact filenames from `dmesg`. Do not go hunting for blobs during Phase 0; it becomes a Phase 2 backlog item.
- The interface is missing entirely: save the `lspci -nnk` and `dmesg` output and mark Wi-Fi as "fails, driver or firmware".
- Flaky connection: run the soak test again after reboot, and note the pattern.
