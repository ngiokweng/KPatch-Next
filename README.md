# KPatch-Next

**Patching and hooking the Linux kernel with only stripped Linux kernel image.**

``` shell
 _  ______       _       _           _   _           _   
| |/ /  _ \ __ _| |_ ___| |__       | \ | | _____  _| |_ 
| ' /| |_) / _` | __/ __| '_ \ _____|  \| |/ _ \ \/ / __|
| . \|  __/ (_| | || (__| | | |_____| |\  |  __/>  <| |_ 
|_|\_\_|   \__,_|\__\___|_| |_|     |_| \_|\___/_/\_\\__|

```

- Obtain all symbol information without source code and symbol information.
- Inject arbitrary code into the kernel. (Static patching the kernel image or Runtime dynamic loading).
- Kernel function inline hook and syscall table hook are provided.
- Pure KPM module support for all root managers i.e Magisk & KernelSU/N (Except APatch).
- Checkout our magisk/kernelsu module! [KPatch-Next-Module](https://github.com/KernelSU-Next/KPatch-Next-Module)

## Requirement

CONFIG_KALLSYMS=y
CONFIG_KALLSYMS_ALL=y

or

CONFIG_KALLSYMS=y
CONFIG_KALLSYMS_ALL=n (Initial support)

or

CONFIG_KALLSYMS=y

## Build Environment

Before building, install the required AArch64 bare-metal toolchain and Android NDK, then configure them as permanent environment variables.

### 1. Install Arm GNU Toolchain

Use Arm GNU Toolchain `12.2.rel1` for `kpimg`.

KPatch-Next's early boot code is sensitive to compiler code generation. Newer bare-metal toolchains, such as Arm GNU Toolchain 15.x, can emit calls such as `memcpy` in the relocated setup/map code and produce a `kpimg` that patches successfully but fails to boot. Keep the `aarch64-none-elf` compiler pinned to `12.2.rel1`.

Download the `arm-gnu-toolchain-12.2.rel1-*-aarch64-none-elf` package for your host system from:

https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads

For example, on Linux x86_64, extract the `12.2.rel1` archive to a stable local path:

```shell
mkdir -p ~/dev/toolchain
tar -xf arm-gnu-toolchain-12.2.rel1-x86_64-aarch64-none-elf.tar.xz -C ~/dev/toolchain
```

### 2. Install Android NDK

Download the Android NDK from:

https://github.com/android/ndk/wiki/Unsupported-Downloads

The official release workflow uses NDK r26b. NDK r29 has also been tested successfully for the Android userspace tools, so the NDK version is less critical than the Arm GNU Toolchain version used for `kpimg`.

Extract it to a stable local path, such as:

```shell
mkdir -p ~/dev/android
unzip android-ndk-*.zip -d ~/dev/android
```

### 3. Configure permanent environment variables

Add the following lines to your shell profile. For Bash, use `~/.bashrc`; for Zsh, use `~/.zshrc`.

```shell
export PATH="$HOME/dev/toolchain/arm-gnu-toolchain-12.2.rel1-x86_64-aarch64-none-elf/bin:$PATH"
export ANDROID_NDK="$HOME/dev/android/android-ndk-<version>"
```

Replace only the Android NDK `<version>` with the actual extracted directory name.

Apply the changes with the profile file used by your shell:

```shell
source ~/.bashrc
# or
source ~/.zshrc
```

Verify the environment:

```shell
aarch64-none-elf-gcc --version  # should report 12.2.rel1
test -f "$ANDROID_NDK/build/cmake/android.toolchain.cmake" && echo "ANDROID_NDK OK"
```

After the environment is configured, build the project directly:

```shell
./build.sh
```

## Supported Versions

Currently only supports arm64 architecture.  

Linux 3.18 - 6.12 (theoretically)  

## Get Involved

## More Information

[Documentation](./doc/)

## Credits

- [KernelPatch](https://github.com/bmax121/KernelPatch): Special thanks to the author for making this project possible.
- [vmlinux-to-elf](https://github.com/marin-m/vmlinux-to-elf): Some ideas for parsing kernel symbols.
- [android-inline-hook](https://github.com/bytedance/android-inline-hook): Some code for fixing arm64 inline hook instructions.
- [tlsf](https://github.com/mattconte/tlsf): Memory allocator used for KPM. (Need another to allocate ROX memory.)

## License

KPatch-Next is licensed under the **GNU General Public License (GPL) 2.0** (<https://www.gnu.org/licenses/old-licenses/gpl-2.0.html>).
