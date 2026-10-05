# Linux Fundamentals — DevOps Notes

A practical Linux reference for DevOps beginners.

> **Learning rule:** Understand what a command does and when to use it. Do not try to memorize every option. Use `man <command>` whenever you need the complete documentation.

---

# 1. Linux File System

Linux uses a **hierarchical file system**. Everything starts from the root directory `/`.

```text
/
├── bin/       → Essential user commands
├── boot/      → Boot-related files
├── dev/       → Device files
├── etc/       → System configuration files
├── home/      → Users' home directories
├── lib/       → Shared libraries
├── media/     → Removable media
├── mnt/       → Temporary mount points
├── opt/       → Optional/add-on software
├── proc/      → Process and kernel information
├── root/      → Home directory of root user
├── run/       → Runtime data
├── sbin/      → System administration commands
├── tmp/       → Temporary files
├── usr/       → User programs and libraries
└── var/       → Variable data such as logs
```

## Important idea: absolute vs relative paths

```text
/
└── home/
    └── tushar/
        └── project/
            ├── app/
            │   └── main.py
            └── data/
                └── data.txt
```

If you are inside `project/`:

```bash
cd app
```

is a **relative path**.

```bash
cd /home/tushar/project/app
```

is an **absolute path**.

Useful path symbols:

```text
/       → Root
.       → Current directory
..      → Parent directory
~       → Current user's home directory
```

---

# 2. Navigation & Directory Commands

## `pwd` — Print Working Directory

Shows where you currently are.

```bash
pwd
```

Example:

```text
/home/tushar/project
```

---

## `ls` — List Directory Contents

Lists files and directories.

```bash
ls
```

### Common forms

```bash
ls -l      # Detailed listing
ls -a      # Include hidden files
ls -la     # Detailed + hidden files
```

### Listing a child directory

Suppose:

```text
parent/
├── parent1.txt
├── parent2.txt
└── child/
    ├── child1.txt
    └── child2.txt
```

If you are inside `parent/`:

```bash
ls
```

Output:

```text
parent1.txt  parent2.txt  child
```

But:

```bash
ls child
```

Output:

```text
child1.txt  child2.txt
```

> `ls child` displays **only the contents of `child`**. It does not include files from the current/parent directory.

### Useful distinction

```bash
ls child
```

→ List what's inside `child`.

```bash
cat child/file1.txt
```

→ Display the contents of `file1.txt`.

```bash
cat child/*
```

→ Display the contents of files matched inside `child`.

---

## `cd` — Change Directory

Move between directories.

```bash
cd child
```

Go to the parent:

```bash
cd ..
```

Go to home:

```bash
cd ~
```

Go to root:

```bash
cd /
```

A common navigation flow:

```text
Current directory
      │
      ├── cd child ──→ child/
      │
      ├── cd .. ─────→ parent/
      │
      ├── cd ~ ──────→ user's home
      │
      └── cd / ──────→ root
```

---

## `mkdir` — Make Directory

Create a directory:

```bash
mkdir project
```

Create multiple directories:

```bash
mkdir dir1 dir2 dir3
```

Create nested directories:

```bash
mkdir -p parent/child/grandchild
```

Without `-p`, the parent directories may need to already exist.

---

# 3. File Creation & Editing

These commands are related, but they have different purposes.

```text
                 FILE OPERATIONS
                       │
        ┌──────────────┼──────────────┐
        │              │              │
      touch           cat           Editors
        │              │          ┌────┴────┐
   Create empty    Read/write    nano       vim
      files          content      │           │
                                  Easy       Advanced
```

## `touch` — Create Files

Creates an empty file if it doesn't exist.

```bash
touch data.txt
```

If the file already exists, `touch` updates its timestamp without deleting its contents.

### Create multiple files

```bash
touch data1 data2 data3
```

### Brace expansion

```bash
touch data{1..10}
```

Creates:

```text
data1
data2
data3
data4
data5
data6
data7
data8
data9
data10
```

> **Important:** `{1..10}` is a range. `{1-10}` is not the same thing.

### Why `touch` is useful

Unlike interactive editors, `touch` can conveniently create many empty files in one command.

```bash
touch data{1..10}
```

---

## `cat` — View / Write File Contents

### View a file

```bash
cat data.txt
```

### View multiple files

```bash
cat file1.txt file2.txt
```

### Create and write a file

