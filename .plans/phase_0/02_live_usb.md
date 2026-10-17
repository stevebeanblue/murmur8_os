# 02: Live USB

## Goal

Produce a verified Arch Linux live USB that boots on the MacBook, plus a second stick for saving logs.

## Why Arch

The project is built on Arch, and the live ISO ships a recent kernel and current firmware, which gives the best chance that this generation of Mac hardware works out of the box. Testing the same family of image that Phase 2 will pin means Phase 0 results carry forward.

## Prerequisites

- A second computer (macOS or Linux) and two USB sticks.
- A network connection on the second computer.

## Steps

- [ ] Download the current ISO from the official Arch Linux download page, using a mirror close to you.
- [ ] Verify it before writing it. Check the SHA-256 checksum against the one published on archlinux.org, and verify the PGP signature if you can.
- [ ] Record the ISO filename and checksum in the logs notes. Phase 1 will reuse the same ISO in a VM.
- [ ] Identify the USB stick's device name carefully.
  - macOS: `diskutil list`
  - Linux: `lsblk`
- [ ] Unmount the stick (do not eject), then write the ISO to the raw device with `dd`. Be certain of the device name; writing to the wrong disk destroys its contents.
  - macOS: `sudo dd if=archlinux.iso of=/dev/rdiskN bs=4m status=progress`
  - Linux: `sudo dd if=archlinux.iso of=/dev/sdX bs=4M status=progress conv=fsync`
- [ ] Safely eject the stick.
- [ ] Prepare the **logs stick**: format it as FAT32 (or ext4 if you are comfortable mounting it) and label it `M8LOGS`.
- [ ] Optional sanity test: boot the written ISO in a VM (QEMU or similar) on the second computer, to rule out a bad write before taking it to the MacBook. This is also a tiny first step toward Phase 1.

## Record

- ISO filename, date and checksum.
- Which stick is which (label them physically).

## Done when

- The ISO checksum matched.
- The live stick is written and the logs stick is formatted.
- Optional: the ISO boots in a VM.

## If it goes wrong

- Checksum mismatch: re-download from a different mirror; do not write a mismatched ISO.
- Stick not recognised on the MacBook later: re-write it, try the other stick, or try a different USB-C to USB-A adapter. Some hubs behave badly at boot, so plug the stick in directly if possible.
