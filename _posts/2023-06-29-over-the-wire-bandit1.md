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

>_List the current directory using `ls` and use the `cat` command to read the_ readme _file_

```console
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL
bandit0@bandit:~$`
```
Password for next level spoiler\
! NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL 