```bash
cat > data.txt
```

Type the content, then press:

```text
Ctrl + D
```

to finish input.

### Append content

```bash
cat >> data.txt
```

This adds content to the end instead of replacing existing content.

### View files inside a directory

```bash
cat child/*
```

> `cat` is primarily a content-oriented command. It can also create/write files through redirection.

---

# 4. `nano` — Simple Terminal Editor

Open or create a file:

```bash
nano data.txt
```

`nano` lets you edit the file interactively.

## Nano operations

> **Do not memorize these shortcuts.** The help/operation bar is already displayed at the bottom of the `nano` editor. Use it when needed.

Common operations:

```text
Ctrl + O  → Save / Write Out
Enter     → Confirm filename
Ctrl + X  → Exit
Ctrl + W  → Search
Ctrl + K  → Cut current line
Ctrl + U  → Paste
Ctrl + G  → Help
Ctrl + C  → Show cursor position
```

Typical workflow:

```text
nano data.txt
      │
      ▼
Write / edit content
      │
      ▼
Ctrl + O
      │
      ▼
Enter
      │
      ▼
Ctrl + X
      │
      ▼
File saved
```

---

# 5. `vim` — Advanced Terminal Editor

Open or create a file:

```bash
vim data.txt
```

`vim` is a more powerful terminal editor and uses different modes.

Basic conceptual flow:

```text
vim file.txt
     │
     ▼
Normal Mode
     │
     ├── i ──→ Insert Mode
     │            │
     │            └── Esc ──→ Normal Mode
     │
     └── : ──→ Command Mode
                    │
                    ├── :w  → Save
                    ├── :q  → Quit
                    └── :wq → Save + Quit
```

For now, remember the distinction:

```text
nano → simpler interactive editor
vim  → more powerful/advanced editor
```

---

# 6. Quick Difference: `touch`, `cat`, `nano`, `vim`

| Command | Main Purpose | Interactive Editing | Multiple Files Easily |
|---|---|---:|---:|
| `touch` | Create empty files / update timestamps | ❌ | ✅ |
| `cat` | Read/write file contents | ❌ | ✅ for reading |
| `nano` | Edit files interactively | ✅ | ❌ |
| `vim` | Advanced file editing | ✅ | ❌ |

Mental model:

```text
touch → Create
cat   → Read / write contents
nano  → Edit simply
vim   → Edit with advanced controls
```

---

# 7. Copy, Move & Remove

## `cp` — Copy

Copy a file:

```bash
cp file.txt backup/
```

Copy and rename:

```bash
cp file.txt newfile.txt
```

### Copy a directory

When the **source itself is a directory**, use recursive copy:

```bash
cp -r source/ destination/
```

### Why `-r`?

`cp` can directly copy files:

```bash
cp file.txt destination/
```

But a directory can contain files and other directories:

```text
source/
├── file1.txt
├── file2.txt
└── nested/
    └── file3.txt
```

`-r` means recursively traverse the directory and copy its contents.

```text
cp source/ destination/
        │
        └── source is a directory
                    │
                    ▼
                 cp -r
                    │
                    ▼
        copy directory recursively
```

---

## Copy only the contents of a directory

If you **do not want the source folder itself**, but only the items inside it:

```bash
cp source/* destination/
```

Example:

```text
source/
├── file1.txt
├── file2.txt
└── file3.txt
```

```bash
cp source/* destination/
```

Result:

```text
destination/
├── file1.txt
├── file2.txt
└── file3.txt
```

The `source/` directory itself is not copied.

### If the contents include subdirectories

Use:

```bash
cp -r source/* destination/
```

because the subdirectories themselves need recursive copying.

> **Note:** `source/*` normally does not match hidden files beginning with `.`.

---

## Copy into a new destination directory

If you want the source directory to appear inside a new directory:

```bash
cp -r source/ destination/new/
```

Conceptually:

```text
Before:

destination/
└── ...

source/
├── file1
└── file2


Command:

cp -r source/ destination/new/


After:

destination/
└── new/
    └── source/
        ├── file1
        └── file2
```

If you want only the **contents of `source`** inside `new`:

```bash
cp -r source/* destination/new/
```

Result:

```text
destination/
└── new/
    ├── file1
    └── file2
```

---

## `mv` — Move or Rename

Move a file:

```bash
mv app.log backup/
```

Rename a file:

