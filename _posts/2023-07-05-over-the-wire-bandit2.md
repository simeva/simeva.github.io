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

### Level 15>16

Task: The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

> Using `s_client` to connect to port 30001

```console
bandit15@bandit:~$ openssl s_client -connect localhost:30001
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = localhost
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = localhost
verify error:num=10:certificate has expired
notAfter=Jul 10 09:52:40 2023 GMT
verify return:1
depth=0 CN = localhost
notAfter=Jul 10 09:52:40 2023 GMT
verify return:1
---
Certificate chain
 0 s:CN = localhost
   i:CN = localhost
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA1
   v:NotBefore: Jul 10 09:51:40 2023 GMT; NotAfter: Jul 10 09:52:40 2023 GMT
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIDCzCCAfOgAwIBAgIEL+b4pDANBgkqhkiG9w0BAQUFADAUMRIwEAYDVQQDDAls
b2NhbGhvc3QwHhcNMjMwNzEwMDk1MTQwWhcNMjMwNzEwMDk1MjQwWjAUMRIwEAYD
VQQDDAlsb2NhbGhvc3QwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC7
zPhKL7uaByqcxY7CccLZTYwCTT5snHLDVYfIDV7GfiqeXEuVcJmlTPvcGgTtTRzg
fFySEnLLhnq2r86zUGAKH1lv94hOEW3ltGtXArcI7j9sUiGbFmjjJJc3N3FzYN8M
bevDp/FFIjmt/UGHI57LmASFUXeKLCOBrcQKefSZEgq0VCr2mR2TBGyRX061rSVV
stI7uHpiEjvPh1AmLgP+N8/lfHiZf06xdB2igOpaBsrR7bsndn1BaBQRG1gXUmOJ
ZEsc9OAmVRlmB9IsMyz4NFTcIKRB+pRmUwjw6Ft1BGaGF/ueXmIVK+Zl53Z2/QwQ
suTLtCYVNaveOaP3pbmZAgMBAAGjZTBjMBQGA1UdEQQNMAuCCWxvY2FsaG9zdDBL
BglghkgBhvhCAQ0EPhY8QXV0b21hdGljYWxseSBnZW5lcmF0ZWQgYnkgTmNhdC4g
U2VlIGh0dHBzOi8vbm1hcC5vcmcvbmNhdC8uMA0GCSqGSIb3DQEBBQUAA4IBAQBv
07EGuWR/3YYeJ63COQAv6zv0o/OkW/izsCMDhCk+aAS7bsVeIzz4jJzVDThA5CKO
Gojxn63k0I81ledPChWqPdHbzEKi2hTw91oq7vKYjvn6scDge9lVAFwtqZhWfqOO
CCcX8LyA0yFv1AIZt9bWmNW3heks63eX2DnkdLTk0i76n4JMinxcLzc+AnBRyiYg
GXElWhVY4Wqaul5gYWXdUpHFO8NMOkSdUZnK70SlTR3aiAc+uC9zrUypuBXe2g08
OF5doCC9AoFMB1JoZtYRLkENfYqWm93oFpepbtxYk4rO+ocmkP2nEUtDiPT9c5Uk
nm0Vr6Tb23KHo7xYdkH9
-----END CERTIFICATE-----
subject=CN = localhost
issuer=CN = localhost
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 1339 bytes and written 373 bytes
Verification error: certificate has expired
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 2048 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 10 (certificate has expired)
---
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: 6D50A2619B671D70F9342EAA96C4B7B0B19521882307549F0F699A2E1F9A7C99
    Session-ID-ctx:
    Resumption PSK: 91A3168A97EB2B3964EE21AAA4B0098AD6F74F8BA7F2BAD45D83FC87F13DAA2A31A54149FB18949C7C921AC908470975
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 14 86 7b aa 8e 49 1f 34-00 fa 3a 28 9a bf 26 71   ..{..I.4..:(..&q
    0010 - c1 52 79 41 80 93 54 e2-39 af fa eb 43 62 25 55   .RyA..T.9...Cb%U
    0020 - df 98 1a f2 e5 bd 2c 0c-4b a6 dc d8 1b 46 59 24   ......,.K....FY$
    0030 - 18 91 5b 7c c6 c1 ff e5-e3 c9 2a 1b be a2 0a b2   ..[|......*.....
    0040 - e3 0d 3f 32 5d c3 d5 7f-f1 5e 40 11 14 9e 11 be   ..?2]....^@.....
    0050 - 54 aa cf 69 05 6b 5f dc-d3 c8 11 bc 32 3b 58 b5   T..i.k_.....2;X.
    0060 - cc ca 94 b1 55 68 6a fe-f0 10 24 e5 16 5b b4 1f   ....Uhj...$..[..
    0070 - ea a8 24 32 3e 98 4b ec-74 44 4e c6 23 02 e2 b0   ..$2>.K.tDN.#...
    0080 - af ae e7 18 fb 56 b7 e4-40 3a 42 8f e4 34 db a5   .....V..@:B..4..
    0090 - 15 4a c6 d8 1c a4 4e cb-81 c9 77 19 79 de c5 2c   .J....N...w.y..,
    00a0 - 89 2c 61 d6 e2 8d da e0-ea 97 ea 4b be f5 48 65   .,a........K..He
    00b0 - b9 ef 0b 5c 5f ea 4b 89-74 40 ed 78 d4 57 17 86   ...\_.K.t@.x.W..
    00c0 - fa ec 3f 63 0b 4e bb 2b-85 4a 57 6a 6b 4d 37 3e   ..?c.N.+.JWjkM7>

    Start Time: 1688986716
    Timeout   : 7200 (sec)
    Verify return code: 10 (certificate has expired)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
---
Post-Handshake New Session Ticket arrived:
SSL-Session:
    Protocol  : TLSv1.3
    Cipher    : TLS_AES_256_GCM_SHA384
    Session-ID: A314ED6C64168D98DABA6F35759279432AED1D1BFEF121E3696A9BF12493739C
    Session-ID-ctx:
    Resumption PSK: 2B4246D5CDA9D57638617CE4F2254B824E44EF3199759C66463535DB8CC961E148C2C4312D489FD18FA88DEA65D715E2
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 14 86 7b aa 8e 49 1f 34-00 fa 3a 28 9a bf 26 71   ..{..I.4..:(..&q
    0010 - ac 86 7a 1a 8b ab ef 99-51 e6 6a 3a 51 81 90 25   ..z.....Q.j:Q..%
    0020 - 18 f3 f3 63 af 17 b9 df-26 a1 66 b4 e0 8e 9d 4d   ...c....&.f....M
    0030 - 0a 2b ee 50 02 58 4b 19-ac d1 65 88 45 2a bc ab   .+.P.XK...e.E*..
    0040 - 4d 23 39 e2 47 0a 55 db-49 86 07 6f 9d a1 84 63   M#9.G.U.I..o...c
    0050 - f7 43 10 63 7d 5e c5 12-f2 a8 84 4e 48 6a 41 7c   .C.c}^.....NHjA|
    0060 - c8 f5 f7 d5 c9 9b f0 a9-f5 e2 42 ee 33 f9 f4 2f   ..........B.3../
    0070 - d9 d9 87 8d c7 ec cb 6d-4d dd c6 12 3e 3b d5 9d   .......mM...>;..
    0080 - 8e 52 db 20 ec 4d da 4d-f4 20 ce 1a 75 31 9b 94   .R. .M.M. ..u1..
    0090 - 1d 65 56 bd 0a 1e 92 6a-3f 4c 2a 2e 65 c5 1a 57   .eV....j?L*.e..W
    00a0 - be 64 3f d3 b9 b8 b8 dd-67 98 9b cb 20 1e e0 80   .d?.....g... ...
    00b0 - 70 40 cb da 0b 4e 12 8e-81 3c 40 6f 11 8c df 87   p@...N...<@o....
    00c0 - 9a 57 11 b1 68 11 ad 6e-75 dc 66 2a c7 ff 4a 0f   .W..h..nu.f*..J.

    Start Time: 1688986716
    Timeout   : 7200 (sec)
    Verify return code: 10 (certificate has expired)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt
Correct!
JQttfApK4SeyHwDlI9SXGR50qclOAil1

closed
bandit15@bandit:~$
```

