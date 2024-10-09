**Booting** is essentially the process of starting a computer. The boot process is responsible for several things:
- finding, running, and loading the OS kernel
- finding, running, and loading the bootstrapping code
- running startup scripts and system daemons
- maintaining process hygiene

#### Boot process
Most Linux distributions use a manager system daemon called **systemd** instead of the traditional UNIX **init**. It streamlines the boot process by adding more features.

Admins have little direct control over how a computer boots itself, but they can modify config files for startup scripts and change the arguments passed to the kernel by the boot loader.

Before the system if fully booted, file systems have to be mounted and system daemons started. These are typically done using startup shell scripts run by init or parsed by systemd. These startup scripts are normally found in the `/etc/init.d` directory.

![[boot-process.png]]

#### System Firmware (BIOS / UEFI)
On startup, the CPU is wired to execute boot code stored in ROM (either BIOS or UEFI). During normal bootstrapping, the firmware probes for hardware devices and then looks for the next stage of bootstrapping code (GRUB).

#### Boot Loader
The boot loader's main job is to identify and load an appropriate system kernel

**GRUB**
The GRand Unified Boot Loader (GRUB) is the default boot loader on most Linux distributions.