```bash
mv old.txt new.txt
```

The same command handles both moving and renaming.

---

## `rm` — Remove

Remove a file:

```bash
rm file.txt
```

Remove a directory and its contents:

```bash
rm -r directory/
```

Force recursive removal:

```bash
rm -rf directory/
```

> **Warning:** `rm -rf` is destructive. Deleted files normally do not go to a recycle bin.

---

# 8. File Viewing & Searching

## `cat`

```bash
cat file.txt
```

View file content.

## `less`

Useful for large files:

```bash
less /var/log/syslog
```

Inside `less`:

```text
Space → Next page
b     → Previous page
q     → Quit
```

## `more`

Another pager:

```bash
more file.txt
```

## `head`

View the beginning of a file:

```bash
head file.txt
```

First 10 lines:

```bash
head -n 10 file.txt
```

## `tail`

View the end:

```bash
tail file.txt
```

Last 100 lines:

```bash
tail -n 100 /var/log/syslog
```

## `grep`

Search inside files:

```bash
grep ERROR /var/log/syslog
```

Conceptually:

```text
Large file
   │
   ▼
grep "ERROR"
   │
   ▼
Only matching lines
```

---

# 9. `man` — Manual Pages

`man` displays the manual/documentation for a command.

```bash
man cp
man touch
man cat
man ls
```

If you forget an option:

```bash
man cp
```

You can find information about options such as `-r`, `-i`, `-v`, etc.

Useful controls:

```text
Space       → Next page
Enter       → Scroll one line
b           → Previous page
/word       → Search
n           → Next search result
q           → Quit
```

> **Don't try to memorize every flag.** `man` is the built-in reference.

---

# 10. `sudo` — Elevated Privileges

`sudo` allows a user to execute a command with elevated/root privileges.

```bash
sudo <command>
```

Examples:

```bash
sudo apt update
sudo touch /root/test.txt
```

Important distinction:

```text
touch → file operation
cat   → file content operation
nano  → editing
vim   → advanced editing

sudo  → permission/elevation
```

`sudo` is **not** a file creation/editing command.

---

# 11. Processes & Resource Monitoring

## `ps`

Show processes.

```bash
ps
ps aux
```

`ps aux` provides a broader view of running processes.

## `top`

Interactive process and resource monitor:

```bash
top
```

Useful for observing:

```text
CPU usage
Memory usage
Running processes
Process IDs
```

## `kill`

Terminate a process using its PID:

```bash
kill 1234
```

Force termination:

```bash
kill -9 1234
```

Basic process flow:

```text
Application
    │
    ▼
Process
    │
    ├── ps / top → inspect
    │
    └── kill PID → terminate
```

---

# 12. Services

`systemctl` is commonly used to manage services.

Check status:

```bash
systemctl status nginx
```

Restart a service:

```bash
systemctl restart nginx
```

Typical flow:

```text
Service
  │
  ├── systemctl status nginx
  │             │
  │             └── Check current state
  │
  └── systemctl restart nginx
                │
                └── Restart service
```

---

# 13. Networking Commands

## `ping`

Check basic network connectivity:

```bash
ping google.com
```

## `ip`

Show IP/network configuration:

```bash
ip a
```

## `ifconfig`

Older/traditional network configuration command:

```bash
ifconfig
```

## `netstat`

Show network connections/listening ports:

```bash
netstat -tulnp
```

## `curl`

Fetch data from a URL:

```bash
curl https://api.github.com
```

It is commonly used for APIs and HTTP requests.

## `wget`

Download files:

```bash
wget https://example.com/file.zip
```

Networking mental model:

```text
Your machine
     │
     ├── ping → Is the host reachable?
     │
     ├── ip a → What is my network/IP configuration?
     │
     ├── netstat → What connections/ports exist?
     │
     ├── curl → Fetch/interact with a URL
     │
     └── wget → Download a file
```

---

# 14. Permissions & Ownership

Linux controls access using **permissions** and **ownership**.

## `chmod`

Change file permissions:

```bash
chmod 755 script.sh
```

## `chown`

Change file owner/group:

```bash
chown user:group file.txt
```

Conceptually:

```text
File
 │
 ├── Owner
 │
 ├── Group
 │
 └── Permissions
       │
       ├── Read
       ├── Write
       └── Execute
```

---

# 15. Package Management

## Ubuntu / Debian

