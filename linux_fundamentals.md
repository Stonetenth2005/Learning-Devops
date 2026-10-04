# Linux Fundamentals --- Commands & Concepts Cheat Sheet

> Beginner-focused DevOps/Linux notes based on the uploaded **The
> Complete Linux Fundamentals Guide**.\
> The guide covers Linux FHS, CLI commands, file permissions and octal
> math, process management, networking, `systemctl`, disk utilities, and
> user administration. fileciteturn0file0L3-L7

------------------------------------------------------------------------

# 1. Core Linux Philosophy

## "Everything is a file"

Linux organizes the system into one unified hierarchical filesystem tree
beginning at `/`.

``` text
/
└── Linux filesystem
    ├── /bin
    ├── /boot
    ├── /dev
    ├── /etc
    ├── /home
    ├── /mnt
    ├── /tmp
    ├── /usr
    └── /var
```

The important idea is that Linux does not use separate drive roots like
`C:` and `D:`. Everything is organized below `/`.
fileciteturn0file0L8-L12

------------------------------------------------------------------------

# 2. `/` vs `/root`

These are **not the same thing**.

``` text
/
│
├── bin
├── etc
├── home
├── var
└── root
     └── root user's home directory
```

  Path      Meaning
  --------- -------------------------------------------------
  `/`       Root directory --- top of the entire filesystem
  `/root`   Home directory of the `root` superuser

`/` contains the complete filesystem hierarchy, while `/root` is simply
the root user's personal home directory. fileciteturn0file0L13-L23

------------------------------------------------------------------------

# 3. Linux File System Hierarchy (FHS)

Understanding where files belong is essential when working with Linux
servers. fileciteturn0file0L24-L25

``` text
/
├── /bin       → Essential user commands
├── /boot      → Bootloader + kernel files
├── /dev       → Device nodes
├── /etc       → Configuration
├── /home      → Normal users' home directories
├── /mnt       → Mount points
├── /tmp       → Temporary files
├── /var       → Variable data
│   └── /log   → System/application logs
└── /root      → Root user's home
```

## Directory Cheat Sheet

  -----------------------------------------------------------------------
  Directory                           Purpose
  ----------------------------------- -----------------------------------
  `/`                                 Root of the entire filesystem

  `/bin`                              Essential user binary commands such
                                      as `ls`, `cd`, `cp`, `systemctl`

  `/boot`                             Bootloader files, kernel images,
                                      and hardware startup files

  `/etc`                              System configuration files

  `/home`                             Personal home directories for
                                      normal users

  `/tmp`                              Temporary files

  `/var`                              Variable data

  `/var/log`                          Application and system logs

  `/mnt`                              Mount point for external storage,
                                      partitions, or network shares

  `/dev`                              Device nodes representing
                                      physical/virtual hardware
  -----------------------------------------------------------------------

Examples mentioned in the guide:

``` text
/etc/passwd       → User information
/etc/group        → Group information
/etc/os-release   → OS release information
/home/ubuntu      → User's home directory
/var/log          → Logs
```

The guide notes that `/tmp` contains temporary files and is cleared
automatically on system reboot, and that `/var/log` is important for
debugging. fileciteturn0file0L29-L52

------------------------------------------------------------------------

# 4. Navigation & Listing

## `ls`

Lists directory contents.

``` bash
ls
```

### Useful options

``` bash
ls -a
```

`-a` → show hidden files.

``` bash
ls -l
```

`-l` → long/listing format.

``` bash
ls -la
```

`-l` + `-a` → long format + hidden files.

## `cd`

Changes the current working directory.

``` bash
cd /etc
```

Go up one directory:

``` bash
cd ..
```

## `pwd`

Prints the current working directory as an absolute path.

``` bash
pwd
```

Example:

``` text
/home/ubuntu
```

The guide introduces `ls`, `cd`, and `pwd` as the basic
navigation/listing commands. fileciteturn0file0L53-L57

------------------------------------------------------------------------

# 5. Creating Files

The guide gives four common ways to create a file.

## 1. `touch`

``` bash
touch file.txt
```

Creates an empty file.

Multiple files:

``` bash
touch data{1..10}.txt
```

Creates:

``` text
data1.txt
data2.txt
...
data10.txt
```

## 2. `nano`

``` bash
nano file.txt
```

Simple terminal text editor.

## 3. `vi`

``` bash
vi file.txt
```

Standard terminal text editor.

## 4. Redirection

``` bash
echo "Hello" > file.txt
```

Creates the file and writes content to it.

