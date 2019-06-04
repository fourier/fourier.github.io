---
layout: post
title:  "Import contacts from Google Contacts to Palm M125"
date:   2019-06-04 21:20:55 +0100
categories: lisp,palm,pda
tags: lisp,palm,pda
---
# Import contacts from Google Contacts to Palm M125

Out of curiosity decided to play with the old Palm M125 PDA.
It is the older PDA on Palm OS 4 with monochrome screen. Advantage of this PDA is that it is using 2 AAA batteries which could last a month.

Unsurprsingly even if the Windows Sync suite for this PDA doesn't work anymore on Windows 10, the Linux suite [pilot-link](https://www.tldp.org/HOWTO/PalmOS-HOWTO/pilotlink.html) works perfectly.

The first thing I decided to do is to import my contacts from the Android phone to this little device.

One of the tools of the **pilot-link** suite is called *pilot-address* and it allows to import CSV with contacts to the Palm device.

Usage (assuming **PILOTPORT** environment variable is correctly set, in my case it is **/dev/ttyUSB1**)
```
pilot-addresses -w addresses.csv
```
to download all contacts from the Palm OS device,
```
pilot-addresses --delete-all
```
to delete all the contacts,
```
pilot-addresses -r new-contacts.csv
```
to upload all the contacts from *new-contacts.csv* to the Palm OS device.

Typically contacts on Android phone synced to the owner's Google account, so the only task is to export them from Google Contacts as a CSV file, do the simple mapping of possible fields to the CSV suitable for the **pilot-addresses** tool, and upload them back.

The [converter](https://github.com/fourier/palm-gcontacts/)  has been implemented in one evening in Common Lisp.

The limitations is that the Palm OS is pre-unicode OS, so the contacts has to be converted to the text format with some coding page. I've decided to go with Cyrillic CP1251 as half of my contacts are spelled in cyrillic, and as some of my other contacts are in Swedish, to drop the accents from the letters of the Swedish alphabet upon conversion.

The implementation uses [cl-csv](https://github.com/AccelerationNet/cl-csv) to read-write CSV and [babel](https://github.com/cl-babel/babel) to decode between coding pages and UTF-8. The Makefile provided as with most of my tools to create command-line application, usage is on [its github page](https://github.com/fourier/palm-gcontacts/)