Update package information:

```bash
sudo apt update
```

Upgrade installed packages:

```bash
sudo apt upgrade -y
```

Install a package:

```bash
sudo apt install nginx -y
```

Typical flow:

```text
sudo apt update
       │
       ▼
Refresh package information
       │
       ▼
sudo apt upgrade -y
       │
       ▼
Upgrade installed packages
```

## RHEL / CentOS

The source document lists:

```bash
sudo yum install nginx -y
```

---

# 16. Disk & Storage

## `df`

Show filesystem/disk usage:

```bash
df -h
```

`-h` makes sizes human-readable.

## `du`

Show file/directory size:

```bash
du -sh /var/log
```

Difference:

```text
df → How much space is available/used on filesystems?
du → How much space is being used by a file/directory?
```

---

# 17. Scheduling & Background Jobs

## `crontab`

Edit cron jobs:

```bash
crontab -e
```

### Cron syntax

```text
* * * * *
│ │ │ │ │
│ │ │ │ └── Day of week
│ │ │ └──── Month
│ │ └────── Day of month
│ └──────── Hour
└────────── Minute
```

Example: run daily at 2 AM:

```cron
0 2 * * * /home/user/backup.sh
```

Flow:

```text
Cron scheduler
      │
      ▼
Matches schedule
      │
      ▼
Runs command/script
```

## `nohup`

Run a command in the background:

```bash
nohup python3 app.py &
```

Useful when you want a process to continue running after logout.

---

# 18. User Management

## Common commands

| Command | Purpose |
|---|---|
| `adduser` | Add a user |
| `useradd` | Create user non-interactively |
| `usermod` | Modify a user account |
| `passwd` | Change password |
| `id` | Display UID, GID and groups |
| `groups` | Show groups a user belongs to |
| `deluser` / `userdel` | Delete a user |
| `who` | List logged-in users |
| `w` | Show who is logged in and what they are doing |
| `last` | Show login history |

Examples:

```bash
sudo adduser devops
sudo useradd -m -s /bin/bash devuser
sudo usermod -aG sudo devops
sudo passwd devops
id devops
groups devops
sudo deluser devops
```

Useful session commands:

```bash
who
w
last
```

---

# 19. System Information & Utilities

## `uname`

Kernel/system information:

```bash
uname
uname -a
```

## `hostname`

Show system hostname:

```bash
hostname
```

## `uptime`

Show how long the system has been running:

```bash
uptime
```

## `whoami`

Show current username:

```bash
whoami
```

## `history`

Show command history:

```bash
history
```

## `date`

Show current system date/time:

```bash
date
```

## `clear`

Clear the terminal:

```bash
clear
```

---

# 20. Useful Terminal Shortcuts

These are useful for working efficiently in the terminal.

| Shortcut | Purpose |
|---|---|
| `!!` | Run the last command again |
| `!n` | Run command number `n` from history |
| `Ctrl + C` | Cancel/interrupt a running command |
| `Ctrl + L` | Clear terminal screen |

Example:

```bash
sudo apt update
```

If you need to run it again:

```bash
!!
```

---

# 21. Command Cheat Sheet

## Navigation

```bash
pwd
ls
ls -la
cd directory
cd ..
cd ~
cd /
mkdir directory
mkdir -p parent/child
```

## Files

```bash
touch file.txt
touch data{1..10}

cat file.txt
cat > file.txt
cat >> file.txt

nano file.txt
vim file.txt
```

## Copy / Move / Remove

```bash
cp file.txt destination/
cp -r source/ destination/
cp source/* destination/
cp -r source/* destination/

mv old.txt new.txt
mv file.txt directory/

rm file.txt
rm -r directory/
rm -rf directory/
```

## Viewing / Searching

```bash
cat file.txt
less file.txt
more file.txt
head -n 10 file.txt
tail -n 100 file.txt
grep ERROR file.txt
```

## System

```bash
whoami
hostname
uname -a
uptime
date
history
free
nproc
df -h
du -sh directory/
top
```

## Processes / Services

```bash
ps aux
top
kill PID
kill -9 PID

systemctl status service
systemctl restart service
```

## Networking

```bash
ping google.com
ip a
ifconfig
netstat -tulnp
curl URL
wget URL
```

## Permissions

```bash
chmod 755 file
chown user:group file
sudo command
```

