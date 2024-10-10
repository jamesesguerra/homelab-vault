systemd aims to standardize the configuration and control of system services in Linux. It's not actually a single daemon, but a collection of programs, daemons, libaries, and kernel components. Think of it as a buffet at which you're forced to eat everything.

#### Units and unit files
Anything managed by systemd is called a **unit**. Units can be anything from a service, a socket, a device, etc. The behavior of each unit is defined and configured in a **unit file**. In the case of a service, the unit file specifies the location of the executable file of the service, tells systemd how to start and stop the service, and identifies other units that the service depends on. These unit files are stored either in `/usr/lib/systemd/system` or `/lib/systemd/system`. On the other hand, local or customized unit files should go in `/etc/systemd/system`. The file suffixes typically reflect what kind of unit is being configured.

#### systemctl
**systemctl** is an all-purpose command for investigating the status of systemd and making changes to its configuration.

It's first argument is a subcommand that sets the general agenda. Running it without any arguments runs the default `list-units` subcommand, which lists all loaded and active services, sockets, targets, and devices.

To show only loaded and active services, use the `type=service` qualifer.
```sh
systemctl list-units --type=service
```

It's also helpful to show all unit files, regardless of whether or not they're active.
```sh
systemctl list-unit-files --type=service
```

![[systemctl-commands.png]]w