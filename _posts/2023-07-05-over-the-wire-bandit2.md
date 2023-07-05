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