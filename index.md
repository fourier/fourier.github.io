# Notes on compiling "modern" software on Zaurus 

## Introduction

### How to install ROM
- Place files:
  1. zImage-2.4.20.bin
  1. hdimage-base.tgz
  1. initrd.bin
  1. updater-tools.bin
  1. updater.sh
  
  to the root directory of the SD/CF card (formatted to FAT32)
- Unplug Zaurus from the power outlet, check what it is not charging
- Take away the battery for 5 seconds
- Insert a battery, close the cover (move the slider towards the battery)
- Press OK on a keyboard and holding it press the power on key. Keep OK pressed until the Zaurus is running.
- You will see the Japaneese menu
- Plug zaurus to the power outlet.
- Select the 4th item, and there select eithe CF or SD(depending on there do you have your files)
- Don't forget to *not to use* entire disk, but default drives layout, otherwise there will be no swap, and the gcc will not be able co compile larger projects (like GNU APL)
- Install the softare
- Reboot

### Notes

Given: Zaurus C3000 with installed [pdaXii3](http://www.users.on.net/~hluc/myZaurus/pdaxii13.html)


## Software
### Feeds

Edit the /etc/ipkg.conf and add there paths to feeds on SD/CF card.
Warning: install from the feed using commands like
```
ipkg install gcc
```
is a preferred way over ```ipkg install /mnt/card/something.ipk``` since it will pick up dependencies automatically!

Install from the feed:
- gcc (it will install binutils as a dependency)
- gcc-headers
- libgcc

Install from capnfish-feed:
- mrxvt

### Fonts
- Copy fonts (ttf files) to ```/usr/X11R6/lib/X11/fonts/TTF/```
- Run ```fc-cache```

### Matchbox configuration
- All applets on a lower panel are configurable via **~/.matchbox/mbdock.session** file.

## Compiling software
- Install gcc from the feed
- Install awk from the feed
- Install sed from the feed
- Install make from the feed

### Git

Using git 2.11.0

1. Need to patch Makefile. Find the line 

  ```
  BASIC_CFLAGS += -DHAVE_CLOCK_MONOTONIC
  ```
  
  and comment it out.

2. Compiling with the following command line

  ```
  NO_NSEC=1 NO_CURL=1 NO_PERL_MAKEMAKER=1 NO_PERL=1 NO_TCLTK=1 NO_GETTEXT=1 NO_REGEX=1 make prefix=/usr/local
  ```

3. Install using 

  ```
  NO_NSEC=1 NO_CURL=1 NO_PERL_MAKEMAKER=1 NO_PERL=1 NO_TCLTK=1 NO_GETTEXT=1 NO_REGEX=1 make prefix=/usr/local install
  ```


### GNU APL
Taken the latest from SVN.

Configure:

```
./configure MAKE_J=1 CXX_WERROR=no --without-sqlite3 --without-postrgesql 
```
