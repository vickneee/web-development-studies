# Introduction to Linux Bash

Bash, short for "Bourne Again Shell," is a command-line interface (CLI) program for Unix and Unix-like operating systems, including Linux. It provides users with a powerful and flexible way to interact with their computer systems through text-based commands.

## Usage:

To use Bash, open a terminal or command prompt on your Linux or Unix system and start typing commands. 

## Example Commands:

- `ls` - List directory contents.
- `cd` - Change directory.
- `mkdir` - Create a new directory.
- `rm` - Remove files or directories.
- `grep`- Search for patterns in text.
- `chmod`- Change file permissions.
- `cat`- Concatenate and display file contents.

## Navigation:

### Searching Files and Directories: 
- `firstletter` + `[Tab]` – search for files and directories
- `ls` - list directory contents
- `ls /` - list contents of root directory
- `ls ~` – list contents of home directory
- `ls Documents/` - list contents of Documents directory
- `ls ..` - one level up in the directory tree and list contents
- `ls ../..` - two levels up in the directory tree and list contents
- `ls ./dir` - directory from current working directory
- `ls -r` - reverse order while sorting
- `ls -R` - list all subdirectories recursively
- `ls -l` - long listing
- `ls -a` - list all (including hidden files, e.g., .bashrc)
- `ls -la` - long listing of all files
- `ls -lS` – listing sorted by file size
- `ls -lt` – listing sorted by time
- `ls -la Documents/*.sh` – search for specific file types in all directories
- `ls -la > lsout.txt` - create long listing of all files into a text file
- `ls -d */` - list all directories
- `ls -la | column -t` – display information in columns
- `ls -G` - list directories and files with colors (MacBook)
- `tree` - list contents of a directory in a tree-like format
- `tree -L 2` – list contents of a directory up to level 2

#### pwd
- `pwd` - print working directory (short path)
- `pwd -P` - print working directory (full path)

#### cd
- `cd` - change directory
- `cd -` - go back to the previous directory
- `cd ~` - change directory to home directory
- `cd /dir` - change directory and go to the specified directory
- `cd ..` - move one level up in the directory tree
- `cd ../dir` – move to a directory one level up and then to the specified directory

#### pushd/popd
- `pushd/popd` - manipulate the directory stack

#### file
- `file file.txt` - determine the file type

#### find
- `find *` - list all directories and files
- `find -type d` - list all directories
- `find -type f` - list all files

#### locate
- `locate file.txt` - search for files by name
- `updatedb` - update the locate database
- `sudo updatedb` – update the locate database as root user

#### which
- `which grep` - search for a command (commands located in /usr/bin/ directory)

#### history
- `history` - display bash command history
- `!(number)` – execute a history command by number
- ⬆︎ ⬇︎ - arrow keys to navigate through bash command history
- `history | less` - view less history and use ⬆︎ ⬇︎ to move up and down
- `HISTTIMEFORMAT='%F %T '` – display date and time in history

## Utilities:

#### date
- `date` – display current date and time

#### cal
- `cal` – display calendar of the current month

#### uptime
- `uptime` – display current uptime

#### w
- `w` – display who is logged in

### whoami
- `whoami` – display the current username

#### finger
- `finger user` - display information about a user

#### uname
- `uname -a` – display kernel information

#### cat /proc/cpuinfo
- `cat /proc/cpuinfo` – display processor information

#### cat /proc/meminfo
- `cat /proc/meminfo` - display memory information

#### df
- `df` – display disk space usage

#### du
- `du` – display directory space usage

#### free
- `free` – display memory and swap usage

#### whereis
- `whereis app` – display possible locations of an application

#### whatis
- `whatis` - display manual page descriptions

#### apropos
- `whatis apropos` – display manual page descriptions

#### man
- `man` – display the manual page of a command

## Linux Package Management Commands:

