# Windows on ARM installation notes for miatoll

These notes summarize the device-specific installation flow for supported
miatoll devices. They are not a substitute for the [maintained Miatoll
Guide](https://github.com/woa-miatoll/Miatoll-Guide). Follow its current
instructions and release notes; do not use generic Snapdragon flashing steps
or instructions for another device.

## Important distinction

`boot.img` is an Android boot image. Extracting it can provide porting inputs,
such as a device tree, but does not produce UEFI. Use the UEFI image published
for miatoll by the [Miatoll releases](https://github.com/woa-miatoll/Miatoll-Releases/releases/latest).
The device guide's Windows dual-boot procedure uses that image with its Android
helper app; it does not instruct users to flash a newly built UEFI image to an
arbitrary partition.

## Before starting

1. Confirm your exact phone variant is listed in the [Miatoll device guide](https://github.com/woa-miatoll/Miatoll-Guide).
   The codename alone does not make firmware interchangeable across devices.
2. Back up all data and make sure you have a tested way to restore the stock
   firmware. Unlocking the bootloader and repartitioning can erase data or make
   the phone unbootable.
3. Unlock the bootloader only by following Xiaomi's current official
   instructions for your exact model and region.
4. Download only the recovery image, Windows image, drivers, and UEFI package
   linked by the maintained Miatoll guide/releases. Read its warnings first.

## Device-specific installation outline

1. **Bootloader and recovery:** Follow the guide's installation prerequisites
   and recovery steps. It specifies a miatoll recovery image; a random custom
   recovery or image for a related phone is not a substitute.
2. **Partitioning:** Follow the guide's [partition instructions](https://github.com/woa-miatoll/Miatoll-Guide/blob/main/guide/1-partition.md)
   exactly, including its recovery-specific formatting steps. Partitioning is
   destructive. Do not improvise sizes, use Windows DiskPart to create/delete
   partitions, or copy partition commands from another device.
3. **Install Windows using one documented method:**
   - The [FFU method](https://github.com/woa-miatoll/Miatoll-Guide/blob/main/guide/ffu.md)
     is the simpler, less customizable option. Its guide warns that flashing
     erases data.
   - For manual installation, follow the guide's
     [Windows installation steps](https://github.com/woa-miatoll/Miatoll-Guide/blob/main/guide/2-install.md).
     It uses an ARM Windows ESD/WIM, not a raw ISO passed directly to DISM.
     The guide has you identify the existing `MainOS` and `EFIESP` volumes and
     assign drive letters; do not format or repartition them in DiskPart.
     Check the image's edition/index with `dism /get-imageinfo` and use the
     appropriate index when applying it to the `MainOS` volume.
4. **Drivers and boot files:** Use the matching driver package and the
   `OfflineUpdater.cmd` procedure documented in the manual-install guide.
   Create the Windows boot files as directed there (`bcdboot` targets the
   identified Windows and EFI volumes).
5. **Dual boot:** Complete the guide's
   [dual-boot setup](https://github.com/woa-miatoll/Miatoll-Guide/blob/main/guide/3-dualboot.md).
   That procedure uses the miatoll UEFI release, the specified StA utility,
   and M3K Helper on Android; it requires root. Follow that page for placing
   files and switching operating systems.

The published guide may change. Check its current Windows-version warnings,
device status, and release notes immediately before installation. If a step
does not match your exact device or downloaded package, stop rather than
guessing.
