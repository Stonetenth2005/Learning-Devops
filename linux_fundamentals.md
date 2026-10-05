# Linux Fundamentals

A concise Linux command reference for DevOps beginners.

> **Note:** Commands below are grouped by purpose. Use `man <command>`
> whenever you need to check options instead of memorizing every flag.

------------------------------------------------------------------------

## 1. Linux Basics & System Information

  Command      Purpose                              Example
  ------------ ------------------------------------ ------------
  `whoami`     Current logged-in user               `whoami`
  `hostname`   System hostname                      `hostname`
  `uname`      Kernel information                   `uname`
  `uname -a`   Detailed system/kernel information   `uname -a`
  `uptime`     System uptime                        `uptime`
  `date`       Current date/time                    `date`
  `history`    Command history                      `history`
  `clear`      Clear terminal                       `clear`
  `id`         UID, GID and groups                  `id`
  `groups`     Groups of a user                     `groups`
  `free`       Memory usage                         `free`
  `free -m`    Memory in MB                         `free -m`
  `df -h`      Disk usage                           `df -h`
  `nproc`      Number of CPU cores                  `nproc`
  `top`        Processes/resource usage             `top`

### OS information

``` bash
cat /etc/os-release
sudo ls /etc/*release
```

------------------------------------------------------------------------

## 2. Linux File System

Linux follows a hierarchical file system:

``` text
/
├── bin      → essential commands
├── boot     → boot files
├── dev      → device files
├── etc      → system configuration
├── home     → user home directories
├── lib      → shared libraries
├── media    → removable media
├── mnt      → temporary mounts
├── opt      → optional software
├── proc     → process/kernel information
├── root     → root user's home
├── run      → runtime data
├── sbin     → system administration commands
├── tmp      → temporary files
├── usr      → user programs/libraries
└── var      → variable data/logs
```

------------------------------------------------------------------------

## 3. Navigation & Directories

### `pwd`

Print the current working directory.

``` bash
pwd
```

### `ls`

List directory contents.

``` bash
ls
ls -l
ls -a
ls -la
```

To list the contents of a child directory while staying in the current
directory:

``` bash
ls child
```

`ls child` shows **only the contents of `child`**, not files from the
parent/current directory.

``` text
parent/
├── parent.txt
└── child/
    ├── file1.txt
    └── file2.txt
```

``` bash
ls child
# file1.txt  file2.txt
```

### `cd`

Change directory.

``` bash
cd child
cd ..
cd ~
cd /
```

### `mkdir`

Create a directory.

``` bash
mkdir project
mkdir dir1 dir2
mkdir -p parent/child/grandchild
```

------------------------------------------------------------------------

## 4. File Creation & Editing

### `touch`

Create empty files or update timestamps.

``` bash
touch data.txt
```

Create multiple files:

``` bash
touch data1 data2 data3
touch data{1..10}
```

> `touch` is especially useful for creating multiple files in one
> command.

### `cat`

Display file contents.

``` bash
cat data.txt
```

Create/write a file:

``` bash
cat > data.txt
```

Append to a file:

``` bash
cat >> data.txt
```

Display contents of files inside a directory:

``` bash
cat child/*
```

### `nano`

Terminal text editor.

``` bash
nano data.txt
```

> **Don't memorize the `nano` shortcuts.** The operation/help box is
> already displayed at the bottom of the editor.

Common operations shown there include:

``` text
Ctrl + O → Save
Ctrl + X → Exit
Ctrl + W → Search
Ctrl + K → Cut line
Ctrl + U → Paste
```

### `vim`

Advanced terminal text editor.

``` bash
vim data.txt
```

### Quick difference

  Command   Main purpose                   Interactive editing
  --------- ------------------------------ ---------------------
  `touch`   Create empty files             No
  `cat`     Read/write file contents       No
  `nano`    Edit files interactively       Yes
  `vim`     Advanced interactive editing   Yes

------------------------------------------------------------------------

## 5. Copy, Move & Remove

### `cp`

Copy files or directories.

``` bash
cp file.txt backup/
cp file.txt newfile.txt
```

Copy a directory:

``` bash
cp -r source/ destination/
```

`-r` is required when the **source itself is a directory**.

### Copy only directory contents

If you don't want to copy the source folder itself, but only its
contents:

``` bash
cp source/* destination/
```

If the contents include subdirectories:

``` bash
cp -r source/* destination/
```

> `source/*` normally does not include hidden files.

### Copy into a new directory

``` bash
cp -r source/ destination/new/
```

This copies the `source` directory into `new`.

To copy only the contents into `new`:

``` bash
cp -r source/* destination/new/
```

### `mv`

Move or rename files/directories.

``` bash
mv file.txt backup/
mv old.txt new.txt
```

### `rm`

Remove files.