## Packages

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install package -y
sudo yum install package -y
```

## Users

```bash
adduser user
useradd user
usermod ...
passwd user
id user
groups user
deluser user
who
w
last
```

## Scheduling

```bash
crontab -e
nohup command &
```

---

# 22. Core Mental Model

```text
                         LINUX COMMANDS
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
   File System            System                 Network
        │                     │                     │
   ┌────┼────┐          ┌─────┼─────┐         ┌────┼────┐
   │    │    │          │     │     │         │    │    │
  ls   cd  mkdir       ps   top  systemctl   ping  ip  curl
   │
   ├── File operations
   │      │
   │      ├── touch → create
   │      ├── cat   → read/write
   │      ├── nano  → edit
   │      ├── vim   → advanced edit
   │      ├── cp    → copy
   │      ├── mv    → move/rename
   │      └── rm    → remove
   │
   └── Information
          │
          ├── pwd
          ├── uname
          ├── df / du
          └── free
```

## The most important distinctions

```text
pwd
 ↓
Where am I?

ls
 ↓
What is here?

cd
 ↓
Move somewhere else.

mkdir
 ↓
Create a directory.

touch
 ↓
Create an empty file.

cat
 ↓
Read/write file contents.

nano / vim
 ↓
Edit a file.

cp
 ↓
Make a copy.

mv
 ↓
Move or rename.

rm
 ↓
Delete.

sudo
 ↓
Run a command with elevated privileges.

man
 ↓
Read the command's documentation.
```

---

# 23. Practical Example

Suppose you want to create a project structure:

```text
project/
├── src/
│   ├── app.py
│   └── config.py
├── data/
│   ├── data1
│   ├── data2
│   └── data3
└── README.md
```

Commands:

```bash
mkdir project
cd project

mkdir src data

touch src/app.py src/config.py
touch data/data{1..3}

touch README.md

ls
ls src
ls data
```

Edit a file:

```bash
nano README.md
```

View it:

```bash
cat README.md
```

Copy it:

```bash
cp README.md README_backup.md
```

Move/rename it:

```bash
mv README_backup.md backup.md
```

Check your location:

```bash
pwd
```

Inspect the command documentation whenever needed:

```bash
man cp
man touch
man ls
```

---

# 24. One-Page Mental Cheat Sheet

```text
NAVIGATION
pwd                 Where am I?
ls                  What's here?
ls child            What's inside child?
cd child            Enter child
cd ..               Go to parent
cd ~                Go home
cd /                Go to root
mkdir dir           Create directory

FILES
touch file          Create empty file
touch data{1..10}   Create 10 files
cat file            Read file
cat > file          Write/replace content
cat >> file         Append content
nano file            Edit simply
vim file             Edit with Vim

COPY / MOVE / DELETE
cp file dest/       Copy file
cp -r dir dest/     Copy directory
cp dir/* dest/      Copy directory contents
mv old new           Move/rename
rm file              Delete file
rm -r dir            Delete directory
rm -rf dir           Force delete recursively

HELP
man command          Read manual

PRIVILEGES
sudo command         Run with elevated privileges

VIEW / SEARCH
less file            View large file
head file            Beginning
tail file            End
grep text file       Search

SYSTEM
whoami               Current user
hostname             Hostname
uname -a             System/kernel info
uptime               Uptime
free                 Memory
nproc                CPU count
df -h                Disk/filesystem usage
du -sh dir            Directory size
top                  Processes/resources

PROCESS / SERVICE
ps aux               Processes
kill PID             Terminate process
systemctl status     Service status
systemctl restart    Restart service

NETWORK
ping host             Connectivity
ip a                  Network/IP info
netstat -tulnp        Connections/ports
curl URL              Fetch URL
wget URL              Download

PERMISSIONS
chmod 755 file        Change permissions
chown user:group file Change ownership

PACKAGE
sudo apt update
sudo apt upgrade -y
sudo apt install pkg -y

SHORTCUTS
!!                   Repeat last command
!n                   Run history command n
Ctrl+C               Interrupt
Ctrl+L               Clear screen
```

---

> **Source note:** This guide incorporates the commands and organization from the supplied Linux basic commands reference, while also preserving the file-operation examples and explanations developed in the accompanying notes. The source covers file/directory commands, viewing/search, processes, networking, permissions, package management, storage, scheduling, users, system information, and terminal shortcuts.