The guide lists these four methods explicitly.
fileciteturn0file0L58-L63

------------------------------------------------------------------------

# 6. Copying, Moving & Deleting

## `cp`

Copy a file:

``` bash
cp source.txt destination.txt
```

Copy a directory recursively:

``` bash
cp -r dir1 dir2
```

`-r` → recursive.

## `mv`

Move a file:

``` bash
mv file.txt /tmp/
```

Rename a file:

``` bash
mv old.txt new.txt
```

## `rm`

Delete a file:

``` bash
rm file.txt
```

## `rmdir`

Remove an empty directory:

``` bash
rmdir mydir
```

## `rm -rf`

Force recursive deletion:

``` bash
rm -rf mydir
```

-   `-r` → recursive
-   `-f` → force

**Be extremely careful with `rm -rf`.**

The guide covers `cp`, `mv`, `rm`, `rmdir`, and recursive deletion.
fileciteturn0file0L64-L67

------------------------------------------------------------------------

# 7. File Content & Redirection

## `>`

Redirects output to a file and **overwrites** existing content.

``` bash
echo "First Line" > file.txt
```

``` text
Command
   │
   ▼
echo "First Line"
   │
   │ >
   ▼
file.txt
```

## `>>`

Redirects output and **appends** to the file.

``` bash
echo "Second Line" >> file.txt
```

``` text
Existing content
      +
Second Line
      ↓
file.txt
```

### Remember

``` text
>    → overwrite
>>   → append
```

The guide explicitly distinguishes these two redirection operators.
fileciteturn0file0L70-L79

------------------------------------------------------------------------

# 8. `cat`

Display a file:

``` bash
cat file.txt
```

Example:

``` bash
cat /etc/passwd
```

`cat` sends the file's contents to standard output (the terminal by
default). fileciteturn0file0L80-L80

------------------------------------------------------------------------

# 9. `head`

Shows the first 10 lines of a file by default.

``` bash
head docker.txt
```

------------------------------------------------------------------------

# 10. `tail`

Shows the last 10 lines by default.

``` bash
tail docker.txt
```

Very useful for logs:

``` bash
tail -f /var/log/syslog
```

`-f` → follow the file as it changes.

This is particularly useful when monitoring logs in real time.
fileciteturn0file0L81-L85

------------------------------------------------------------------------

# 11. `grep`

Searches text for a pattern.

``` bash
grep -i "engine" docker.txt
```

`-i` → case-insensitive search.

You can combine commands using a pipe:

``` bash
cat docker.txt | grep -i "engine"
```

``` text
cat docker.txt
      │
      ▼
    pipe |
      │
      ▼
grep -i "engine"
      │
      ▼
matching lines
```

The guide uses `grep -i` and explains that `-i` makes the search
case-insensitive. fileciteturn0file0L86-L91

------------------------------------------------------------------------

# 12. Pipes `|`

A pipe sends the output of one command into another command.

``` bash
command1 | command2
```

Example:

``` bash
history | grep "systemctl"
```

``` text
history
   │
   │ output
   ▼
  | pipe
   │
   ▼
grep "systemctl"
   │
   ▼
matching commands
```

------------------------------------------------------------------------

# 13. `history`

Shows recently executed terminal commands.

``` bash
history
```

Search history:

``` bash
history | grep "systemctl"
```

The guide highlights `history` as useful for tracing commands executed
during troubleshooting incidents. fileciteturn0file0L92-L96

------------------------------------------------------------------------

# 14. Linux File Permissions

Run:

``` bash
ls -l
```

You may see:

``` text
-rw-r--r--
```

This is a 10-character permission representation.

``` text
-rw-r--r--
│││ │││ │││
│││ │││ ││└── Others
│││ │││ └───── Others
│││ ││└─────── Others
│││ └───────── Group
││└─────────── Group
│└──────────── Group
└───────────── File type
```

The permission groups are:

``` text
Owner        Group        Others
  │            │            │
 rw-          r--          r--
```

------------------------------------------------------------------------

# 15. Permission Symbols

  Permission   Symbol     Value
  ------------ -------- -------
  Read         `r`            4
  Write        `w`            2
  Execute      `x`            1

The octal values come from binary bit positions:

``` text
r = 2² = 4
w = 2¹ = 2
x = 2⁰ = 1
```

The guide presents this mapping explicitly.
fileciteturn0file0L97-L110

------------------------------------------------------------------------

# 16. Octal Permission Math

For each category, add the permission values.

