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


#### Daemons and Processes
Once the kernel is loaded and has been initalized, it spawns a set of *spontaneous* processes, which are a part of the kernel. These are recognized with their low PIDs and with brackets around their name, such as `[kthreadd]`. These processes aren't configurable and aren't mapped to any programs in the filesystem.

The exception to this pattern is the system management daemon. It has a PID of 1 and usually runs under the name **init**.

##### init
init has multiple functions, but its overarching goal is to ensure that the system runs the right set of services and daemons at any given time until the system shuts down. It normally takes care of many different startup chores such as:
- setting the name of the computer
- setting the time zone
- mounting file systems
- configuring network interfaces
- starting up other daemons and network services

init also has a couple of implementations:
**sysvinit**
	- the traditional init predominantly used in Linux systems before the introduction of systemd
	- isn't powerful enough to handle the needs of a modern system
**BSD init** - derives from BSD UNIX
**systemd** - the most recent; a one-stop-shop for all daemon and state-related issues
