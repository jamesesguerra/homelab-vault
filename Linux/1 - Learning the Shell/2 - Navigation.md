
Unlike Windows that has separate file system trees for each storage device, Unix-like systems always have a single file system tree, regardless of the number of storage devices. Storage devices are mounted at various points on the tree according to a system administrator.

#### Commands
- `pwd` - prints the current working directory

When you first open a terminal, the current working directory will be the home directory of the user currently logged in, which is the only place the user is allowed to write files as a regular user.

- `cd` - changes into a new directory using either a relative / absolute path name
	- `cd -` - change into previous working directory
	- `cd ~<user>` - change into the home directory of a user

Unlike other operating systems, Linux has no concept of file extensions. Files can be named any way you like, and the contens and/or purpose of a file is determined by other means.

- `ls` - lists the contents of a directory
	- `ls <dir1> <dir2>` - list the contents of multiple directories
	- `ls -l` / `ll` - show more detail
	- `ls -a` / `la` - show all files including hidden ones
	- `ls -S` - sort results by file size
	- `ls -t` - sort results by modification time
	- `ls -r` - reverse the results
	- `ls -lh` - prints file sizes with human readable units 

- `file` - determine a file's type using a number of different factors

- `less` - page through the contents of a text file
	- `G` - move to the end of the text file
	- `g` - move to the start of the text file
	- `/` - search for a word
	- `n` - move to the next occurrence of the searched word

#### Notable directories
 - `/bin` - binaries (programs) that must be present for the system to boot and run
 - `/boot` - the Linux kernel, initial RAM disk image, and boot loader
 - `/etc` - a collection of text files of system-wide configuration files, including shell scripts that start system services at boot time
 - `/lib` - shared library files used by core system programs (similar to DLLs in Windows)
 - `/media` - mounts points for removable media such as USB drives
 - `/sbin` - system binaries that do vital tasks
 - `/tmp` - temporary, transient files created by various programs
 - `/usr` - all the programs and support files used by regular users
	 - `/usr/bin` - executable programs included with the Linux distribution
	 - `/usr/local` - executable programs not included with the distribution
- `/var` - data that's likely to change
	- `/var/log` - important log files that records various system activity

#### Symbolic links