``` text
rwx = 4 + 2 + 1 = 7
rw- = 4 + 2 + 0 = 6
r-x = 4 + 0 + 1 = 5
r-- = 4 + 0 + 0 = 4
-wx = 0 + 2 + 1 = 3
-w- = 0 + 2 + 0 = 2
--x = 0 + 0 + 1 = 1
--- = 0 + 0 + 0 = 0
```

Example:

``` text
-rwxr-xr--
  │   │   │
  │   │   └── 4
  │   └────── 5
  └────────── 7

chmod 754
```

------------------------------------------------------------------------

# 17. `chmod`

Changes file permissions.

## `chmod 777`

``` bash
chmod 777 file
```

``` text
rwx rwx rwx
 7   7   7
```

Everyone has read, write, and execute access.

## `chmod 755`

``` bash
chmod 755 file
```

``` text
rwx r-x r-x
 7   5   5
```

Owner → full access\
Group → read + execute\
Others → read + execute

The guide identifies this as a standard mode for scripts.

## `chmod 644`

``` bash
chmod 644 file
```

``` text
rw- r-- r--
 6   4   4
```

Owner → read + write\
Group → read\
Others → read

The guide identifies this as a standard mode for data files.

## `chmod 616`

``` bash
chmod 616 file
```

``` text
rw- -wx rw-
 6   1   6
```

Owner → read + write\
Group → execute\
Others → read + write

The guide lists these common permission examples.
fileciteturn0file0L111-L118

------------------------------------------------------------------------

# 18. Permission Mental Model

``` text
                chmod 754
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
     Owner        Group        Others
       7            5             4
       │            │             │
     rwx          r-x           r--
       │            │             │
   4+2+1        4+0+1         4+0+0
```

------------------------------------------------------------------------

# 19. Process Management

A **process** is a running program.

``` text
Program
   │
   │ execute
   ▼
Process
   │
   ├── PID
   ├── CPU usage
   ├── Memory
   └── resources
```

## `ps`

Shows a snapshot of active processes.

``` bash
ps
```

The guide describes it as a process snapshot for the active terminal
session. fileciteturn0file0L119-L124

## `top`

Interactive real-time resource monitor.

``` bash
top
```

Shows information such as:

-   CPU usage
-   RAM usage
-   PID
-   Load average
-   Running processes

## `kill`

Terminates a process using its PID.

``` bash
kill 1000
```

## `pkill`

Terminates processes by name.

``` bash
pkill nginx
```

------------------------------------------------------------------------

# 20. System Status Commands

``` bash
uptime
```

Shows how long the system has been running and system load information.

``` bash
whoami
```

Shows the current user.

``` bash
date
```

Shows the current date/time.

The guide groups these as basic system-status utility commands.
fileciteturn0file0L119-L124

------------------------------------------------------------------------

# 21. Networking Commands

## `ping`

Tests network reachability and packet loss.

``` bash
ping <IP>
```

Example:

``` bash
ping 8.8.8.8
```

------------------------------------------------------------------------

## `ifconfig`

Displays IP addresses and network interfaces.

``` bash
ifconfig
```

`ifconfig` is a traditional utility.

------------------------------------------------------------------------

## `ip a`

Modern way to view network interfaces and IP addresses.

``` bash
ip a
```

``` text
Machine
  │
  └── Network interfaces
       ├── IP address
       ├── MAC address
       └── interface state
```

The guide lists both `ifconfig` and `ip a`.
fileciteturn0file0L125-L130

------------------------------------------------------------------------

## `curl`

Transfers data from/to a server, commonly using HTTP/HTTPS.

``` bash
curl <URL>
```

Example:

``` bash
curl https://example.com
```

Useful for testing APIs and HTTP services.

------------------------------------------------------------------------

# 22. `systemctl` --- Service Management

`systemctl` is used to control system services.

Example with Docker:

``` bash
sudo systemctl start docker
```

Start a service:

``` bash
sudo systemctl start docker
```

Stop:

``` bash
sudo systemctl stop docker
```

Restart:

``` bash
sudo systemctl restart docker
```

Enable:

``` bash
sudo systemctl enable docker
```

### Service lifecycle

``` text
             docker.service
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
     start         stop        restart
       │            │            │
       ▼            ▼            ▼
   Running       Stopped      Restarted
```

`enable` configures the service to start automatically according to the
system's service configuration. fileciteturn0file0L131-L135

------------------------------------------------------------------------

# 23. Disk Storage

## `df -h`

Shows filesystem disk capacity across mounted filesystems.

``` bash
df -h
```

`-h` → human-readable sizes.

``` text
Filesystem
    │
    ├── Size
    ├── Used
    ├── Available
    └── Use%
```

