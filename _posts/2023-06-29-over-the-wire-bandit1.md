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


> List the current directory using `ls` and use the `cat` command to read the `readme file`


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






