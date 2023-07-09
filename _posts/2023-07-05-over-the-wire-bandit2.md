---
layout: single
title: "OverTheWire - Bandit Solutions [10 - 20]"
header:
  caption: "[__OverTheWire__](http://overthewire.org/wargames/)"
---

### Level 10>11

Task: The password for the next level is stored in the file `data.txt`, which contains `base64` encoded data

> Decoding using base64 command

```console
bandit10@bandit:~$ base64 data.txt -d
The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
bandit10@bandit:~$
```
### Level 11>12

Task: The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

> Using the `tr` command we can translate the data in data.txt using the ROT13 format.

```console
bandit11@bandit:~$ tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
bandit11@bandit:~$
```

### Level 12>13

Task: The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

> Tricky ! Exploring the different compression/decompresson tools.

```console
bandit12@bandit:~$ mkdir /tmp/sime/
bandit12@bandit:~$ cp data.txt /tmp/sime/
bandit12@bandit:~$ mv /tmp/sime/data.txt /tmp/sime/file.txt
bandit12@bandit:~$ cd /tmp/sime
bandit12@bandit:/tmp/sime$ ls
file.txt
bandit12@bandit:/tmp/sime$ xxd -r file.txt file.gz
bandit12@bandit:/tmp/sime$ file file.gz
file.gz: gzip compressed data, was "data2.bin", last modified: Sun Apr 23 18:04:23 2023, max compression, from Unix, original size modulo 2^32 581
bandit12@bandit:/tmp/sime$ gzip -d file.gz
bandit12@bandit:/tmp/sime$ file file
file: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/sime$ bzip2 -d file
bzip2: Can't guess original name for file -- using file.out
bandit12@bandit:/tmp/sime$ ls
file.out  file.txt
bandit12@bandit:/tmp/sime$ file file.out
file.out: gzip compressed data, was "data4.bin", last modified: Sun Apr 23 18:04:23 2023, max compression, from Unix, original size modulo 2^32 20480
bandit12@bandit:/tmp/sime$ mv file.out file.gz
bandit12@bandit:/tmp/sime$ file file
file: POSIX tar archive (GNU)
bandit12@bandit:/tmp/sime$ tar -xf file
bandit12@bandit:/tmp/sime$ ls
data5.bin  file  file.txt
bandit12@bandit:/tmp/sime$ ls
data5.bin  file  file.txt
bandit12@bandit:/tmp/sime$ file data5.bin
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/sime$ tar -xf data5.bin
bandit12@bandit:/tmp/sime$ ls
data5.bin  data6.bin  file  file.txt
bandit12@bandit:/tmp/sime$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/sime$ bzip2 -d data6.bin
bzip2: Can't guess original name for data6.bin -- using data6.bin.out
bandit12@bandit:/tmp/sime$ ls
data5.bin  data6.bin.out  file  file.txt
bandit12@bandit:/tmp/sime$ file data6.bin.out
data6.bin.out: POSIX tar archive (GNU)
bandit12@bandit:/tmp/sime$ tar -x-f data6.bin.out
tar: invalid option -- '-'
Try 'tar --help' or 'tar --usage' for more information.
bandit12@bandit:/tmp/sime$ tar -xf data6.bin.out
bandit12@bandit:/tmp/sime$ ls
data5.bin  data6.bin.out  data8.bin  file  file.txt
bandit12@bandit:/tmp/sime$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Sun Apr 23 18:04:23 2023, max compression, from Unix, original size modulo 2^32 49
bandit12@bandit:/tmp/sime$ gzip -d data8.bin
gzip: data8.bin: unknown suffix -- ignored
bandit12@bandit:/tmp/sime$ ls
data5.bin  data6.bin.out  data8.bin  file  file.txt
bandit12@bandit:/tmp/sime$ gzip -d data8.bin data8.gz
gzip: data8.bin: unknown suffix -- ignored
gzip: data8.gz: No such file or directory
bandit12@bandit:/tmp/sime$ ls
data5.bin  data6.bin.out  data8.bin  file  file.txt
bandit12@bandit:/tmp/sime$ mv data8.bin data8.gz
bandit12@bandit:/tmp/sime$ gzip -d data8.gz
bandit12@bandit:/tmp/sime$ ls
data5.bin  data6.bin.out  data8  file  file.txt
bandit12@bandit:/tmp/sime$ file data8
data8: ASCII text
bandit12@bandit:/tmp/sime$ cat data8
The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
bandit12@bandit:/tmp/sime$
```
