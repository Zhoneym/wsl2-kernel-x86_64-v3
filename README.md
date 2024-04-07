# wsl2-kernel-x86_64-v3
Stable/Longterm Kernel for Windows Subsystem for Linux version 2 (WSL2)

It has higher performance than the official kernel(about 8-12% improvement), and compiles specify link-time optimizations using ThinLTO, enabling a variety of extended instruction sets.


| Kernel Version | Status |
|:--------------:|:------:|
| 6.6.25 | Support until release new LTS kernel |
| 6.8.4 | Support |


[![compile wsl2 kernel](https://github.com/Zhoneym/wsl2-kernel-x86_64-v3/actions/workflows/build.yml/badge.svg)](https://github.com/Zhoneym/wsl2-kernel-x86_64-v3/actions/workflows/build.yml)

The source code comes from:

https://github.com/Nevuly/WSL2-Linux-Kernel-Rolling

https://github.com/Nevuly/WSL2-Linux-Kernel-Rolling-LTS

Official kernel repository:

https://github.com/microsoft/WSL2-Linux-Kernel

# Feature Requests

Is there a missing feature that you'd like to see? Please request it on the
[WSL GitHub project][wsl-issue].

If you're able and interested in contributing kernel code for your feature
request, we encourage you to [submit the change upstream][submit-patch].

# Build Instructions

Instructions for building an x86_64 WSL2 kernel with an Arch-Linux Rolling distribution are
as follows:

1. Install the build dependencies (Arch Linux):  

   `$ sudo pacman -S aarch64-linux-gnu-gcc bc bison curl flex gcc git pahole python unzip wget zip`

2. Build the kernel using the WSL2 kernel configuration (x86_64-v3):  

   `$ sed -i 's/-WSL2-STABLE/-stable-x86_64-v3-wsl2/g' arch/x86/configs/config-wsl-x86`

   or

   `$ sed -i 's/-WSL2-LTS/-longterm-6.1-x86_64-v3-wsl2/g' arch/x86/configs/config-wsl-x86`

   `$ make KCONFIG_CONFIG=arch/x86/configs/config-wsl-x86 -j $(nproc) CFLAGS="-march=x86-64-v3 -mtune=native -O3 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=2 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -mcx16 -mpopcnt -msse4.1 -msse4.2 -mssse3 -mavx -mavx2 -mbmi -mbmi2 -mf16c -mfma -mmovbe -mxsave" CXXFLAGS="$CFLAGS -Wp,-D_GLIBCXX_ASSERTIONS" LTOFLAGS="-flto=thin -falign-functions=32" RUSTFLAGS="-Copt-level=3 -Ctarget-cpu=x86-64-v3" GOAMD64="v3"`

   
   Build the kernel using the WSL2 kernel configuration (x86_64-v4):  

   `$ sed -i 's/-WSL2-STABLE/-stable-x86_64-v3-wsl2/g' arch/x86/configs/config-wsl-x86`

   or

   `$ sed -i 's/-WSL2-LTS/-longterm-6.1-x86_64-v3-wsl2/g' arch/x86/configs/config-wsl-x86`

   `$ make KCONFIG_CONFIG=arch/x86/configs/config-wsl-x86 -j $(nproc) CFLAGS="-march=x86-64-v4 -mtune=native -O3 -pipe -fno-plt -fexceptions -Wp,-D_FORTIFY_SOURCE=2 -Wformat -Werror=format-security -fstack-clash-protection -fcf-protection -mcx16 -mpopcnt -msse4.1 -msse4.2 -mssse3 -mavx -mavx2 -mavx512f -mbmi -mbmi2 -mf16c -mfma -mmovbe -mxsave" CXXFLAGS="$CFLAGS -Wp,-D_GLIBCXX_ASSERTIONS" LTOFLAGS="-flto=thin -falign-functions=32" RUSTFLAGS="-Copt-level=4 -Ctarget-cpu=x86-64-v4" GOAMD64="v4"`
   
3. Build the kernel using the WSL2 kernel configuration (arm64):  

   `$ export ARCH=arm64 && export CROSS_COMPILE=aarch64-linux-gnu-`  

   `$ cp arch/arm64/config/config-wsl-arm64 .config`

   `$ make -j $(nproc)`
   
4. Build and install the kernel modules

   `$ sudo make modules_install`
   
   or, without installation, save the module to a separate folder

   `$ mkdir -p modules_install && make modules_install INSTALL_MOD_PATH=modules_install`
   
# Install Instructions

Please see the documentation on the [.wslconfig configuration
file][install-inst] for information on using a custom built kernel.

[about-wsl2]:   https://docs.microsoft.com/en-us/windows/wsl/about#what-is-wsl-2
[wsl-issue]:    https://github.com/microsoft/WSL/issues/new/choose
[kernel-lts]:   https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/log/?h=linux-6.1.y
[normal-bug]:   https://www.kernel.org/doc/html/latest/admin-guide/bug-hunting.html#reporting-the-bug
[security-bug]: https://www.kernel.org/doc/html/latest/admin-guide/security-bugs.html
[submit-patch]: https://www.kernel.org/doc/html/latest/process/submitting-patches.html
[install-inst]: https://docs.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig
