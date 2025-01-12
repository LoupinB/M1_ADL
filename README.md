# Projet M1 Analyse et Développement Logiciel
Ce projet est consacré à l'analyse de l'hyperviseur raspvisor par 4 étudiants (Louis BOURDIL, Maxence NAYET, Kwami Maxwell NUMANYO, Ulrich TATCHOU) dans le cadre d'une UE du Master 1 Cryptis à Limoges.
Notre analyse de chaque fichier est disponible sur le [wiki](https://github.com/maxilli13/raspvisor_M1/wiki).

# raspvisor
A simple type-1 hypervisor on Raspberry Pi 3 (aarch64)

Article about raspvisor is [here](https://qiita.com/matsud224/items/7ce824d62152054eec41) (Japanese).

*Warning: This is a hobby project and not for practical use.*

# Setup
First, write a Raspberry Pi OS (previously called Raspbian) image to your SD card to make partitions and install firmwares.
Execute following commands to build. (GNU toolchain for aarch64-linux-gnu is required.)
```
$ export SD_BOOT_DIR=/path/to/bootpartiton/on/sdcard
$ export SD_ROOTFS_DIR=/path/to/rootfspartiton/on/sdcard
$ make install
```

Programs runs on a hypervisor startup are hard-coded in `src/main.c`. I confirmed that programs in `example` directory can be run on this hypervisor. In `example` directory, following programs are found.
* test_binary : issues hypervisor call once.
* echo : Mini-UART echo back (based on [raspberry-pi-os/lesson02](https://github.com/s-matyukevich/raspberry-pi-os/tree/master/src/lesson02))
* mini-os : a simple operating system which has a process scheduler, interrupt handling and virtual memory support (based on [raspberry-pi-os/lesson06](https://github.com/s-matyukevich/raspberry-pi-os/tree/master/src/lesson06))

Enter each directory and `make` to build. Then copy `*.bin` file to `SD_BOOT_DIR`.

# Usage
UART is assigned to the hypervisor's console. Connect your cable to the GPIO 14/15 pins.
* <kbd>?</kbd> + <kbd>l</kbd> : show the list of VMs
* <kbd>?</kbd> + <kbd>1-9</kbd> : switch to the console of VM 1-9

# Features
* Enabling stage 2 translation
* Memory-mapped IO emulation
  * BCM2837 Interrupt Controller
  * BCM2837 System Timer (inacculate)
  * BCM2837 Mini-UART
* IRQ virtualization by using virtual IRQs
* Trapping access of some system register
* Trapping WFI/WFE instruction

# Links
* Armv8-A Virtualization - Learn the Architecture (https://developer.arm.com/architectures/learn-the-architecture/armv8-a-virtualization)
* ARM Architecture Reference Manual ARMv8, for ARMv8-A architecture profile
* 「ハイパーバイザの作り方」(http://syuu1228.github.io/howto_implement_hypervisor/)

# License

This software is licensed under the MIT license.

* Source codes and examples (echo and mini-os) are mostly based on [Sergey Matyukevich's raspberry-pi-os](https://github.com/s-matyukevich/raspberry-pi-os).
* SD card driver (`src/sd.c`, `src/delays.c`, `include/sd.h` and `include/delays.h`) is from [bzt's raspi3-tutorial](https://github.com/bztsrc/raspi3-tutorial).
