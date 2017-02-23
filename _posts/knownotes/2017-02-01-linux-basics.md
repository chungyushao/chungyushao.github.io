---
layout: post
title: "Linux Directory Structure"
excerpt: ""
categories: knownotes
tags: [linux]
comments: true
share: true
author: chungyu
---

> From Udemy Course [Learn Linux in 5 Days and Level Up Your Career](https://www.udemy.com/learn-linux-in-5-days)


# The Linux Directory Structure

* `/`: The top of the  file system hierarchy.
* `/bin`: Binaries and other executable programs.
* `/etc`: System configuration files.
* `/home`: Home directories.
* `/tmp`: Temporary space, typically cleared on reboot.
* `/usr`: User related programs.
  * `/usr/local`: Locally installed software that is not part of the base OS.
* `/opt`: Optional or third party software.  
* `/var`: Variable data, most notably log files.
* `/boot`: Files needed to boot the operation systems
* `/cgroup`: Control Groups hierarchy
* `/dev`: Device files, typically controlled by the OS and the system administrators.
* `/export`: Shared file systems
* `/lib`: System libraries
* `/lib64`: System libraries, 64 bit
* `/lost+found`: Used by the file system to store recovered
* `/cdrom`: Mount point for CD-ROMs
* `/media`: Used to mount removable media like CD-ROMs
* `/mnt`: Used to mount external file systems
* `/proc`: provides info about running processes
* `/root`: the home directory for the root account
* `/sbin`: System administration binaries
* `/srv`: Contains data which is served by the system.
  * `/srv/www`: Web server files
  * `/srv/ftp`: FTP files
* `/sys`: Used to display and sometimes configure the devices known to the linux kernel

##### Common Application Directory Structure 1
* `/usr/local/some_app/bin`
* `/usr/local/some_app/etc`
* `/usr/local/some_app/lib`
* `/usr/local/some_app/log`

##### Common Application Directory Structure 2
* `/opt/some_app/bin`
* `/opt/some_app/etc`
* `/opt/some_app/lib`
* `/opt/some_app/log`

##### Common Application Directory Structure 3
* `/etc/opt/some_app`
* `/opt/some_app/bin`
* `/opt/some_app/lib`
* `/var/opt/some_app`

##### Common Application Directory Structure 4
* `/usr/local/bin/some_app`
* `/usr/local/etc/some_app.conf`
* `/usr/local/lib/libsome_app.so`


# Shell
##### What is Shell?
* The default interface to Linux
* A program that accepts your commands and executes those commands
* Also called a command line interpreter

# Root
* "Root" might refer to the Superuser or the top of the file system
* Root user is all powerful
* Normal accounts can only do a subset of the things root can do
* Root access is typically restricted to system administrators
* Root access may be required to to install, start, or stop an application
* Day to day activities will be performed using a normal account

# Man Pages
* `g`: Move to the top of the page.
* `G`: Move to the bottom of the page.
* `man -k SEARCH_TERM`: Searching man pages

# `ls`
* `ls -F`: reveal file types.
  * `/`: Directory
  * `@`: Link
  * `*`: Executable

# Permissions
![fp]({{site.url}}/images/linux/filepermission.png)

##### File Type Symbol
* `-`: Regular file
* `d`: Directory
* `|`: Symbolic link

---

##### Access Type Symbol
* `r`: Read
  * **file**: Allows a file to be read.
  * **directory**: Allows file names in the directory to be read.
* `w`: Write
  * **file**: Allows a file to modified.
  * **directory**: Allows entries to be modified within the directory.
* `x`: Execute
  * **file**: Allows the execution of a file.
  * **directory**: Allows access to contents and metadata for entries.

> Permission on a directory can affect the files in the directory. If the file permissions look correct, start checking directory permissions. Work your way up to the root.

##### Default File Creation Permissions
* `777` for directories
* `666` for files
* `umask` sets the file creation mask to the mode that u pass to it.
  * `chmod`: adding permissions
  * `umask`: subtracts permissions

--- | Directory | Files
:---|:---|:---
original|777|666
umask|022|022
result|755|644

---

##### Group Type Symbol
* `u`: User (Owner)
* `g`: Group
* `o`: Other
* `a`: All

##### Commonly Used Permissions
* `-rwx------`, `700`: full access to the file's owner, but not anyone else
* `-rwxr-xr-x`, `755`: everyone can execute the file, user in the same group can read the file, only the user/owner of the file can edit the file.
* `-rw-rw-r--`, `664`: a group of people to modify the file and let others read it
* `-rw-rw----`, `660`: allows a group of people to modify the file and not let others read it
* `-rw-r--r--`, `644`: everyone can read the file, only user/owner can modify the file

# Group
* Every user is in at least one group.
  * New files belong to the user's primary group
  * `chgrp`command change the group
* Users can belong to many groups.
* Groups are used to organize users.
* The `groups` or `id -Gn` command displays a user’s groups.

# Vi
* You are always working in one of three modes:
  * Command, Insert, Line

# Finding Files and Directory
##### `find [path] [expression]`
* Recursively finds files in path that match expression. If no arguments are supplied it find all files in the current directory.
* `-name pattern`: Find files and directories that match pattern.
* `-iname pattern`: Case-insensitive version
* `-mtime days`: Finds files that are days old.
* `-size num`: Finds file that are of size num.
* `-newer file`: Finds files that are newer than `file`.

##### `locate pattern`
* Lists files that match pattern; Faster than the `find` command.
* Results are not in real time. (Queries an index.)
  * May not be enabled on all systems in default.


# I/O
* File descriptors are just numbers that represent open files.

> 〝file descriptor〞 (fd) 簡單的說就是當 Unix-like 的 OS 在讀取檔案時會把每一檔案編號,此編號是 Kernel 用來追蹤 process 開啟檔案輸出/輸入的索引。

> 例如用瀏覽器瀏覽此文章時至少已開了20 個檔案(html 檔和多個圖檔),這些檔案會被編號 (例如 100,101,102 ....) 這些當檔案索引的數字就是 file descriptor (fd),但維護這些 file descriptor 是 Kernel 的事一般的使用者不用也不必知道。

> 有三個 fd 檔案是永遠開啟的那就是 stdin(鍵盤)、stdout(螢幕)、stderr(錯誤訊息)且 POSIX 標準保留這 3 個檔案的 fd 編號各為 0~2 供使用者操作重定向(redirection)的應用。

> 重定向實際上就是把三個永不關閉的 fd 0~2 重定向到其他地方(大部分為檔案或另一 fd ) 。

> ---[阿旺的 Linux 開竅手冊](http://wanggen.myweb.hinet.net/ech2/ech2.html)


##### Input/Output Types
I/O Name | Abbreviation | File Descriptor | Operator
:---|:---|:---|:---
Standard Input | stdin | 0 | `<` (replace), `<<` (append)
Standard Output | stdout | 1 | `>` (replace), `>>` (append)
Standard Error | stderr  | 2 | `2>` (replace), `2>>` (append)

* `2>&1`: Combine stderr and standard output
* `>/dev/null`: Redirect output to nowhere


# Comparing Files
```bash
$ diff file1 file2
3c3
< this is a line in a file.
---
> This is a Line in a File!
```
* LineNumFile1-Action-LineNumFile2
  * Action = (A)dd (C)hange (D)elete
  * `3c3`: File1's line 3 is changed from File2's line 3
* `<` Line from file1
* `>` Line from file2

##### `vimdiff`
* `Ctrl-w` then `w`: Go to next window
* `:q`: Quit (close current window)
* `:qa`: Quit all (close both files)
* `:qa!`: Force quit all

# Environment Variables
* Environment vars are name-value pairs.
* View with `printenv` and `echo`
* Create with `export VAR="value"`
* Delete with `unset VAR`
* "ENVIRONMENT" section in man page will show the effeteness of the environment variable

# Processes and Job

##### Listing process and info
* `ps`: Display process status.
* `pstree`: Display processes in a tree format.
* `top`: Interactive process viewer.
* `htop`: Interactive process viewer.

##### Background and Foreground Processes
* `command &`: Start command in background.
* `Ctrl-c`: Kill the foreground process.
* `Ctrl-z`: Suspend the foreground process.
* `bg [%num]`: Background a suspended process.
  * `num` can be seen using `jobs`
* `fg [%num]`: Foreground a background process.
* `kill`: Kill a process by job number or PID.
* `jobs [%num]`: List jobs.

##### Killing Processes
* `Ctrl-c`: Kills the foreground proc.
* `kill [-sig] pid`: Send a signal to a process.
* `kill -l`: Display a list of signals.