``` bash
rm file.txt
```

Remove a directory and its contents:

``` bash
rm -r directory/
```

Force removal:

``` bash
rm -rf directory/
```

> Use `rm -rf` carefully. Deleted files normally do not go to a recycle
> bin.

------------------------------------------------------------------------

## 6. File Viewing & Search

  Command   Purpose               Example
  --------- --------------------- ------------------------
  `cat`     View file             `cat file.txt`
  `less`    View large files      `less /var/log/syslog`
  `more`    Page through a file   `more file.txt`
  `head`    View beginning        `head -n 10 file.txt`
  `tail`    View end              `tail -n 100 file.txt`
  `grep`    Search inside files   `grep ERROR file.txt`

------------------------------------------------------------------------

## 7. `man` --- Manual Pages

`man` displays the manual/documentation for a command.

``` bash
man cp
man touch
man cat
```

Useful inside `man`:

``` text
Space    → Next page
Enter    → Scroll one line
b        → Previous page
/word    → Search
n        → Next search result
q        → Quit
```

Use `man` to check options when you are unsure about a command.

Example:

``` bash
man cp
```

------------------------------------------------------------------------

## 8. `sudo`

`sudo` allows a user to execute a command with elevated/root privileges.

``` bash
sudo <command>
```

Examples:

``` bash
sudo apt update
sudo touch /root/test.txt
```

> `sudo` is a **permission/elevation command**, not a file creation or
> editing command.

------------------------------------------------------------------------

## 9. Processes & Services

### Processes

``` bash
ps
ps aux
top
```

### Kill a process

``` bash
kill 1234
kill -9 1234
```

### Services

``` bash
systemctl status nginx
systemctl restart nginx
```

------------------------------------------------------------------------

## 10. Networking

  Command      Purpose                         Example
  ------------ ------------------------------- -------------------------------------
  `ping`       Check connectivity              `ping google.com`
  `ip a`       Show IP/network configuration   `ip a`
  `ifconfig`   Network configuration           `ifconfig`
  `netstat`    Network connections             `netstat -tulnp`
  `curl`       Fetch URL data                  `curl https://api.github.com`
  `wget`       Download files                  `wget https://example.com/file.zip`

------------------------------------------------------------------------

## 11. Permissions & Ownership

### `chmod`

Change file permissions.

``` bash
chmod 755 script.sh
```

### `chown`

Change file ownership.

``` bash
chown user:group file.txt
```

------------------------------------------------------------------------

## 12. Package Management

### Ubuntu/Debian

``` bash
sudo apt update
sudo apt upgrade -y
sudo apt install nginx -y
```

### RHEL/CentOS

``` bash
sudo yum install nginx -y
```

------------------------------------------------------------------------

## 13. Disk & Storage

### `df`

Show filesystem/disk usage.

``` bash
df -h
```

### `du`

Show file/directory size.

``` bash
du -sh /var/log
```

------------------------------------------------------------------------

## 14. Scheduling & Background Jobs

### `crontab`

Edit cron jobs:

``` bash
crontab -e
```

Example --- run daily at 2 AM:

``` cron
0 2 * * * /home/user/backup.sh
```

### `nohup`

Run a command in the background so it can continue after logout:

``` bash
nohup python3 app.py &
```

------------------------------------------------------------------------

## 15. User Management

  Command                 Purpose
  ----------------------- -----------------------------------
  `adduser`               Add a user
  `useradd`               Create user non-interactively
  `usermod`               Modify user account
  `passwd`                Change password
  `id`                    Show UID/GID/groups
  `groups`                Show user's groups
  `deluser` / `userdel`   Delete user
  `who`                   List logged-in users
  `w`                     Show logged-in users and activity
  `last`                  Show login history

Examples:

``` bash
sudo adduser devops
sudo useradd -m -s /bin/bash devuser
sudo usermod -aG sudo devops
sudo passwd devops
id devops
groups devops
```

------------------------------------------------------------------------

## 16. Useful Terminal Shortcuts

  Shortcut     Purpose
  ------------ -------------------------------------
  `!!`         Run the last command again
  `!n`         Run command number `n` from history
  `Ctrl + C`   Cancel/interrupt running command
  `Ctrl + L`   Clear terminal screen

------------------------------------------------------------------------

## Quick Mental Model

``` text
Navigation
├── pwd
├── ls
├── cd
└── mkdir

Files
├── touch  → create
├── cat    → read/write
├── nano   → edit
├── vim    → advanced edit
├── cp     → copy
├── mv     → move/rename
└── rm     → remove

System
├── ps / top       → processes
├── systemctl      → services
├── df / du        → storage
└── uname / free   → system information

Network
├── ping
├── ip
├── netstat
├── curl
└── wget

Permissions
├── sudo
├── chmod
└── chown
```