## `du -sh`

Shows space consumed by a directory.

``` bash
du -sh /var/log
```

Options:

-   `-s` → summary
-   `-h` → human-readable

``` text
/var/log
   │
   ▼
du -sh
   │
   ▼
Total space used
```

The guide distinguishes `df` for filesystem capacity from `du` for
directory usage. fileciteturn0file0L136-L140

------------------------------------------------------------------------

# 24. Users & Groups

Linux uses users and groups to control identity and permissions.

``` text
User
 │
 ├── UID
 ├── Primary Group
 └── Supplementary Groups
```

## `useradd`

Create a user:

``` bash
sudo useradd <user>
```

## `userdel -r`

Delete a user and remove their home directory/files associated with the
deletion option:

``` bash
sudo userdel -r <user>
```

`-r` → remove the user's home directory and mail spool where applicable.

## `passwd`

Set/change a user's password:

``` bash
sudo passwd <user>
```

## `groupadd`

Create a group:

``` bash
sudo groupadd <group>
```

## `usermod -aG`

Add a user to a supplementary group:

``` bash
sudo usermod -aG <group> <user>
```

Important options:

``` text
-a → append
-G → supplementary groups
```

The guide lists these user/group administration commands.
fileciteturn0file0L141-L144

------------------------------------------------------------------------

# 25. Key Commands --- Quick Revision

## Filesystem

``` bash
ls
ls -a
ls -l
ls -la
cd <path>
cd ..
pwd
touch file
nano file
vi file
cat file
cp source destination
cp -r dir1 dir2
mv old new
rm file
rmdir dir
rm -rf dir
```

## File content/search

``` bash
cat file
head file
tail file
tail -f file
grep -i "text" file
history
history | grep "text"
```

## Redirection

``` bash
echo "text" > file
echo "text" >> file
```

``` text
>   → overwrite
>>  → append
|   → pipe output into another command
```

## Permissions

``` bash
ls -l
chmod 777 file
chmod 755 file
chmod 644 file
```

``` text
r = 4
w = 2
x = 1
```

## Processes

``` bash
ps
top
kill <PID>
pkill <name>
uptime
whoami
date
```

## Networking

``` bash
ping <IP>
ifconfig
ip a
curl <URL>
```

## Services

``` bash
sudo systemctl start docker
sudo systemctl stop docker
sudo systemctl restart docker
sudo systemctl enable docker
```

## Disk

``` bash
df -h
du -sh /var/log
```

## Users/groups

``` bash
sudo useradd <user>
sudo userdel -r <user>
sudo passwd <user>
sudo groupadd <group>
sudo usermod -aG <group> <user>
```

------------------------------------------------------------------------

# 26. High-Level Linux Mental Model

``` text
                         LINUX
                           │
            ┌──────────────┴──────────────┐
            │                             │
       FILESYSTEM                       PROCESSES
            │                             │
       ┌────┼────┐                   ┌────┼────┐
       │    │    │                   │    │    │
      /etc /home /var                ps   top  kill
       │         │
       │       /var/log
       │
       └── Configuration

            │
            ▼
        PERMISSIONS
            │
       ┌────┼────┐
       │    │    │
       u    g    o
       │    │    │
      rwx  rwx  rwx
       │    │    │
       └────┼────┘
            │
          chmod
```

------------------------------------------------------------------------

# 27. DevOps Troubleshooting Flow

A useful way to connect these commands is:

``` text
                 Something is wrong
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       Process         Network         Disk
          │              │              │
         ps             ip a           df -h
         top            ping           du -sh
         kill           curl
          │              │
          └──────────────┼──────────────┘
                         ▼
                       Logs
                         │
                      /var/log
                         │
                    cat / tail
                         │
                       grep
```

------------------------------------------------------------------------

# 28. AWS Practice Warning

When practicing Linux on AWS EC2 Free Tier instances, the guide
recommends stopping or terminating instances after finishing to avoid
unnecessarily consuming the available monthly quota.
fileciteturn0file0L145-L147

------------------------------------------------------------------------

# 29. Core Commands to Memorize First

Don't try to memorize everything at once.

Start with these:

``` bash
pwd
ls
cd
mkdir
touch
cat
cp
mv
rm
head
tail
grep
history
ps
top
kill
ip a
ping
curl
df -h
du -sh
chmod
whoami
systemctl
```

Then learn the options (`-a`, `-l`, `-r`, `-f`, `-h`, `-i`, `-s`, `-G`,
etc.) as you encounter them.
