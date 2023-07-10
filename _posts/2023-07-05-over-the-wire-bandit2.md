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

> Useful reading to get [Heartbeating](https://www.feistyduck.com/library/openssl-cookbook/online/ch-testing-with-openssl.html)

```console
bandit15@bandit:~$ openssl s_client -connect localhost:30001 -tlsextdebug
CONNECTED(00000003)
TLS server extension "supported versions" (id=43), len=2
0000 - 03 04                                             ..
TLS server extension "key share" (id=51), len=36
0000 - 00 1d 00 20 d2 9b 31 d3-55 46 20 48 0b a8 e1 98   ... ..1.UF H....
0010 - fe db d1 a8 5b 21 f9 63-fb 8a 05 7c 1d 06 02 3a   ....[!.c...|...:
0020 - a3 ac 34 55                                       ..4U
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
    Session-ID: E5F25A6253F8FD8D7928758B0A9B92252C8D32CF655FCEA18D6DD1AE86A53EDF
    Session-ID-ctx:
    Resumption PSK: 67249ADDF35D63C9089F8109D73CCD21A2ACC34346DBCE0413CF9976A778C2AA5D816E202290C3CADD673657A98CBF2C
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 14 86 7b aa 8e 49 1f 34-00 fa 3a 28 9a bf 26 71   ..{..I.4..:(..&q
    0010 - 0d 9a e1 21 9b 77 e5 a3-45 9a d8 c4 07 7a 52 6b   ...!.w..E....zRk
    0020 - b3 92 1f 97 d1 f2 32 a7-e4 b0 b4 94 84 e4 50 fd   ......2.......P.
    0030 - ac f6 fb bd 43 30 1b 23-0f 71 69 12 eb 9e 7c af   ....C0.#.qi...|.
    0040 - 11 c9 6f b1 2c 7c 04 89-ca 8e 7b 2e a2 c8 3f 02   ..o.,|....{...?.
    0050 - f0 26 a7 eb 3f 94 d7 93-a2 9a b4 85 1f 34 51 52   .&..?........4QR
    0060 - 17 74 29 0a 66 b5 8d 74-40 b0 9d 60 58 be 42 ff   .t).f..t@..`X.B.
    0070 - 9d 70 65 9d a6 91 5d 26-b1 a3 9a 17 0e 29 9b e1   .pe...]&.....)..
    0080 - b2 1b ae 33 43 c5 de 3c-6d d5 41 64 19 bb 39 3b   ...3C..<m.Ad..9;
    0090 - 6c 75 49 7f db 7b 5f 15-9b 45 9b 85 2e f2 55 55   luI..{_..E....UU
    00a0 - 5b d0 9b 6e 45 3b f3 a2-1d 27 63 a0 76 f6 1b 4a   [..nE;...'c.v..J
    00b0 - 60 26 2f 01 3a 54 39 76-17 c1 70 ce 08 80 85 4f   `&/.:T9v..p....O
    00c0 - fb 17 0d bb 2c 7a e1 f5-9e 64 39 ca a2 80 1d 37   ....,z...d9....7

    Start Time: 1688986283
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
    Session-ID: E52653D7C78DC2A76768EBED58ACF76B4F080ED4AB21AE570A36853A6CC2CEFB
    Session-ID-ctx:
    Resumption PSK: BF8C63B03A9B10EBEAEBD032F055595F1A58C4355C2069CC89F8C12FDBB3BDC97CD40F877973267578E6D34D354395E5
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 14 86 7b aa 8e 49 1f 34-00 fa 3a 28 9a bf 26 71   ..{..I.4..:(..&q
    0010 - 99 ca 3f 72 69 50 f9 d8-ba 21 9f 17 a8 e0 9d f1   ..?riP...!......
    0020 - 0c 5d 11 da a4 be f6 f8-98 8a e0 63 4f 76 6a 4c   .].........cOvjL
    0030 - df 47 19 b1 be 0f e1 68-fe 09 42 59 b6 85 9d b0   .G.....h..BY....
    0040 - a9 38 29 6b 9d 92 7f 78-1c 1a 7c fa 98 e7 de f6   .8)k...x..|.....
    0050 - 3f 1b 65 7e fd e1 97 c5-0d 5b ce 69 dc 32 57 ac   ?.e~.....[.i.2W.
    0060 - 91 d7 61 d0 81 b7 d5 e9-94 01 16 9b 5d 1d ac 9b   ..a.........]...
    0070 - 28 ca 50 9a db d3 e6 81-fc 6d 9e 26 35 41 67 7d   (.P......m.&5Ag}
    0080 - db 1d a4 42 eb 59 5c 19-f9 db 1e 8f c0 a2 4c 5b   ...B.Y\.......L[
    0090 - 9a 1f 7f 91 ff 2d 5f c8-00 27 b8 a9 c3 c7 a9 60   .....-_..'.....`
    00a0 - 7d 1c 90 9a 7f c3 f7 fc-ba b9 eb 8b 89 c2 aa 86   }...............
    00b0 - 4d 84 07 2f 99 50 2d 96-2d 0e 2f 86 73 13 10 9c   M../.P-.-./.s...
    00c0 - 51 c2 87 e4 88 05 08 a1-95 98 25 45 07 52 be ea   Q.........%E.R..

    Start Time: 1688986283
    Timeout   : 7200 (sec)
    Verify return code: 10 (certificate has expired)
    Extended master secret: no
    Max Early Data: 0
---
read R BLOCK
-ign_eof
Wrong! Please enter the correct current password
closed
bandit15@bandit:~$ openssl s_client -connect localhost:30001 -tlsextdebug
CONNECTED(00000003)
TLS server extension "supported versions" (id=43), len=2
0000 - 03 04                                             ..
TLS server extension "key share" (id=51), len=36
0000 - 00 1d 00 20 b4 a1 0a 0b-48 fb e8 40 de dd 34 7d   ... ....H..@..4}
0010 - 3b 57 a3 b3 f6 0d 1a ea-da 05 d8 2c ce 80 37 ea   ;W.........,..7.
0020 - 1c aa 27 4e                                       ..'N
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
    Session-ID: 57DA37E3B4DD90EF65EA26B64C8C42B391BCA95EC22E9AA868A9F589605F82E7
    Session-ID-ctx:
    Resumption PSK: B9E4020A42BC53B1E30DFA667DCC23DA3B45B342BEABA7033C311C3E6A7A01ABF028E3390CD7DA834F5E9A1A42440EEA
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 14 86 7b aa 8e 49 1f 34-00 fa 3a 28 9a bf 26 71   ..{..I.4..:(..&q
    0010 - 46 cc 13 54 04 05 a0 ab-35 67 d3 e1 74 2e 3d b9   F..T....5g..t.=.
    0020 - ac 49 ac 3b e2 45 4f b4-cd 0f ac 4d 22 47 f3 b4   .I.;.EO....M"G..
    0030 - dd 2b d8 0e f7 1d 51 3f-05 d3 13 2c 03 0c fd 6e   .+....Q?...,...n
    0040 - dd df 6b f8 0f 57 7b 52-b8 dd d2 f6 b8 f1 7a 0d   ..k..W{R......z.
    0050 - b1 78 5e e5 87 56 a2 52-e5 a1 a0 44 51 b8 87 d4   .x^..V.R...DQ...
    0060 - 45 01 cf 45 50 c7 35 47-0a 34 b9 1d 05 66 6f 16   E..EP.5G.4...fo.
    0070 - 1f b8 aa 3b 0a 14 9c 27-33 f1 a4 64 f6 b0 b9 19   ...;...'3..d....
    0080 - ba 3b 5e 12 85 10 4f 76-c4 8c 98 df a7 b4 30 e4   .;^...Ov......0.
    0090 - a3 32 61 e3 6c 14 5c 68-08 41 8c 56 af 32 df e0   .2a.l.\h.A.V.2..
    00a0 - dc 93 93 54 5f ed 5c 5a-05 3a 84 e9 8d d5 b8 e5   ...T_.\Z.:......
    00b0 - 65 32 0f 97 ae e9 ef d8-cc 4a c1 41 75 51 0f 3a   e2.......J.AuQ.:
    00c0 - 38 fe b1 9d 4c da 6a fb-9e a8 3e 96 27 f9 36 df   8...L.j...>.'.6.

    Start Time: 1688986368
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
    Session-ID: FC98FB2C01F8E1B138B9FFFB38858A28C682C894504963022E8B697D8F6DCCC1
    Session-ID-ctx:
    Resumption PSK: 006F820102AEAE4052EC79A368085F78D194AC8FF368933C0E2A82ACF172379691DAD62DC0E23CA113E3F70C666DC676
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 14 86 7b aa 8e 49 1f 34-00 fa 3a 28 9a bf 26 71   ..{..I.4..:(..&q
    0010 - 19 ac d7 e2 05 45 5b ab-eb d0 1c 0e 32 b3 fb 1d   .....E[.....2...
    0020 - ed 48 91 30 0f 9b a7 ef-57 af 08 ae c1 1a 3f 10   .H.0....W.....?.
    0030 - ec 3e 39 39 1c 3c c5 d6-42 d8 82 01 d1 c5 44 c8   .>99.<..B.....D.
    0040 - f4 1a eb 45 f6 89 23 88-c3 db 63 4c a3 39 1a 55   ...E..#...cL.9.U
    0050 - a8 5c d0 b5 dd 36 de 41-eb c3 80 d8 82 bc 88 ca   .\...6.A........
    0060 - 33 dc d6 05 90 f9 be bf-6d e6 eb 0f 90 a2 a7 ed   3.......m.......
    0070 - 1a a8 95 09 91 6a f7 54-38 df bf cb 00 92 64 bb   .....j.T8.....d.
    0080 - 18 92 a1 89 f9 8e a0 f8-8f 3f ba 8f 01 2e 73 a6   .........?....s.
    0090 - 07 88 d8 68 65 46 53 35-7d 77 a5 4f 85 23 cd af   ...heFS5}w.O.#..
    00a0 - 3f 7a 89 d0 08 70 d5 de-37 a4 94 81 43 6e ac 8f   ?z...p..7...Cn..
    00b0 - 81 98 0d 93 27 0d 96 0f-1f ab 7d a6 aa 36 d9 f0   ....'.....}..6..
    00c0 - 1b 64 30 c9 c7 0f bf f0-77 a1 d3 30 c7 bc d4 04   .d0.....w..0....

    Start Time: 1688986368
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

