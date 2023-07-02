---
layout: single
title: "OverTheWire - Bandit Solutions"
header:
  caption: "[__OverTheWire__](http://overthewire.org/wargames/)"
---

As part of my Cloud journey I am improving my Bash and Linux knowledge.
The [Overthewire](https://overthewire.org/wargames/) community have created some Wargames that are designed to help you learn and practice security concepts in a gameified way.  I am finding these very useful for practicing Bash and basic level Linux skills.

I am documenting my experience through the game/s here.

### Level 0

Task: SSH into the game using username: _bandit0_ and password: _bandit0_

```console

simeva% ssh bandit0@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit0@bandit.labs.overthewire.org's password:
```

### Level 0->1

Task: The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.\


> List the current directory using `ls` and use the `cat` command to read the `readme` file


```console
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
bandit0@bandit:~$`
```

### Level 1->2

Task: The password for the next level is stored in a file called - located in the home directory\

> To cd into a directory with `a` - as its name we need to alter the string that_ `cat` _sees so it doesn't treat it as a synonym for_ `stdin` _but instead still refers to a file called `-`

```console
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
rRGizSaX8Mk1RTb1CNQoXTcYZWU6lgzi
bandit1@bandit:~$
```

### Level 2->3

Task: The password for the next level is stored in a file called spaces in this filename located in the home directory

> There are a couple of ways to do this but in this example I have used the escape character between the words of the filename. Alternatively we could have used 2 x double quotes " " around the words.

```console
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename
aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG
bandit2@bandit:~$
```

### Level 3->4

Task: The password for the next level is stored in a hidden file in the inhere directory.

> After navigating to the inhere directory we need to view all files including hidden starting with a `.` Using the `ls -a` option we can do this.

```console
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls -a
.  ..  .hidden
bandit3@bandit:~/inhere$ cat .hidden
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe
bandit3@bandit:~/inhere$
```

### Level 4->5

Task: The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

> Using the `file` command with `./*` we can test all the files and dirs in the current directory

```console
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere/
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: Non-ISO extended-ASCII text, with no line terminators
bandit4@bandit:~/inhere$ cat ./-file07
lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR
bandit4@bandit:~/inhere$
```

### Level 5->6

Task: The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties: human-readable, 1033 bytes in size, not executable.

> By specifying a size test we can easily search for files with an exact file size 

```console
bandit5@bandit:~$ find -type f -size 1033c
./inhere/maybehere07/.file2
bandit5@bandit:~$ cat ./inhere/maybehere07/.file2
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU
bandit5@bandit:~/inhere$
```

### Level 6->7

Task: The password for the next level is stored somewhere on the server and has all of the following properties: owned by user bandit7, owned by group bandit6, 33 bytes in size

> Sending the find results to 2>/dev/null means that stderr (2 - containing error messages from the executed command or script) is redirected (>&) to stdout (1 - the output of the command) and that the latter is being redirected to /dev/null (the null device) 

```console
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -type f -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$
```














