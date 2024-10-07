# Cadmium, A Linux ~~distro~~ installer for some RISC laptops

<p align="center"><img src="/pics/logo/cd_smol.png" alt="Logo" data-canonical-src="/pics/cd_smol.png"/></p>

Thanks @LoganMD for the logo

## Pre-requisites

### User

- Patience and time.
- A working Linux machine.
- Familiarity with Cadmium's [wiki](<https://github.com/Maccraft123/Cadmium/wiki>).
- Join Cadmium's [Discord server](<https://discord.gg/ZZbwyvKCmV>).

### Linux System

**If you plan to compile, ensure your build system meets the following requirements (Debian is recommended, but not mandatory):**

- A working LLVM toolchain.
- Build dependencies for kernel compilation.
- `debootstrap` (if using a Debian root filesystem).
- `qemu-user-static` with binfmt support (for cross-architecture builds).
- Firmware-specific packages:
  - For Chromebook machines with stock boot firmware: `vboot-utils`, `u-boot-tools` (includes `vbutil_kernel`, `futility`, `cgpt`, and `mkimage`).
  - For EFI machines: `ukify` (usually included in `systemd-boot` or a related package).
- `bc` for calculating the number of threads for compilation.
- `curl` for downloading the kernel.
- `bsdtar` for writing archive files (from the `libarchive-tools` package).
- `f2fs-tools` for creating the filesystem used by Cadmium.
- `parted` for preparing the GPT table to be modified by `cgpt`.
- `rsync` for copying files.

*To check if your build system meets the minimum requirements, run `./check`. If the output is empty, you're set!*

**Alternatively, you can use the available [releases](<https://github.com/Maccraft123/Cadmium/releases>) if they are recent enough.**

- `tar` for extracting the archive.
- `dd` for flashing the image onto the pendrive.

### Target Chromebook (Stock Firmware)

1. Enter Developer Mode. Google provides a [guide](<https://www.chromium.org/chromium-os/developer-library/guides/device/developer-mode/#enable-developer-mode>).
2. Enable booting from USB:
    - Boot into ChromeOS (guest account is fine).
    - Access VT-2 (virtual terminal 2) by pressing `ctrl + alt + F2`.
    - Login to `root` or `chronos` without a password.
    - Run the following command:
    ```shell
    sudo crossystem dev_boot_usb=1 dev_boot_signed_only=0
    ```
3. It's also highly recommended you change a few gbb flags to avoid losing your data unexpectedly (more info can be found on [GitHub](<https://github.com/hexdump0815/linux-mainline-on-arm-chromebooks?tab=readme-ov-file#setting-gbb-flags-enabling-ccd-and-the-magic-suzyqable>)).
4. If you plan to keep ChromeOS, it's recommended to disable the root account and set a password for the user `chronos` using:
```shell
chromeos-setdevpasswd
```

## Compiling

1. Edit `config` to reflect your board.
2. Run `build-all` on your build system:
    - To build to a file:
    ```shell
    ./build-all <file> <size>  # 8G should be sufficient
    ```
    - To build directly onto a pendrive or external drive:
    ```shell
    ./build-all /dev/<disk>
    ```

## Installing

1. Get your pendrive ready, if it isn't already:
    - Flash the image file you compiled previously.
    - Alternatively, extract and flash `cadmium-<device>.img.xz` from [releases](<https://github.com/Maccraft123/Cadmium/releases>) to your pendrive.
2. Shut your target machine down, then boot to USB.
3. Once booted, run:
```shell
./install
```
to install Cadmium onto the target storage medium. An internet connection is required.

4. If you're already running Cadmium and wish to update the kernel, simply run:
```shell
./install-kernel
```
from a pendrive running a newer release.

#### *Note that Binary drivers are unsupported in Cadmium and never will be.*