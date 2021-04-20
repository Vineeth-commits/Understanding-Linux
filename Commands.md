#Understanding Linux

## A Basic Guide to Linux Boot Process
---
### Power on
* BIOS (Basic Input Output System) is a software program comes pre-built in a motherboard chipset.
* BIOS loads and scans for devices such as Hard Disk, CD-ROM, RAM, etc.
* BIOS searches for MBR (Master Boot Record: 1st sector) of the primary hard drive, it scans for 1st stage loader (In our case boot loader is (GRUB LILO) and hands over the responsibility to MBR.
* Boot PROM/FLASH/BIOS is proficient of loading the MBR into RAM and executing it.
### MBR (Master Boot Record)
* 512 bytes of space –> MBR
* MBR contains the information of loader of most operating system e.g UNIX, Linux and WINDOWS
* MBR holds the small binary information of 1st stage of loader
* MBR consist physical sector of the first disk drive (i.e 512 bytes) and it’s not part of any partition.
* Placed on the prime disk drive, in the prime sector of the first cylinder of track is 0 and head is 0 (this whole path is generally booked for boot programs)
* MBR involve a mini executable programs and a table specify the primary partitions.
* MBR also documents which primary partition is ACTIVE.
* The BIOS surrender rights to the first stage boot loader, which then scans partition table and finds second stage boot loader on the partition configured as bootable.
### Boot Loader
* The boot loader termed from 1st stage loader and loads itself into RAM. All this go on in milliseconds.
* The default stage 2 boot loader is a GRUB (Grand Unified Boot Loader) or LILO (Linux Loader)
* Once GRUB is loaded into RAM, then it searches for the location of Kernel.
* GRUB will scrutinize the map file to find the kernel image, that is located under (/boot) and load it.
* GRUB loads the kernel (vmlinuz-version) from /boot partition
	* **Trivia 1**
		GRUB organize RAMDISK for initrd —> (RAMDISK is reserved space from RAM). In addition, it drives initrd into RAM to ready the kernel for loading itself into memory and depended modules so that it can leave the system to “init” process.
		In, Linux most of the drivers are pre-built as modules, these would be initial ram drive (initrd.img) where it can keep all the information of additional modules. So, when the kernel boots, it creates ramdrive, loads the initrd.img and its depended modules.
		GRUB reads /boot/grub/grub.conf & shows us a clean interface for selecting Operating System
		Once Kernel loads its depended modules and then it hand over to “init” process. The kernel image has a small, unpacked program that un-compresses kernel and runs it.

	* **Trivia 2**
		LILO needed to indicate MBR in order to locate operating systems on the hard drive. Any modifications done to /etc/lilo.conf, that must be updated in MBR, but in GRUB‘s case no need to update, it reads directly from the file /boot/grub/grub.conf.
		After making changes in /etc/lilo.conf, we’ll have to update the MBR manually
	* **Trivia 3**
		The GRUB second stage loader resides within the MBR and within /boot partition. Once GRUB is loaded into memory it becomes 2nd stage loader.

	* **Trivia 4**
		The /initrd directory should not be removed it is a temporary place holder for kernel to have quick access to the modules that it needs to start the system modules include device drivers.

### Kernel initialization highlights include:
* Initialize CPU components, eg, MMU
* Initialize the scheduler (PID 0)
* Mount the root filesystem in rw mode
* Fork off the init process (PID 1)

In essence, kernel initialization does two things:
* Start the core system of shared resource managers (RAM, processor and mass storage).
* Starts a single process, /sbin/init. Init process (sbin/init) is the very fist process which loads all the various daemons and mounts all the partitions which are listed under /etc/fstab.

### About /etc/fstab
* The /sbin/init reads /etc/inittab file
* Set default runlevel ( the telinit command allows administrators to tell the init process to change its current runlevel)
* Calls /etc/rc.d/rc.sysinit and /etc/rc.d/rc x (where ‘x‘ is a runlevel)
* In /etc/rc.d/rc5.d directory files starting with letter K –> kill scripts and files starting with letter S –> Startup scripts.
* Start up the tty processes and xdm ( X display manager)
* Starts User’s login screen

### More about init
* init 0 - shutdown
* init 1 - Single user or emergency mode where no networking no multitasking is present in this mode only root access is present in the run level.
* init 2 - No network but multitasking support is present
* init 3 - Networking and multitasking is present without GUI
* init 4 - It is similar to runlevel 3; It is reserved for other purposes in research.
* init 5 - Network is present multitasking and GUI is present with sound etc.
* init 6 - This runlevel is defined to system restart.
* init s - Tells the init command to enter the maintenance mode. When the system enters maintenance mode from another run level, only the system console is used as the terminal.

## Linux Directory Structure and Important Files Paths Explained
---
### Linux Directory Structure Diagram
The linux follows a reverse tree like structure starting with the root directory(/). It is hierarchical in structure.
Describing briefly the purpose of each directory, we are starting hierarchically.
* /bin : All the executable binary programs (file) required during booting, repairing, files required to run into single-user-mode, and other important, basic commands viz., cat, du, df, tar, rpm, wc, history, etc.
* /boot : Holds important files during boot-up process, including Linux Kernel.
* /dev : Contains device files for all the hardware devices on the machine e.g., cdrom, cpu, etc
* /etc : Contains Application’s configuration files, startup, shutdown, start, stop script for every individual program.
* /home : Home directory of the users. Every time a new user is created, a directory in the name of user is created within home directory which contains other directories like Desktop, Downloads, Documents, etc.
* /lib : The Lib directory contains kernel modules and shared library images required to boot the system and run commands in root file system.
* /lost+found : This Directory is installed during installation of Linux, useful for recovering files which may be broken due to unexpected shut-down.
* /media : Temporary mount directory is created for removable devices viz., media/cdrom.
* /mnt : Temporary mount directory for mounting file system.
* /opt : Optional is abbreviated as opt. Contains third party application software. Viz., Java, etc.
* /proc : A virtual and pseudo file-system which contains information about running process with a particular Process-id aka pid.
* /root : This is the home directory of root user and should never be confused with ‘/‘
* /run : This directory is the only clean solution for early-runtime-dir problem.
* /sbin : Contains binary executable programs, required by System Administrator, for Maintenance. Viz., iptables, fdisk, ifconfig, swapon, reboot, etc.
* /srv : Service is abbreviated as ‘srv‘. This directory contains server specific and service related files.
* /sys : Modern Linux distributions include a /sys directory as a virtual filesystem, which stores and allows modification of the devices connected to the system.
* /tmp :System’s Temporary Directory, Accessible by users and root. Stores temporary files for user and system, till next boot.
* /usr : Contains executable binaries, documentation, source code, libraries for second level program.
* /var : Stands for variable. The contents of this file is expected to grow. This directory contains log, lock, spool, mail and temp files.

### Exploring Important file, their location and their Usability
* /boot/vmlinuz : The Linux Kernel file.
* /dev/hda : Device file for the first IDE HDD (Hard Disk Drive)
* /dev/hdc : Device file for the IDE Cdrom, commonly
* /dev/null : A pseudo device, that don’t exist. Sometime garbage output is redirected to /dev/null, so that it gets lost, forever.
* /etc/bashrc : Contains system defaults and aliases used by bash shell.
* /etc/crontab : A shell script to run specified commands on a predefined time Interval.
* /etc/exports : Information of the file system available on network.
* /etc/fstab : Information of Disk Drive and their mount point.
* /etc/group : Information of Security Group.
* /etc/grub.conf : grub bootloader configuration file.
* /etc/init.d : Service startup Script.
* /etc/lilo.conf : lilo bootloader configuration file.
* /etc/hosts : Information of Ip addresses and corresponding host names.
* /etc/hosts.allow : List of hosts allowed to access services on the local machine.
* /etc/host.deny : List of hosts denied to access services on the local machine.
* /etc/inittab : INIT process and their interaction at various run level.
* /etc/issue : Allows to edit the pre-login message.
* /etc/modules.conf : Configuration files for system modules.
* /etc/motd : motd stands for Message Of The Day, The Message users gets upon login.
* /etc/mtab : Currently mounted blocks information.
* /etc/passwd : Contains password of system users in a shadow file, a security implementation.
* /etc/printcap : Printer Information
* /etc/profile : Bash shell defaults
* /etc/profile.d : Application script, executed after login.
* /etc/rc.d : Information about run level specific script.
* /etc/rc.d/init.d : Run Level Initialisation Script.
* /etc/resolv.conf : Domain Name Servers (DNS) being used by System.
* /etc/securetty : Terminal List, where root login is possible.
* /etc/skel : Script that populates new user home directory.
* /etc/termcap : An ASCII file that defines the behaviour of Terminal, console and printers.
* /etc/X11 : Configuration files of X-window System.
* /usr/bin : Normal user executable commands.
* /usr/bin/X11 : Binaries of X windows System.
* /usr/include : Contains include files used by ‘c‘ program.
* /usr/share : Shared directories of man files, info files, etc.
* /usr/lib : Library files which are required during program compilation.
* /usr/sbin : Commands for Super User, for System Administration.
* /proc/cpuinfo : CPU Information
* /proc/filesystems : File-system Information being used currently.
* /proc/interrupts : Information about the current interrupts being utilised currently.
* /proc/ioports : Contains all the Input/Output addresses used by devices on the server.
* /proc/meminfo : Memory Usages Information.
* /proc/modules : Currently using kernel module.
* /proc/mount : Mounted File-system Information.
* /proc/stat : Detailed Statistics of the current System.
* /proc/swaps : Swap File Information.
* /version : Linux Version Information.
* /var/log/lastlog : log of last boot process.
* /var/log/messages : log of messages produced by syslog daemon at boot.
* /var/log/wtmp : list login time and duration of each user on the system currently.

## ls Command
---
The general syntax for a command
	```bash
	command [options] [location]
	```

### List Files using ls with no option
ls with no option list files and directories in bare format where we won’t be able to view details like file types, size, modified date and time, permission and links etc.

### List Files With option –l
Here, ls -l (-l is character not one) shows file or directory, size, modified date and time, file or folder name and owner of file and its permission.

### View Hidden Files
List all files including hidden file starting with ‘.‘.

### List Files with Human Readable Format with option -lh
With combination of -lh option, shows sizes in human readable format.

## List Files and Directories with ‘/’ Character at the end
Using -F option with ls command, will add the ‘/’ Character at the end each directory.

## List Files in Reverse Order
The following command with ls -r option display files and directories in reverse order.

## Recursively list Sub-Directories
ls -R option will list very long listing directory trees. See an example of output of the command.

##	Reverse Output Order
With combination of -ltr will shows latest modification file or directory date as last.

## Sort Files by File Size
With combination of -lS displays file size in order, will display big in size first

## Display Inode number of File or Directory()
We can see some number printed before file / directory name. With -i options list file / directory with inode number. An inode is an entry in inode table, containing information (the metadata) about a regular file and directory. An inode is a data structure on a traditional Unix-style file system such as ext3 or ext4.
## Display UID and GID of Files
To display UID(User Identifier) and GID(Group Identifier) of files and directories. use option -n with ls command. A UID (user identifier) is a number assigned by Linux to each user on the system. This number is used to identify the user to the system and to determine which system resources the user can access. UIDs are stored in the /etc/passwd file.Groups in Linux are defined by GIDs (group IDs). Just like with UIDs, the first 100 GIDs are usually reserved for system use. The GID of 0 corresponds to the root group and the GID of 100 usually represents the users group. GIDs are stored in the /etc/groups file.

## cd Command
---
* Switch back to previous directory where you working earlier.
	```bash
	>cd -
	```
* Change Current directory to parent directory.
	```bash
	>cd ..
	```
	Note: Every directory has . and .. hidden folder. The . is for the current directory and .. is for the parent directory.
* Show last working directory from where we moved (use ‘–‘ switch) as shown.
	```bash
	cd --
	```
* Move two directory up from where you are now.
	```bash
	cd ../../
	```
* Move to users home directory from anywhere.
	```bash
	cd ~
	```
	Note: Tilde is a shortcut to your home directory
* Change working directory to current working directory (seems no use of in General).
	```bash
	cd .
	```
* Navigate from your current working directory to /etc/v__ _, Oops! You forgot the name of directory and not supposed to use TAB.
	```bash
	cd /etc/v*
	```
	This will move to ‘vbox‘ only if there is only one directory starting with ‘v‘. If more than one directory starting with ‘v‘ exist, and no more criteria is provided in command line, it will move to the first directory starting with ‘v‘, alphabetically as their presence in standard dictionary.
* pushd and popd
	The pushd command saves the current location to memory and changes to the requested directory.
	```bash
	pushd ~/Desktop
	```
	As soon as popd is fired, it fetch the saved directory location from memory and makes it current working directory.
	```bash
	popd
	```
* Change to a directory containing white spaces. While dealing with white spaces we can use quotes(double or single) or back slash
	```bash
	cd hello\ world\
	```
* Change from current working directory to Downloads and list all its settings in one go.
	```bash
	cd ~/Downloads && ls
	```
* Absolute and relative path
	The absolute path specify a location in relation to the root directory.You can identify them easily as they always begin with a forward slash.
	A relative path specify a location in relation to where we currently are in the system. They dont begin with a slash.
* Move to the root directory
	```bash
	cd /
	```
	Note: The slash represents the root directory


##	dir Command
---
Lists directory contents
* View all files in a directory including hidden files. To list all files in a directory including . (hidden) files, use the -a option. You can include the -l option to format output as a list.
	```bash
	dir -al
	```
* View directory entries instead of content. When you need to list only directory entries instead of directory content, you can use the -d option.
	```bash
	dir -d /
	```
* View index number of files. In case you want to view the index number of each file, use the option -i.
	```bash
	dir -i
	```
* List files and their allocated sizes in blocks. You can view files sizes using the -s option. If you need to sort the files according to size, then use the -S option. In this case you need to also use the -h option to view the files sizes in a human-readable format
	```bash
	dir -shlS
	```
* List files without owner or group owner. To list files without their owners, you have to use -g option which works like the -l option only that it does not print out the file owner.
* List directories before other files. You may wish to view directories before all other files and this can be done by using the –group-directories-first flag. You can also view subdirectories recursively, meaning that you can list all other subdirectories in a directory using the -R


## pwd (Print Working Directory) Command
---
command ‘pwd‘ tells where you are – in which directory, starting from the root (/).
* creating symbolic links using 'ln' command. A symbolic link, also known as a symlink or a soft link, is a special type of file that simply points to another file or directory.
	```bash
	ln -s [target_file] [link_name]
	ln -s /etc/bashrc/script.py script
	```
	The -s option determines that the link is soft link. If not mentioned then it would create a hard link.
	* A hard link always points a filename to data on a storage device
	* A soft link always points a filename to another filename, which then points to information on a storage device
* Print working directory from environment even if it contains symlinks
	```bash
	pwd -L
	```
* Print actual physical current working directory by resolving all symbolic links
	```bash
	pwd -P
	```
	Tip: tty command gives the pwd of the terminal.

## Touch Command
---
The touch command is a standard program for Unix/Linux operating systems, that is used to create, change and modify timestamps of a file.
* How to Create an Empty File
	```bash
	touch [empty_file]
	```
	The following touch command creates an empty (zero byte) new file.
* How to Change File Access and Modification Time
To change or update the last access and modification times of a file use the -a option as follows. The following command sets the current time and date on a file.
	```bash
	touch -a [empty_file]
	```
* How to Avoid Creating New File
Using -c option with touch command avoids creating new files.
	```bash
	touch -c [empty_file]
	```
* How to Change File Modification Time
If you like to change the only modification time of a file called leena, then use the -m option with touch command. Please note it will only updates the last modification times (not the access times) of the file.
	```bash
	touch -m [empty_file]
	```
* Explicitly Set the Access and Modification times
You can explicitly set the time using -c and -t option with touch command. The format would be as follows.
	```bash
	touch -c -t [YYMMHHMM] [file_name]
	touch -c -t [20110015] [empty_file] [11 nov 2020 00:15]
	```
* Create a File using a specified time
If you would like to create a file with specified time other than the current time, then the format should be
	```bash
	touch -t [YYMMHHMM.SS] [file_name]
	touch -t [20110015.45] [empty_file]
	```

## cp, mv, rm, mkdir and rmdir Command
---
* The copy command makes a duplicate copy of the file or directory.
	```bash
	cp [options] <source> <destination>
	```
	Using the -r option, which stands for recursive, we may copy directories. Recursive means that we want to look at a directory and all files and directories within it, and for subdirectories, go into them and do the same thing and keep doing this.
* To move a file we use the command mv which is short for move. It operates in a similar way to cp. One slight advantage is that we can move directories without having to provide the -r option.
	```bash
	mv [options] <source> <destination>
	```
	moving and renaming can be done using mv command only
	```bash
	mv ~/Desktop/project ~/Desktop/new_project
	```
* The command to remove or delete a file is rm which stands for remove.
	```bash
	rm [options] <file>
	```
	When rm is run with the -r option it allows us to remove directories and all files and directories contained within. A good option to use in combination with r is i which stands for interactive.
	```bash
	rm -ri project
	```
	Linux has no undo feature so the destructive operations must be done with precaution
* The command mkdir creates an empty directory
The -p options tells mkdir to make parent directories as needed and the -v option makes mkdir tell us what it is doing.
* The command rmdir removes empty directories
rmdir has similar options as mkdir i.e -p, -v but the directories within should also be empty

## Piping and redirectional
---
### Introduction
Every program we run on the command line automatically has three data streams connected to it.
* STDIN (0) - Standard input (data fed into the program)
* STDOUT (1) - Standard output (data printed by the program, defaults to the terminal)
* STDERR (2) - Standard error (for error messages, also defaults to the terminal)

Piping and redirection is the means by which we may connect these streams between programs and files to direct data in interesting and useful ways.
### Redirecting to a File
The greater than operator ( > ) indicates to the command line that we wish the programs output (or whatever it sends to STDOUT) to be saved in a file instead of printed to the screen.
	```bash
	echo hello\ world\ > output.txt
	```
Note: When piping and redirecting, the actual data will always be the same, but the formatting of that data may be slightly different to what is normally printed to the screen.
### Saving to an Existing File
If the output is directed to an existing file with data in it, the existing data will be replaced with the new data. To avoid this we use double greater than operator ( >> ).
	```bash
	echo hello\ world\ >> output.txt
	```
### Redirecting from a File
If we use the less than operator ( < ) then we can send data the other way. We will read data from the file and feed it into the program via it's STDIN stream.
	```bash
	cat < input.txt > output.txt
	```
### Redirecting STDERR
To redirect an error message to a file we use 2> operator.
	```bash
	ls -l image.png 2> error.txt
	```
### Transfer of data
Though piping we can send data from one program to another program and the operator we use is ( | ).
	```bash
	ls -l | head -3
	```
### Some examples of redirectional
Appending the file or avoiding trucating the file
	```bash
	cat <input.txt >output.txt 2>error.txt
	car <input.txt >>output.txt 2>>error.txt
	```
### Some examples of piping
tee command is used to create a snapshot of the output without breaking the chain. xargs converts data coming in, into command line arguments
```bash
date | tee fulldate.txt | cut -d " " -f 1 | tee date.txt | xargs echo hello
```
If two or more fields then we separate them with a comma as shown
```bash
date | tee fulldate.txt | cut -d " " -f 1,2 | tee date.txt | xargs echo hello
```
redirection breaks the pipelines, so use tee to save the output

## cat and tac commands
---
### Basic Usage of Cat Command in Linux
Cat command, acronym for Concatenate, the most basic usage of the command is to read files and display them to stdout, meaning to display the content of files on your terminal. This command can be used to read multiple files also.
	```bash
	cat file1.txt file2.txt file3.txt
	```
The command can be used to redirect the output also
	```bash
	cat file.txt >> output.txt
	```
To input the number of lines in the output file use -n option
	```bash
	cat -n file.txt
	```
To input the number of non empty lines use the -b option
	```bash
	cat -b file.txt
	```
If the number of contents exceed the the output terminal then use less or more command(use less preferably)
	```bash
	cat log.txt | less
	```
### Tac Command
Tac is practically the reverse version of cat command (also spelled backwards) which prints each line of a file starting from the bottom line and finishing on the top line to your machine standard output. One of the most important option of the command is represented by the -s switch, which separates the contents of the file based on a string or a keyword from the file.
	```bash
	tac -s file.txt
	```
Most important usage of tac command is, that it can provide a great help in order to debug log files, reversing the chronological order of log contents.
	```bash
	tail log.txt | tac
	```

## Wildcards
---
\* - represents zero or more characters
? - represents a single character
[] - represents a range of characters

Some examples
* lists out files having one letter behind 'i' and the one or more characters after that. The extension can be jpg or png.
	```bash
	ls ?i*.??g
	```
* The range operator allows you to limit to a subset of characters. In this example we are looking for every file whose name either begins with a, s or v
	```bash
	ls [sv]*
	```
* We may also reverse a range using the caret ( ^ ) which means look for any character which is not one of the following.
	```bash
	ls [^a-k]* 
	```

## Creating permanent aliases
---
* Edit ~/.bash_aliases or ~/.bashrc file using: vi ~/.bash_aliases
* Append your bash alias
	For example append: alias update='sudo yum update'
* Save and close the file.
* Activate alias by typing: source ~/.bash_aliases
* Please note that ~/.bash_aliases file only works if the following line presents in the ~/.bashrc file:
	```bash
	if [ -f ~/.bash_aliases ]; then
		. ~/.bash_aliases
	fi
	```
	Syntax:
	```bash
	alias alias_name="command -options"
	```

## Filters
A filter, in the context of the Linux command line, is a program that accepts textual data and then transforms it in a particular way.
### head command
Head is a program that prints the first so many lines of it's input. By default it will print the first 10 lines but we may modify this with a command line argumen
	```bash
	head [-number_of_lines_to_print] [path]
	```
### tail command
Tail is the opposite of head. Tail is a program that prints the last so many lines of it's input. By default it will print the last 10 lines but we may modify this with a command line argument.
	```bash
	tail [-number_of_lines_to_print] [path]
	```
### sort command
Sort will sort it's input, nice and simple. By default it will sort alphabetically but there are many options available to modify the sorting mechanism.
	```bash
	sort [-options] [path]
	```

Some options to sort command
| Flag | Purpose|
|-------|--------|
|-r	|reverse the sorting order|
|-n	|sort in a numerical order|
|-u	|returns unique entires|
|-k	|sort tabular data by providing a KEYDEF as an argument|

sorts the data based on the 5th column.
	```bash
	ls -l | sort -k 5n
	```
### nl command
nl stands for number of lines and it just displays the number of lines as mentioned.
It has different options to view the content.
The -s option specifies what should be printed after the number and -w option specifies the amount of padding before the number.
```bash
nl -s "." -w 10 sampletext.txt 
```
### wc command
wc stands for word count and it also displays the number of characters and lines. By default it will give a count of all 3 but using command line options we may limit it to just what we are after. The order is lines, words and characters. Sometimes you just want one of these values. -l will give us lines only, -w will give us words and -m will give us characters.

### cut command
cut is a nice little program to use if your content is separated into fields (columns) and you only want certain fields.
```bash
cut -f 1,2 -d " " sampletext.txt
```
cut defaults to using the TAB character as a separator to identify fields. In our file we have used a single space instead so we need to tell cut to use that instead. The separator character may be anything you like, for instance in a CSV file the separator is typically a comma ( , ). This is what the -d option does (we include the space within single quotes so it knows this is part of the argument). The -f option allows us to specify which field or fields we would like. If we wanted 2 or more fields then we separate them with a comma as below.
### sed command
sed stands for Stream Editor and it effectively allows us to do a search and replace on our data.
```bash
sed <expression> [path]
```
expression = 's/search/replace/g'

The initial s stands for substitute and specifies the action to perform. Then between the first and second slashes ( / ) we place what it is we are searching for. Then between the second and third slashes, what it is we wish to replace it with. The g at the end stands for global and is optional. If we omit it then it will only replace the first instance of search on each line. With the g option we will replace every instance of search that is on each line.
### uniq command
uniq stands for unique and it's job is to remove duplicate lines from the data. One limitation however is that those lines must be adjacent ie one after the other.
note:If a program behaves strangely then it be stopped by pressing ctrl+c to cancel the program and get back to the prompt.

## history command
---
Shows commands previously entered.
Enters the last command executed
```bash
!!
```
Run the command that is on the line 40 of the output from the history command
```bash
!40
```
Will clear the history of commands
```bash
history -c; history -w
```
## To search for a file or the file contents
---
### Find command
The find command can be used for more sophisticated search tasks than the locate command.
Will list all files and directories below the current working directory (which is denoted by the .)
```bash
find .
```
Will list out all the directories in the file system
```bash
find /
```

Certain options in find command
| Flag | Purpose|
|-------|-------|
|-maxdepth|	limiting the search depth|
|-maxdepth 4| Will list everything on the file system below the base directory, provided that it is within 4 levels of the base directory|
|-type| only lists items of certain type. f restricts the search to file and d for directories|
|-name ".txt"| search for terms matching a certain name|
|-iname| searches in a case insensitive manner|
|-size|	Finds the files based on the size like -size +100k searches for files greater than 100kiB and -size -1M finds files with less than 100Mib|
### examples of find command
Will copy every item below the /etc folder on the file system to the ~/Desktop directory.Commands are executed on each item using the –exec option. The argument to the –exec option is the command you want to execute on each item found by the find command. Commands should be written as they would normally, with {} used as a placeholder for the results of the find command. Be sure to terminate the –exec option using \; (a backslash then a semicolon).
```bash
find /etc –exec cp {} ~/Desktop \;
```
The –ok option can also be used, to prompt the user for permission before each action.
```bash
find /etc –ok cp {} ~/Desktop \;
```
Lists the number of file between the size 100kiB and 1Mib in the file system
```bash
find / -type f -size +100k -size -1M | wl -l
```
Lists the number of file less than 100kiB and more than 1Mib in the file system
```bash
find / -type f -size +100k -o -size +1M | wl -l
```
### locate command
The locate command searches a database on your file system for the files that match the text (or regular expression) that you provide it as a command line argument. If results are found, the locate command will return the absolute path to all matching files. The locate command is fast, but because it relies on a database it can be error prone if the database isn’t kept up to date.

Certain options in locate command
| Flag| Purpose|
|------|-------| 
|-S	| Print info about the database file|
|--existing| Checks whether a result actually exists before returning|
|-limit| limits the output|

Used to update the database.
```bash
sudo updatedb
```
### grep command
The grep command will return all lines that match the particular piece of text (or regular expression) provided as a search term.

Some options in grep
| Flag| Purpose|
|------|-------| 
|-i	|search in a case insensitive manner|
|-v	|invert the search i.e returns all the lines that dont contain the search term|
|-c	|returns the number of lines that match a search term|
### which command
To check if a valid program is present in the shell's path.
```bash
which commandName
```

## File Archiving and Compression
---
### Creating a tarball
tarballs are created using the tar command
```bash
tar -cvf <name of the tarball> <file>
```
Breakdown of the command
| Flag| Purpose|
|------|-------| 
|-c | This allows to create the tarball [required]|
|-v	|verbose, feedback of its progress [optional]|
|-f |tells tar that the next argument is the name of the tarball.[required]|
| \<name of the tarball\>|The absolute or relative file path to where you want the tarball to be placed|
|\<file\>|The absolute or relative file paths to files that you want to insert into the tarball|

### Checking a tarball's contents
```bash
tar -tf <name of the tarball>
```
The –t option: “test-label”. This allows us to check the contents of a tarball. [required]
The –f option: Tells tar that the next argument is the name of the tarball. [required]
<name of tarball>: The absolute or relative file path to where you want the tarball to be placed

### Extracting from the tarball
```bash
tar -xvf <name of the tarball>
```
| Flag| Purpose|
|------|-------| 
|–x |“extract”. This allows us to extract a tarball’s contents. [required]|
|–v |“verbose”. This makes tar give us feedback on its progress. [optional]|
|–f |Tells tar that the next argument is the name of the tarball.  [required]|
|\<name of tarball\>|The absolute or relative file path to where the tarball is located|
Note: Extracting a tarball does not empty the tarball. You can extract from a tarball as many times as you want without affecting the tarball’s contents.

### Compressing tarball
Tarballs can be done with two compression algorithms - gzip and bzip2 (there are other algorithms)
The gzip compression algorithm tends to be faster than bzip2 but, as a trade-off, gzip usually offers less compression.
#### Compressing and Decompressing with gzip
Compressing with gzip
```bash
gzip <name of the tarball>
```
When compressing with gzip, the file extension .gz is automatically added to the .tar archive. Therefore, the gzip compressed tar archive would, by convention, have the file extension .tar.gz

Decompressing with gzip
```bash
gunzip <name of the tarball>
```
#### Compressing and Decompressing with bzip2
Compressing with bzip2
```bash
bzip2 <name of the tarball> 
```
When compressing with bzip2, the file extension .bz2 is automatically added to the .tar archive. Therefore, the bzip2 compressed tar archive would, by convention, have the file extension .tar.bz2

Decompressing with bzip2
```bash
>bunzip <name fo the tarball>
```
[For more info](https://www.rootusers.com/gzip-vs-bzip2-vs-xz-performance-comparison/)
	
### Creating a tarball and compressing it
For gzip, we use -z option
```bash
tar –cvzf <name of tarball> <file> 
```
For bzip2, we use -j option
```bash
tar -cvjf <name of tarball> <file> 
```
For xzip, we use -J option
```bash
tar –cvJf <name of tarball> <file> 
```
### Decompressing a tarball and extracting it
```bash
tar –xvzf <name of tarball> #For gzip
tar –xvjf <name of tarball> #For bzip2
tar –xvJf <name of tarball> #For xzip
```
### Creating .zip files
Although .tar.gz and .tar.bz2 archives are the archives of choice on Linux, .zip archives are common on other operating systems such as Windows and Mac OSX.

Creating a .zip archive
```bash
zip <name of zipfile> <file> 
```

Extracting a .zip archive
```bash
unzip <name of zipfile>
```

## Looking up in the man page
---
Search the manual for pages matching <search term>
```bash
man –k <search term>
```
Open the man page called <page name> in section 5 of the manual
```bash
man 5 <page name> 
```
Open the man page called <page name> in section 1 of the manual
```bash
>man <page name>
```
There are different sections in the manual page
* Section 1 (User Commands) - Commands that can be run from the shell by a normal user (typically no administrative privileges are needed)
* Section 2 (System Calls) - Programming functions used to make calls to the Linux kernel
* Section 3 (C Library Functions) - Programming functions that provide interfaces to specific programming libraries.
* Section 4 (Devices and Special Files) - File system nodes that represent hardware devices or software devices.
* Section 5 (File Formats and Conventions) - The structure and format of file types or specific configuration files.
* Section 6 (Games) - Games available on the system
* Section 7 (Miscellaneous) - Overviews of miscellaneous topics such as protocols, filesystems and so on.
* Section 8 (System administration tools and Daemons) - Commands that require root or other administrative privileges to use

## Crontab - Scheduled automation
---
Used to run scripts or just a command (could also be an alias) at a schedule time.

To select a editor
```bash
crontab -e
```
Check out [crontab guru](https://crontab.guru/)

## Introduction to vi editor
---
Vi is a command line text editor. There are two modes in Vi. Insert (or Input) mode and Edit mode. In input mode you may input or enter content into the file. In edit mode you can move around the file, perform actions such as deleting, copying, search and replace, saving etc. A common mistake is to start entering commands without first going back into edit mode or to start typing input without first going into insert mode. If you do either of these it is generally easy to recover so don't worry too much.
```bash
vi <file name>
```
You always start off in edit mode so the first thing we are going to do is switch to insert mode by pressing i.
### Saving and exiting
|Command(Note the capitals)| Functionality|
|--------|-------------|
|:ZZ | Save and exit|
|:q!| discard all changes, since the last save, and exit|
|:w|save file but don't exit|
|:wq|again, save and exit|

Note: If you are unsure if you are in edit mode or not you can look at the bottom left corner. As long as it doesn't say INSERT you are fine. Alternatively you can just press Esc to be sure. If you are already in edit mode, pressing Esc does nothing so you won't do any harm.
### Navigating a file in vi
* Arrow keys - move the cursor around
* j, k, h, l - move the cursor down, up, left and right (similar to the arrow keys)
* ^ (caret) - move cursor to beginning of current line
* $ - move cursor to end of the current line
* nG - move to the nth line (eg 5G moves to 5th line)
* G - move to the last line
* w - move to the beginning of the next word
* nw - move forward n word (eg 2w moves two words forwards)
* b - move to the beginning of the previous word
* nb - move back n word
* { - move backward one paragraph
* } - move forward one paragraph

Note: If you type :set nu in edit mode within vi it will enable line numbers. I find that turning line numbers on makes working with files a lot easier.
### Deleting content
* x - delete a single character
* nx - delete n characters (eg 5x deletes five characters)
* dd - delete the current line
* dn - d followed by a movement command. Delete to where the movement command would have taken you. (eg d5w means delete 5 words)
### undoing
* u - Undo the last action (you may keep pressing u to keep undoing)
* U (Note: capital) - Undo all changes to the current line

## Process Management
---
### To check the current processes
Top will give you a realtime view of the system and only show the number of processes which will fit on the screen.
```bash
top
```
Another program to look at processes is called ps which stands for processes. In it's normal usage it will show you just the processes running in your current terminal (which is usually not very much). If we add the argument aux then it will show a complete system view which is a bit more helpful.
```bash
ps [aux]
```
### Killing a process
To kill a process use ps and grep the name of the process
```bash
ps aux | grep <name of the process to kill>
```
This will return a PID(Process ID) which is next to the owner of the process
```bash
kill[signal] <PID>
```
This kill sends the default signal ( 1 ) to the process which effectively asks the process nicely to quit. We always try this option first as a clean quit is the best option. Sometimes this does not work however. We can run kill again but this time supply a signal of 9 which effectively means, go in with a sledge hammer and make sure the process is well and truly gone.
```bash
kill -9 <PID>
```
### Troubleshooting a locked Desktop
Linux actually runs several virtual consoles. Most of the time we only see console 7 which is the GUI but we can easily get to the others.To switch between consoles you use the keyboard sequence CTRL + ALT + F<Console>. So CTRL + ALT F2 will get you to a console (if all goes well) where you can run the commands as above to identify process ids and kill them. Then CTRL + ALT F7 will get you back to the GUI to see if it has been fixed.If nothing working restart the system.
### Foreground and Background Jobs
A program called jobs lists currently running background jobs for us.
```bash
jobs
```
To run a process in the background we use ampersand ( & ) at the end of the command.
For demonstration we use the command sleep which waits for the entered number of seconds and quits
```bash
sleep 50
```
This process runs in the foreground. But if you want to carry out of other work it can be run in the background by putting a &
```bash
sleep 50 &
```
To bring it in the foreground we run fg
```bash
fg <job number>
```
If you press CTRL + z then the currently running foreground process will be paused and moved into the background.
We can also use the bg to bring it to the background
```bash
bg <job number>
```

## Tree command
---
### Purpose
To see linux directories in a tree structure (installation required)
### Options
| Flag| Purpose|
|------|-------| 
|-a| To list all files. Tree doesnt present the file system constructs like '.'(current directory) and '..'(previous directory)|
|-d| To list directories only|
|-C| To colorise the output|
|-f| To list full path of the directories|
|-L [level]| Descend only level directories deep|
|-x | Stay on current filesystem only|
|-P [pattern]|List only those files that match the pattern given|
|-I [pattern]|Do not list files that match the given pattern|
|--ignore-case|Ignore case when pattern matching|