- `dpkg -l` - list installed packages
- `dpkg -L <package>` - show files installed by a package
- `dpkg -S </path/file>` - find which package a file belongs to
- `apt-get install` = `apt install`
- `apt-get update` = `apt update`

## Working with Files:

- `mkdir` - create directory/directories
- `mkdir important` – create a new directory with name important
- `mkdir -p aa/ab/ac/ad/ae/af` – creates all subdir at once 
- `mkdir -p aa/ab/ac` - creates and adds ac subdir
- `touch` - change file timestamps
- `touch newfile.txt` - create empty file
- `cp` - copy files and directories
- `cp file1 file2` - copy file1 to file2
- `cp test.txt Downloads` - copy file to the Downloads directory
- `rename file1 file2 ` - rename a file
- `mv` - move (rename) files
- `mv test.txt Dowloads` – moves file to the Downloads directory 
- `mv file1 file2` - rename or move file1 to file2
- `rm` - remove files or directories
- `rmdir` - remove empty directories
- `rm -f` – forced file remove
- `rm -iv` - before removing confirm it by asking 
- `rm -d` - remove empty directory
- `rm -r` - remove directories and their contents recursively 
- `rmdir aa/ab/ac/ad/ae/af` – remove last subdir 
- `rmdir -p aa/ab/ac/ad/ae/af` – remove all empty subdir
- `more/less` - filter for crt viewing of files
- `more file1.txt` - show more
- `less file1.txt` - show less
- `head` - display first 10 lines of a file
- `tail` - display last 10 lines of a file
- `head file1.txt` - show the first 10 lines of file 
- `head -5 file.txt` - show the first 5 lines of files 
- `tail file1.txt` – show the last 10 lines of files 
- `tail -5 file.txt` - show the last 5 lines of files 
- `head -25 faces.txt | tail -1` - print the 25th line of the file
- `wget` - download file
- `wget -c file` – continue a stopped download
- `compress` – compress files
- `uncompress` – help decompress files
- `zip` – compress files
- `unzip` – help decompress files
- `gzip file.txt` – compress file and rename to file.gz
- `gunzip file.gz` – help decompress gzip files
- `gzip -d file.gz` – decompress file.gz back to a file
- `cmp file1 file2` – compare two files byte by byte
- `diff file1 file2` - compare files line by line

#### Modifying File Permissions:
- `chmod` - change file permissions
- `chmod +x file.sh` – add permissions to the file 
- `chmod +755 file.sh` – add/remove permissions to/from the file

#### nano
- `nano` - command-line text editor for USERS
- `nano file.txt` - create a new file or open an existing file

#### mousepad
- `mousepad` - simple text editor for Xfce
- `mousepad newfile.txt` - create a new file or open an existing file

#### cat
- `cat` - concatenate and display the content of files
- `cat filename` - display the content of the specified file
- `cat file1 file2` - display contents of file1 and file2
- `tac filename` - display the content of a file in reverse order
- `cat -n filename` - display content with line numbering
- `cat -b file1.txt` - number non-blank output lines, overrides -n
- `cat -s filename` - squeeze multiple adjacent empty lines
- `cat ./dir1/dir2/dir3/text.txt` - display content of a txt file
- `cat .bashrc | grep -n fin` - search for word "fin" in .bashrc file with line numbering
- `cat > filename` - create a new file or overwrite an existing file
- `cat list1.txt list2.txt > out.txt` - concatenate list1.txt and list2.txt into out.txt
- `cat >> filename` - append content to an existing file
- `cat list1.txt >> list2.txt` – append content of list1.txt to list2.txt
- `cat /etc/passwd` - display computer accounts and users
- `cat /etc/shadow` – display computer passwords (hashed)
- `cat /etc/sudoers` – display user privileges

