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
