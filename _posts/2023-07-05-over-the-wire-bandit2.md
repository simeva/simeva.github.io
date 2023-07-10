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

### Level 13>14

Task: The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on.

> `ssh --help` listed how to use ssh with an `identity` file (private key).

```console
bandit13@bandit:~$ ls
sshkey.private
ssh -i /home/bandit13/sshkey.private  bandit14@bandit.labs.overthewire.org -p2220
The authenticity of host '[bandit.labs.overthewire.org]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit13/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit13/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

!!! You are trying to log into this SSH server with a password on port 2220 from localhost.
!!! Connecting from localhost is blocked to conserve resources.
!!! Please log out and log in again.


      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!

If you find any problems, please report them to the #wargames channel on
discord or IRC.

--[ Playing the games ]--

  This machine might hold several wargames.
  If you are playing "somegame", then:

    * USERNAMES are somegame0, somegame1, ...
    * Most LEVELS are stored in /somegame/.
    * PASSWORDS for each level are stored in /etc/somegame_pass/.

  Write-access to homedirectories is disabled. It is advised to create a
  working directory with a hard-to-guess name in /tmp/.  You can use the
  command "mktemp -d" in order to generate a random and hard to guess
  directory in /tmp/.  Read-access to both /tmp/ is disabled and to /proc
  restricted so that users cannot snoop on eachother. Files and directories
  with easily guessable or short names will be periodically deleted! The /tmp
  directory is regularly wiped.
  Please play nice:

    * don't leave orphan processes running
    * don't leave exploit-files laying around
    * don't annoy other players
    * don't post passwords or spoilers
    * again, DONT POST SPOILERS!
      This includes writeups of your solution on your blog or website!

--[ Tips ]--

  This machine has a 64bit processor and many security-features enabled
  by default, although ASLR has been switched off.  The following
  compiler flags might be interesting:

    -m32                    compile for 32bit
    -fno-stack-protector    disable ProPolice
    -Wl,-z,norelro          disable relro

  In addition, the execstack tool can be used to flag the stack as
  executable on ELF binaries.

  Finally, network-access is limited for most levels by a local
  firewall.

--[ Tools ]--

 For your convenience we have installed a few useful tools which you can find
 in the following locations:

    * gef (https://github.com/hugsy/gef) in /opt/gef/
    * pwndbg (https://github.com/pwndbg/pwndbg) in /opt/pwndbg/
    * peda (https://github.com/longld/peda.git) in /opt/peda/
    * gdbinit (https://github.com/gdbinit/Gdbinit) in /opt/gdbinit/
    * pwntools (https://github.com/Gallopsled/pwntools)
    * radare2 (http://www.radare.org/)

 Both python2 and python3 are installed.

--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

bandit14@bandit:~$
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
```

### Level 14>15

Task: The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

> `Telnet` to `localhost`, enter password from prev level. Success!

```console
bandit14@bandit:~$ telnet localhost 30000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
fGrHPx402xGC7U7rXKDaxiWFTOiF0ENq
Correct!
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt

Connection closed by foreign host.
bandit14@bandit:~$
```