#### grep
- `grep` - print lines matching a pattern
- `grep` - print lines matching a pattern
- `grep [options] pattern [files]` – search for pattern in files
- `cat > geekfile.txt` – create a new file
- `grep -i "UNix" geekfile.txt` – search for pattern "UNix" case-insensitively
- `grep -c "unix" geekfile.txt` – count occurrences in a file
- `grep -l "unix" *` - display files containing the pattern
- `grep -l "unix" f1.txt f2.txt f3.xt f4.txt` – display files containing the pattern
- `grep -w "unix" geekfile.txt` – search for whole word "unix"
- `grep -n "unix" geekfile.txt` – display line numbers
- `grep -v "unix" geekfile.txt` – display lines not matching the pattern
- `command | grep "unix"` – search for pattern "unix" in a command
- `grep "o" /etc/ufw/new.txt` – search for 'o' pattern in /etc/ufw/new.txt file
- `cat .bashrc | grep alias > file.txt` – create a new file containing alias lines from .bashrc file

## Users, Permissions:

- `sudo` - execute a command as superuser
- `sudo update` - execute an update command as superuser (assuming you meant `apt update`)
- `su` - change user ID or become another user
- `sudo su` - change user to the superuser
- `sudo -s` - change user to the superuser
- `adduser john` – adds new user john
- `useradd nick` – adds new user nick
- `su john` – change user ID to john
- `sudo !!` - `!!` refers to the previous command, so `sudo !!` would execute the previous command as superuser
- `exit` - change the user ID back to the normal user
- `exit sudo` - change the user ID back to the normal user (not a valid command, you'd simply use `exit` to exit the superuser mode)
- `sudo apt update` - you forgot to type the `sudo` command before `apt update`
- `sudo !!` - `!!` refers to the previous command, so `sudo !!` would execute the previous command as superuser (`sudo apt update`)
- `users` - print the usernames of users currently logged in
- `id` - print real and effective user and group IDs
- `passwd` - change your password
- `userdel username` – delete user

## Network:

#### ifconfig
- `ifconfig` - configure network interface parameters

#### iwconfig
- `iwconfig` - configure a wireless network interface

#### arp
- `arp -a` - manipulate the system ARP cache

### route
- `route` - show/manipulate the IP routing table

#### ping
- `ping host` - send ICMP ECHO_REQUEST packets to a host
- `ping host -c 2` - send 2 packets to a host

#### netstat
- `netstat` - print network connections

#### NetworkManager restart
- `NetworkManager restart` – restart the Network Manager

#### service
- `service NetworkManager restart` – restart the Network Manager
- `sudo service apache2 start` - start the apache2 service
- `sudo service apache2 stop` - stop the apache2 service

#### systemctl
- `sudo systemctl enable ssh`
- `sudo systemctl disable ssh`

## Processes and Logout:

#### ps
- `ps` – display currently active processes

#### top
- `top` - display all running processes

#### htop
- `htop` – display all running processes colorfully

#### killall
- `killall` - kill processes by name *be careful*

#### bg
- `bg` - list stopped or background jobs
- `bg` - resume a stopped job in the background

#### fg
- `fg` – bring the most recent job to the foreground

#### exit
- `exit` - exit bash shell
- `reboot` – reboot the machine
- `shutdown` – shutdown the machine in a minute

## Useful Shortcuts:

- `F11` – full screen in terminal
- `ls -la; cat hello.txt` - execute two commands consecutively
- `make && sudo make install` - execute two commands consecutively
- `Ctrl + +` - increase text size in terminal
- `Ctrl + -` - decrease text size in terminal
- `Ctrl + C` - terminate a running command
- `Ctrl + L` - clear and refresh the screen
- `Ctrl + D` - tell bash no more input
- `Ctrl + R` - reverse search through command history `bck-i-search: _`
- `Ctrl + A` - move cursor to the beginning of the line
- `Ctrl + E` - move cursor to the end of the line
- `Crtl + W` - delete the previous word on the line
- `Ctrl + U` - delete the line

## Language Switching:

- `setxkbmap fi` – switch keyboard layout to Finnish
