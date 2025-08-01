
### CCT2019 - pcap1

This is a pcap-focused challenge originally created for the U.S. Navy Cyber Competition Team 2019 Assessment. Although the assessment is over, the created challenges are provided for community consumption here.

If you find the right clues, they will guide you to the next step. I did include some red herrings in this challenge, but you can stay on track by focusing on pcap-related skills.

HINT1: It's a pcap challenge. If you're doing stego or re, you're either down a rabbit hole or there's an easier way.

HINT2: It is very important to do the first step correctly. If you don't recover the first file in its entirety, you may not be able to complete steps later on in the challenge. The second pcap file has 4,588 packets in it. Contact me on Discord (send a DM to username zoobah) if you are struggling with this step.

HINT3: For the final step, the binary was built to run in an amd64 Kali Linux environment. If you are using a different Linux distro, you may run into some problems.
Find the flag.

```bash

┌──(root㉿kali)-[~/Downloads]
└─$ mv pcap2.pcapng CCT2019 
                                                                                                             
┌──(root㉿kali)-[~/Downloads]
└─$ cd CCT2019 
                                                                                                             
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ file pcap2.pcapng                          
pcap2.pcapng: pcapng capture file - version 1.0

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ wireshark pcap2.pcapng



 File encapsulation USB packets with USBPcap header
Number of packets 20
Data size 2308 kB
Start time 2019-07-21 02:39:27
File type pcap
End time 2019-07-21 02:39:27
Capture duration 0.114676 seconds 

usb.device_address==7 && usb.capdata

More precisely, the interesting data is stored in the field 'Leftover Capture Data', so we righ-click on it and select 'Apply as Column' so we can see it in the main Wireshark window.

We can perform the same filtering using 'tshark' from the command line, which may be useful in order to extract the packets later:

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ tshark -r pcap2.pcapng -Y 'usb.capdata and usb.device_address==7' -T text 
    1   0.000000         host → 1.7.1        USB 43 1000000001000910b20200002a000000 URB_BULK out
    2   0.020330        1.7.1 → host         USB 539 6338230002000910b2020000504b03041400000008002d0cef4eb789459ab537230084ae… URB_BULK in
    3   0.036774        1.7.1 → host         USB 261659 111a662d2f96d4957cddf5df0dc5b4b44b68152ba2a5fde5ef69c597ff0a5abef965c21e… URB_BULK in
    4   0.037098        1.7.1 → host         USB 539 cd2c6e3cd4c3d00a962cff13648557a445bc132ba5b1b45e5662002cbd89b98ad61cdc50… URB_BULK in
    5   0.046228        1.7.1 → host         USB 261659 67ba4ab5985dfd4cdc79d7af686c0c404a2e0ee3029b061f1709a7da04c1ae13fe2a495f… URB_BULK in
    6   0.046560        1.7.1 → host         USB 539 10c4c92934d6b325075034a04ac087f986b4576da9faf2ffb4b573a6c1dcae25f5aa94c9… URB_BULK in
    7   0.054979        1.7.1 → host         USB 261659 6bb480853b0e6a87710189bf57a25ef3d0dfd7b9e67c1d516b67c890f6ac7da5e15f9b14… URB_BULK in
    8   0.055346        1.7.1 → host         USB 539 0f8e910f52dc6b33b0c00cae65b205b3ffcc8397529058341714f74c6d4e20f943937c08… URB_BULK in
    9   0.064198        1.7.1 → host         USB 261659 cbaa12da6cc0b82a3de62146a10b9c21e6c34c963d46a7ccf8b995f25ad6f5c2e45b51cc… URB_BULK in
   10   0.064548        1.7.1 → host         USB 539 aa49b3ebbf79688de67f7a6834cf3f3c94a1ec9f1efaa7bfce49a1da3e40f35fc8d52461… URB_BULK in
   11   0.073199        1.7.1 → host         USB 261659 d53f7f28e322967f5e91bd17382c0387959482f85187e7733f3595f4e1b4a9478be9682f… URB_BULK in
   12   0.073556        1.7.1 → host         USB 539 f8442904977ff5ebbb2431529fcf95153a898b218c585dea56600a524b8720fa3eb75189… URB_BULK in
   13   0.082533        1.7.1 → host         USB 261659 9c391baa719ce9f6a8279e0d13b27ab576ff85db5b94bdb248bcedbde23e80204d2c53ac… URB_BULK in
   14   0.082962        1.7.1 → host         USB 539 e9001b80faeb8a2d6a07a957022efe0e00f7be76ae7903b205fcdfda0255e8972d8a242f… URB_BULK in
   15   0.091622        1.7.1 → host         USB 261659 44577232c4e863df407fd86ee56a70cc23431e45598acae8c082c8868d4918e16eb384fb… URB_BULK in
   16   0.092080        1.7.1 → host         USB 539 3d2259acbd15af0778728af2aa96e123ac1f307ef8312f4ef2c5ef1ddf1d7a001fa8b50f… URB_BULK in
   17   0.101070        1.7.1 → host         USB 261659 eed949b4af6a2e1697fe22250c1541517ed6a71937516e62f40e86a7a81f3f630fe5fc4a… URB_BULK in
   18   0.101616        1.7.1 → host         USB 539 e5162eabc3e193debff6fef719dc4cff3d23fa7ced02e79f6b1ca6ff77472684c3a17fb9… URB_BULK in
   19   0.109352        1.7.1 → host         USB 210558 ab34055930287c6c1d4f8e1fc38a374a3c095aa4e3089905a8bead4952e72824eb0f9e1d… URB_BULK in
   20   0.114676        1.7.1 → host         USB 39 0c00000003000120b2020000 URB_BULK in

The `-Y` option is a shorthand for the `--display-filter` option.
The `-T text` option is used to print the filtered packets in text format, which is a human-readable representation of the packet contents.


┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ tshark -r pcap2.pcapng -Y 'usb.capdata and usb.device_address==7' -T fields -e usb.capdata > raw 

-T: Set the format of the output when viewing decoded packet data ('fields' format).

-e: Add a field to the list of fields to display if -T fields is selected.


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ xxd -r -p raw  output2.bin

Options used:

-r: revert - reverse operation: convert (or patch) hexdump into binary

-p: plain hexdump style


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ stegoveritas output2.bin 
and finally use stegoveritas to unzip the pcap_chal.pcap

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ cd results 

┌──(root㉿kali)-[~/Downloads/CCT2019/results]
└─$ cd keepers  

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ wireshark pcap_chal.pcap 

and we have 4588 packets



wireshark
frame.len < 98 && icmp

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ tshark -r pcap_chal.pcap -Y 'frame.len lt 98 and icmp' -T fields -e data.data > raw 
                                                                                                                  
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat raw        
62726f2c207768617420796f7520757020746f3f
6e326d68
7768793f
796f75206469646e27742073656e642074686174207468696e6720796574
6f682e2e2e2077656c6c2c206e6f74206f7665722074686973
6966206e6f7420746869732c207468656e20776861743f
6c657427732075736520637279707463617420696e7374656164
616e6f74686572207468696e6720746f20696e7374616c6c3f
6d616e2e2e2e206e6f206f6e652063616e207365652074686973
7374696c6c2e2e2e207261746865722075736520656e6372797074696f6e
7765206e65656420746f207069636b2061206b657920746f20757365
49206b6e6f77206a75737420746865206f6e65
576861743f204f682c2074686174206f6c64207468696e673f
48616e67206f6e2c206c656d6d65206c6f6f6b206974207570
6f6b61792c204920666f756e642069742e2075736520746865206d65746173706c6f697420706f727420746f2072656365697665
6c697374656e65722069732075702e2073656e642069742e
6f6b61792c20697427732073656e74
3731383166346434356465303061653335623663663832303163386438353262
6861736820697320676f6f64



┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ xxd -r -p raw  output2.bin
                                                                                                                  
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat output2.bin 
bro, what you up to?n2mhwhy?you didn't send that thing yetoh... well, not over thisif not this, then what?let's use cryptcat insteadanother thing to install?man... no one can see thisstill... rather use encryptionwe need to pick a key to useI know just the oneWhat? Oh, that old thing?Hang on, lemme look it upokay, I found it. use the metasploit port to receivelistener is up. send it.okay, it's sent7181f4d45de00ae35b6cf8201c8d852bhash is good  

uhmm (cryptocat, metasploit port 4444?, hash, key)

wireshark expert information (Warning Response not found there's one message behind)

0000   00 50 56 33 4a 1d 00 0c 29 b7 69 63 08 00 45 00   .PV3J...).ic..E.
0010   00 5a 00 01 00 00 40 01 89 cb c0 a8 37 cb c0 a8   .Z....@.....7...
0020   37 bb 08 00 4b 47 7a 69 00 00 41 6e 67 65 6c 61   7...KGzi..Angela
0030   20 42 65 6e 6e 65 74 74 20 75 73 65 73 20 69 74    Bennett uses it
0040   20 74 6f 20 6c 6f 67 20 69 6e 74 6f 20 74 68 65    to log into the
0050   20 42 65 74 68 65 73 64 61 20 4e 61 76 61 6c 20    Bethesda Naval 
0060   48 6f 73 70 69 74 61 6c                           Hospital



Using her knowledge of the backdoor and a password found in Devlin's wallet, Angela logs into the Bethesda Naval Hospital's computers and learns that Under Secretary of Defense Bergstrom, who had opposed Gatekeeper's use by the federal government, was misdiagnosed. Fellow hacker "Cyberbob" connects 'Pi' with the "Praetorians," a notorious group of cyber-terrorists linked to recent computer failures around the country. Angela and Cyberbob plan to meet, but the unseen Praetorians intercept their online chat and relay it to Devlin who is now relentlessly pursuing her. Angela manages to escape from Devlin... who is now revealed to be a contract killer for the cyber-terrorists, but the Praetorians kill Champion by tampering with pharmacy and hospital computer records for his medication.


 MIN:SEC  |  TYPE  |  ACTION

 0 1:2 4  |  SYS   | SYSTEM IDLE
 0 2:1 3  |  Ae    | Exit System
 0 1:5 0  |  Key   | ^W (close)
 0 1:2 4  |  Ae    | Confirmation: message sent
 0 1:0 4  |  Key   | Praetorian <TAB> Suspect ANGEL
          |        | is near - advise immediately <CR>
          |        | Archangel
 0 0:4 6  |  Ae    | /TALK/
 0 0:3 9  |  Key   | /Password:/ natoar23ae
 0 0:1 9  |  Ae    | /LOGIN/
 0 0:0 6  |  Ae    | /TELNET/
 ? ?:? ?  |  ????T | Echo Enabled: Station 5E12

possible key : natoar23ae

wireshark search by http

GET /analysis.php?id=e7e47ecfd72c324519c9a72239cd2b399aaafc4b.9686&fmt=card HTTP/1.1
Host: fotoforensics.com
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:60.0) Gecko/20100101 Firefox/60.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Pragma: no-cache
Cache-Control: no-cache

HTTP/1.1 200 OK
Server: nginx
Date: Mon, 15 Jul 2019 04:44:56 GMT
Content-Type: image/png

https://fotoforensics.com/analysis.php?id=e7e47ecfd72c324519c9a72239cd2b399aaafc4b.9686&fmt=card

https://fotoforensics.com/analysis.php?id=e7e47ecfd72c324519c9a72239cd2b399aaafc4b.9686

Metadata 

Comment

Rar!..Ιs.ֽL֑e$.MV..'.?..ӻ..m.8A@^<.RsB6|l.@.$u�.@.n.,>Xuk !IU5.ֽL֑AA...|r�(.:Ӛ

let's download it

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ ls
1C.zip                                                 fakeflag.txt
7212.zip                                               flag.txt
archive.zip                                            flag.zip
cipher.txt                                             for1.jpg
config.txt                                             output2.bin
decrypt_enigma.py                                      pcap_chal.pcap
e7e47ecfd72c324519c9a72239cd2b399aaafc4b.9686-600.png  raw

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ file e7e47ecfd72c324519c9a72239cd2b399aaafc4b.9686-600.png 
e7e47ecfd72c324519c9a72239cd2b399aaafc4b.9686-600.png: PNG image data, 300 x 300, 8-bit/color RGB, non-interlaced

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ tshark -r pcap_chal.pcap -Y 'icmp.type == 8' -T fields -e data.data | cut -c -4 | xxd -r -p > data.bin
                                                                            
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ file data.bin 
data.bin: OpenPGP Secret Key

wireshark IRC

follow tcp

:irc.cct NOTICE Auth :*** Looking up your hostname...
CAP LS 302
PASS RedRoverRedRover$$
NICK zoobah
USER binaryphalanx 2 * :binaryphalanx
:irc.cct NOTICE Auth :*** Could not resolve your hostname: Request timed out; using your IP address (192.168.55.64) instead.
:irc.cct NOTICE Auth :Welcome to .Localnet.!
:irc.cct 001 zoobah :Welcome to the Localnet IRC Network zoobah!binaryphala@192.168.55.64
:irc.cct 002 zoobah :Your host is irc.cct, running version InspIRCd-2.0
:irc.cct 003 zoobah :This server was created on Debian
:irc.cct 004 zoobah irc.cct InspIRCd-2.0 iosw biklmnopstv bklov
:irc.cct 005 zoobah AWAYLEN=200 CASEMAPPING=rfc1459 CHANMODES=b,k,l,imnpst CHANNELLEN=64 CHANTYPES=# CHARSET=ascii ELIST=MU FNC KICKLEN=255 MAP MAXBANS=60 MAXCHANNELS=20 MAXPARA=32 :are supported by this server
:irc.cct 005 zoobah MAXTARGETS=20 MODES=20 NETWORK=Localnet NICKLEN=32 PREFIX=(ov)@+ STATUSMSG=@+ TOPICLEN=307 VBANLIST WALLCHOPS WALLVOICES :are supported by this server
:irc.cct 042 zoobah 108AAAAAC :your unique ID
:irc.cct 375 zoobah :irc.cct message of the day
:irc.cct 372 zoobah :- Welcome to the CCT Test Server for the PCAP assessment!
:irc.cct 376 zoobah :End of message of the day.
:irc.cct 251 zoobah :There are 1 users and 0 invisible on 1 servers
:irc.cct 254 zoobah 0 :channels formed
:irc.cct 255 zoobah :I have 1 clients and 0 servers
:irc.cct 265 zoobah :Current Local Users: 1  Max: 1
:irc.cct 266 zoobah :Current Global Users: 1  Max: 1

https://www.kali.org/tools/cryptcat/


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat data.bin | mcrypt -d -z
An OpenPGP encrypted file has been detected.
Enter passphrase: 
Stdin was NOT decrypted successfully.


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cryptcat -l -k natoar23ae -p 4444 < pcap_chal.pcap 

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cryptcat -k natoar23ae 10.8.19.103 4444 > file.pcap


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cryptcat -l -p 42
hello
hi
this is secret
yep
:)

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cryptcat 10.8.19.103 42  
hello
hi
this is secret
yep
:)

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cryptcat -k natoar23ae -lvp 4444                  
listening on [any] 4444 ...
hey
can u help me
10.8.19.103: inverse host lookup failed: Unknown host
connect to [10.8.19.103] from (UNKNOWN) [10.8.19.103] 44888
nope
why
:)

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cryptcat -v -k natoar23ae 10.8.19.103 4444
10.8.19.103: inverse host lookup failed: Unknown host
(UNKNOWN) [10.8.19.103] 4444 (?) open
nope
hey
can u help me
why
:)
^C punt!

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cryptcat -vv -k natoar23ae -l -p 4444
listening on [any] 4444 ...
10.8.19.103: inverse host lookup failed: Unknown host
connect to [10.8.19.103] from (UNKNOWN) [10.8.19.103] 51650
 sent 0, rcvd 0

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ nc -vv -w 1 10.8.19.103 4444 < data.bin 
10.8.19.103: inverse host lookup failed: Unknown host
(UNKNOWN) [10.8.19.103] 4444 (?) open
 sent 1824, rcvd 0

Data + key = encrypted data >>>> transfer >>>>>> encrypted data + key = data

the key is not natoar23ae I was wrong is BER5348833
reading again


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cryptcat -k BER5348833 -l -p 4444 > decrypted_file

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat data.bin | nc 10.8.19.103 4444  

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ file decrypted_file 
decrypted_file: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=10e9d33fce367a29cfe8d74866c3cb474ec61172, for GNU/Linux 3.2.0, stripped


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ md5sum decrypted_file                                 
7181f4d45de00ae35b6cf8201c8d852b  decrypted_file

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ ls
data.bin  decrypted_file  results
                                                                                                            
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ chmod +x decrypted_file                                    
                                                                                                            

┌──(root㉿kali)-[/Downloads/CCT2019/results/keepers]
└─# strace ./decrypted_file

execve("./decrypted_file", ["./decrypted_file"], 0x7ffd929bbf80 /* 32 vars */) = 0
brk(NULL)                               = 0x55f15638a000
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fd66b6c9000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
newfstatat(3, "", {st_mode=S_IFREG|0644, st_size=88866, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 88866, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fd66b6b3000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0Ps\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
newfstatat(3, "", {st_mode=S_IFREG|0755, st_size=1922136, ...}, AT_EMPTY_PATH) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
mmap(NULL, 1970000, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fd66b4d2000
mmap(0x7fd66b4f8000, 1396736, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x26000) = 0x7fd66b4f8000
mmap(0x7fd66b64d000, 339968, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x17b000) = 0x7fd66b64d000
mmap(0x7fd66b6a0000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1ce000) = 0x7fd66b6a0000
mmap(0x7fd66b6a6000, 53072, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fd66b6a6000
close(3)                                = 0
mmap(NULL, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fd66b4cf000
arch_prctl(ARCH_SET_FS, 0x7fd66b4cf740) = 0
set_tid_address(0x7fd66b4cfa10)         = 557137
set_robust_list(0x7fd66b4cfa20, 24)     = 0
rseq(0x7fd66b4d0060, 0x20, 0, 0x53053053) = 0
mprotect(0x7fd66b6a0000, 16384, PROT_READ) = 0
mprotect(0x55f154c86000, 4096, PROT_READ) = 0
mprotect(0x7fd66b6fb000, 8192, PROT_READ) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
munmap(0x7fd66b6b3000, 88866)           = 0
getrandom("\x75\x40\xfe\xf8\xc0\xe2\xb3\xc9", 8, GRND_NONBLOCK) = 8
brk(NULL)                               = 0x55f15638a000
brk(0x55f1563ab000)                     = 0x55f1563ab000
socket(AF_INET, SOCK_STREAM, IPPROTO_IP) = 3
newfstatat(AT_FDCWD, "/etc/resolv.conf", {st_mode=S_IFREG|0644, st_size=74, ...}, 0) = 0
openat(AT_FDCWD, "/etc/host.conf", O_RDONLY|O_CLOEXEC) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=9, ...}, AT_EMPTY_PATH) = 0
read(4, "multi on\n", 4096)             = 9
read(4, "", 4096)                       = 0
close(4)                                = 0
futex(0x7fd66b6ad42c, FUTEX_WAKE_PRIVATE, 2147483647) = 0
openat(AT_FDCWD, "/etc/resolv.conf", O_RDONLY|O_CLOEXEC) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=74, ...}, AT_EMPTY_PATH) = 0
read(4, "# Generated by NetworkManager\nse"..., 4096) = 74
read(4, "", 4096)                       = 0
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=74, ...}, AT_EMPTY_PATH) = 0
close(4)                                = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 4
connect(4, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(4)                                = 0
socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 4
connect(4, {sa_family=AF_UNIX, sun_path="/var/run/nscd/socket"}, 110) = -1 ENOENT (No such file or directory)
close(4)                                = 0
newfstatat(AT_FDCWD, "/etc/nsswitch.conf", {st_mode=S_IFREG|0644, st_size=558, ...}, 0) = 0
newfstatat(AT_FDCWD, "/", {st_mode=S_IFDIR|0755, st_size=4096, ...}, 0) = 0
openat(AT_FDCWD, "/etc/nsswitch.conf", O_RDONLY|O_CLOEXEC) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=558, ...}, AT_EMPTY_PATH) = 0
read(4, "# /etc/nsswitch.conf\n#\n# Example"..., 4096) = 558
read(4, "", 4096)                       = 0
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=558, ...}, AT_EMPTY_PATH) = 0
close(4)                                = 0
openat(AT_FDCWD, "/etc/hosts", O_RDONLY|O_CLOEXEC) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=198, ...}, AT_EMPTY_PATH) = 0
lseek(4, 0, SEEK_SET)                   = 0
read(4, "127.0.0.1\tlocalhost\n127.0.1.1\tka"..., 4096) = 198
read(4, "", 4096)                       = 0
close(4)                                = 0
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=88866, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 88866, PROT_READ, MAP_PRIVATE, 4, 0) = 0x7fd66b6b3000
close(4)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libnss_mdns4_minimal.so.2", O_RDONLY|O_CLOEXEC) = 4
read(4, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=18504, ...}, AT_EMPTY_PATH) = 0
mmap(NULL, 20496, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 4, 0) = 0x7fd66b4c9000
mmap(0x7fd66b4ca000, 8192, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0x1000) = 0x7fd66b4ca000
mmap(0x7fd66b4cc000, 4096, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0x3000) = 0x7fd66b4cc000
mmap(0x7fd66b4cd000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 4, 0x3000) = 0x7fd66b4cd000
close(4)                                = 0
mprotect(0x7fd66b4cd000, 4096, PROT_READ) = 0
munmap(0x7fd66b6b3000, 88866)           = 0
socket(AF_INET, SOCK_DGRAM|SOCK_CLOEXEC|SOCK_NONBLOCK, IPPROTO_IP) = 4
setsockopt(4, SOL_IP, IP_RECVERR, [1], 4) = 0
connect(4, {sa_family=AF_INET, sin_port=htons(53), sin_addr=inet_addr("192.168.253.2")}, 16) = 0
poll([{fd=4, events=POLLOUT}], 1, 0)    = 1 ([{fd=4, revents=POLLOUT}])
sendto(4, "\230\23\1\0\0\1\0\0\0\0\0\0\3irc\3cct\0\0\1\0\1", 25, MSG_NOSIGNAL, NULL, 0) = 25
poll([{fd=4, events=POLLIN}], 1, 5000)  = 1 ([{fd=4, revents=POLLIN}])
ioctl(4, FIONREAD, [25])                = 0
recvfrom(4, "\230\23\201\202\0\1\0\0\0\0\0\0\3irc\3cct\0\0\1\0\1", 1024, 0, {sa_family=AF_INET, sin_port=htons(53), sin_addr=inet_addr("192.168.253.2")}, [28 => 16]) = 25
close(4)                                = 0
socket(AF_INET, SOCK_DGRAM|SOCK_CLOEXEC|SOCK_NONBLOCK, IPPROTO_IP) = 4
setsockopt(4, SOL_IP, IP_RECVERR, [1], 4) = 0
connect(4, {sa_family=AF_INET, sin_port=htons(53), sin_addr=inet_addr("192.168.253.2")}, 16) = 0
poll([{fd=4, events=POLLOUT}], 1, 0)    = 1 ([{fd=4, revents=POLLOUT}])
sendto(4, "\230\23\1\0\0\1\0\0\0\0\0\0\3irc\3cct\0\0\1\0\1", 25, MSG_NOSIGNAL, NULL, 0) = 25
poll([{fd=4, events=POLLIN}], 1, 5000)  = 1 ([{fd=4, revents=POLLIN}])
ioctl(4, FIONREAD, [100])               = 0
recvfrom(4, "\230\23\201\203\0\1\0\0\0\1\0\0\3irc\3cct\0\0\1\0\1\0\0\6\0\1\0\0"..., 1024, 0, {sa_family=AF_INET, sin_port=htons(53), sin_addr=inet_addr("192.168.253.2")}, [28 => 16]) = 100
close(4)                                = 0
socket(AF_INET, SOCK_DGRAM|SOCK_CLOEXEC|SOCK_NONBLOCK, IPPROTO_IP) = 4
setsockopt(4, SOL_IP, IP_RECVERR, [1], 4) = 0
connect(4, {sa_family=AF_INET, sin_port=htons(53), sin_addr=inet_addr("192.168.253.2")}, 16) = 0
poll([{fd=4, events=POLLOUT}], 1, 0)    = 1 ([{fd=4, revents=POLLOUT}])
sendto(4, "\246R\1\0\0\1\0\0\0\0\0\0\3irc\3cct\vlocaldomain"..., 37, MSG_NOSIGNAL, NULL, 0) = 37
poll([{fd=4, events=POLLIN}], 1, 5000)  = 1 ([{fd=4, revents=POLLIN}])
ioctl(4, FIONREAD, [37])                = 0
recvfrom(4, "\246R\204\3\0\1\0\0\0\0\0\0\3irc\3cct\vlocaldomain"..., 1024, 0, {sa_family=AF_INET, sin_port=htons(53), sin_addr=inet_addr("192.168.253.2")}, [28 => 16]) = 37
close(4)                                = 0
newfstatat(1, "", {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0x5), ...}, AT_EMPTY_PATH) = 0
write(1, "ERROR No su\310\260v\246\376\177\n", 18ERROR No suȰv��
) = 18
exit_group(-1)                          = ?
+++ exited with 255 +++


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ sudo head /etc/inspircd/inspircd.conf
# This is just a more or less working example configuration file, please
# customize it for your needs!
#
# Once more: Please see the examples in /usr/share/doc/inspircd/examples/
# and the official InspIRCd docs at https://docs.inspircd.org/

<server name="irc.cct"
        description="Local IRC Server"
        network="localhost">

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ tail /etc/hosts
127.0.0.1	localhost
127.0.1.1	kali
::1		localhost ip6-localhost ip6-loopback
fe02::1		ip6-allnodes
fe02::2		ip6-allrouters

#10.10.188.193 lundc.lunar.eruca.com lundc lunar-LUNDC-CA lunar.eruca

127.0.0.1 irc.cct


┌──(root㉿kali)-[~/…/results/keepers/results/keepers]
└─$ ./elf                                                     
Found the server at 127.0.0.1
Connected to my server!
 NOTICE * :*** Looking up your hostname...
 NOTICE cct :*** Could not resolve your hostname: Request timed out; using your IP address (127.0.0.1) instead.
:irc.cct 001 cct :Welcome to the Localnet IRC Network cct!cct2019@127.0.0.1
:irc.cct 002 cct :Your host is irc.cct, running version InspIRCd-3
:irc.cct 003 cct :This server was created 11:34:32 Feb 20 2023
:irc.cct 004 cct irc.cct InspIRCd-3 iosw biklmnopstv :bklov
:irc.cct 005 cct AWAYLEN=200 CASEMAPPING=rfc1459 CHANLIMIT=#:20 CHANMODES=b,k,l,imnpst CHANNELLEN=64 CHANTYPES=# ELIST=CMNTU HOSTLEN=64 KEYLEN=32 KICKLEN=255 LINELEN=512 MAXLIST=b:100 :are supported by this server
:irc.cct 005 cct MAXTARGETS=20 MODES=20 NAMELEN=128 NETWORK=Localnet NICKLEN=30 PREFIX=(ov)@+ SAFELIST STATUSMSG=@+ TOPICLEN=307 USERLEN=10 USERMODES=,,s,iow WHOX :are supported by this server
:irc.cct 251 cct :There are 0 users and 2 invisible on 1 servers
:irc.cct 253 cct 1 :unknown connections
:irc.cct 254 cct 0 :channels formed
:irc.cct 255 cct :I have 2 clients and 0 servers
:irc.cct 265 cct :Current local users: 2  Max: 3
:irc.cct 266 cct :Current global users: 2  Max: 3
:irc.cct 375 cct :irc.cct message of the day
:irc.cct 372 cct :**************************************************
:irc.cct 372 cct :*             H    E    L    L    O              *
:irc.cct 372 cct :*  This is a private irc server. Please contact  *
:irc.cct 372 cct :*  the admin of the server for any questions or  *
:irc.cct 372 cct :*  issues.                                       *
:irc.cct 372 cct :**************************************************
:irc.cct 372 cct :*  The software was provided as a package of     *
:irc.cct 372 cct :*  Debian GNU/Linux <https://www.debian.org/>.   *
:irc.cct 372 cct :*  However, Debian has no control over this      *
:irc.cct 372 cct :*  server.                                       *
:irc.cct 372 cct :**************************************************
:irc.cct 372 cct :(The sysadmin possibly wants to edit </etc/inspircd/inspircd.motd>)
:irc.cct 376 cct :End of message of the day.
2019@127.0.0.1 JOIN :#flag
:irc.cct 353 cct = #flag :@cct
:irc.cct 366 cct #flag :End of /NAMES list.


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ irssi

 Irssi v1.4.3 - https://irssi.org                                                 
12:24 -!-  ___           _
12:24 -!- |_ _|_ _ _____(_)
12:24 -!-  | || '_(_-<_-< |
12:24 -!- |___|_| /__/__/_|
12:24 -!- Irssi v1.4.3 - https://irssi.org
12:24 -!- Irssi: The following settings were initialized
12:24                        real_name 
12:24                        user_name witty
12:24                             nick witty

 [12:25] [] [1]                                                                   
[(status)] /connect irc.cct

12:25 -!- **************************************************
12:25 -!- *  The software was provided as a package of     *
12:25 -!- *  Debian GNU/Linux <https://www.debian.org/>.   *
12:25 -!- *  However, Debian has no control over this      *
12:25 -!- *  server.                                       *
12:25 -!- **************************************************
12:25 -!- (The sysadmin possibly wants to edit </etc/inspircd/inspircd.motd>)
12:25 -!- End of message of the day.
12:25 -!- Mode change [+i] for user witty_
12:25 -!- Irssi: Your nick is in use by witty [witty@127.0.0.1]
 [12:25] [witty_(+i)] [1:rc (change with ^X)]                                     
[(status)] /join flag

then execute

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ ./elf
Found the server at 127.0.0.1
Connected to my server!
 NOTICE * :*** Looking up your hostname...
 NOTICE cct :*** Could not resolve your hostname: Request timed out; using your IP address (127.0.0.1) instead.
:irc.cct 001 cct :Welcome to the Localnet IRC Network cct!cct2019@127.0.0.1
:irc.cct 002 cct :Your host is irc.cct, running version InspIRCd-3
:irc.cct 003 cct :This server was created 11:34:32 Feb 20 2023
:irc.cct 004 cct irc.cct InspIRCd-3 iosw biklmnopstv :bklov
:irc.cct 005 cct AWAYLEN=200 CASEMAPPING=rfc1459 CHANLIMIT=#:20 CHANMODES=b,k,l,imnpst CHANNELLEN=64 CHANTYPES=# ELIST=CMNTU HOSTLEN=64 KEYLEN=32 KICKLEN=255 LINELEN=512 MAXLIST=b:100 :are supported by this server
:irc.cct 005 cct MAXTARGETS=20 MODES=20 NAMELEN=128 NETWORK=Localnet NICKLEN=30 PREFIX=(ov)@+ SAFELIST STATUSMSG=@+ TOPICLEN=307 USERLEN=10 USERMODES=,,s,iow WHOX :are supported by this server
:irc.cct 251 cct :There are 0 users and 1 invisible on 1 servers
:irc.cct 253 cct 1 :unknown connections
:irc.cct 254 cct 1 :channels formed
:irc.cct 255 cct :I have 1 clients and 0 servers
:irc.cct 265 cct :Current local users: 1  Max: 3
:irc.cct 266 cct :Current global users: 1  Max: 3
:irc.cct 375 cct :irc.cct message of the day
:irc.cct 372 cct :**************************************************
:irc.cct 372 cct :*             H    E    L    L    O              *
:irc.cct 372 cct :*  This is a private irc server. Please contact  *
:irc.cct 372 cct :*  the admin of the server for any questions or  *
:irc.cct 372 cct :*  issues.                                       *
:irc.cct 372 cct :**************************************************
:irc.cct 372 cct :*  The software was provided as a package of     *
:irc.cct 372 cct :*  Debian GNU/Linux <https://www.debian.org/>.   *
:irc.cct 372 cct :*  However, Debian has no control over this      *
:irc.cct 372 cct :*  server.                                       *
:irc.cct 372 cct :**************************************************
:irc.cct 372 cct :(The sysadmin possibly wants to edit </etc/inspircd/inspircd.motd>)
:irc.cct 376 cct :End of message of the day.
2019@127.0.0.1 JOIN :#flag
:irc.cct 353 cct = #flag :witty cct
:irc.cct 366 cct #flag :End of /NAMES list.

and wait the flag :)

12:19 [Users #flag]
12:19 -!- Irssi: #flag: Total of 2 nicks [1 ops, 0 halfops, 0 voices, 1 normal]
12:19 -!- Channel #flag created Mon Feb 20 12:17:20 2023
12:19 -!- Irssi: Join to #flag was synced in 8 secs
12:20 [Users #flag]
12:20 -!- Irssi: #flag: Total of 2 nicks [1 ops, 0 halfops, 0 voices, 1 normal]
End of Lastlog
12:22 < root> ?
12:22 -!- cct [cct2019@127.0.0.1] has joined #flag
12:22 < cct> CCT{flag}



```



### CCT2019 - re3

 Download Task Files

There's some kind of a high security lock blocking the way. Defeat the GUI to claim your key!

NOTE: The key is a 32-character hex blob and doesn't follow the CCT{.*} format. It'll be apparent when you've found it.

If you need a Windows machine to help reverse engineering this, please use the [Windows base room](https://tryhackme.com/room/windowsbase).

Answer the questions below

```

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ mv ../re3.exe re3.exe                                         
                                                                                  
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ ls
crypt1c.py
crypto1a_flag.txt
crypto1a.txt
crypto1a.zip
crypto1b_flag.txt
crypto1b.txt
crypto1b.zip
crypto1c.txt
crypto1.zip
encoder.py
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8
output2.bin
pcap2.pcapng
rail_decoder.py
raw
re3.exe
results
──(root㉿kali)-[~/Downloads/CCT2019]
└─$ file re3.exe       
re3.exe: PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows, 3 sections

──(root㉿kali)-[~/Downloads/CCT2019]
└─$ strings re3.exe 
!This program cannot be run in DOS mode.
.text
`.rsrc
@.reloc
cdacb
}\]V
@[R__
CR@@
rTRZ]
tZEV
@As7
lSystem.Resources.ResourceReader, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089#System.Resources.RuntimeResourceSet
PADPADP
lSystem.Resources.ResourceReader, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089#System.Resources.RuntimeResourceSet
PADPADP
BSJB
v4.0.30319
#Strings
#GUID
#Blob
<Module>
RE3.exe  
DotfuscatorAttribute
Settings
RE3  
.Properties
Resources
mscorlib
System
Attribute
System.Configuration
ApplicationSettingsBase
Object
System.Windows.Forms
Form
.ctor
get_A
get_C
get_Default
Default
System.Resources
ResourceManager
System.Globalization
CultureInfo
get_Culture
set_Culture
get_ResourceManager
Culture
Main
byteA
byteB
System.ComponentModel
IContainer
GroupBox
groupBox1
Button
checkButton
HScrollBar
bar1
bar2
bar3
bar4
Label
bar1Label
bar2Label
bar3Label
bar4Label
classA
EventArgs
eventHandlerA
ScrollEventArgs
scrollEventHandlerA
badBoy
eventHandlerB
scrollEventHandlerB
goodBoy
scrollEventHandlerC
scrollEventHandlerD
Dispose
value
disposing
System.Runtime.Versioning
TargetFrameworkAttribute
System.Runtime.CompilerServices
RuntimeCompatibilityAttribute
System.Reflection
AssemblyTitleAttribute
AssemblyDescriptionAttribute
AssemblyTrademarkAttribute
CompilationRelaxationsAttribute
AssemblyCompanyAttribute
AssemblyProductAttribute
AssemblyCopyrightAttribute
AssemblyConfigurationAttribute
System.Runtime.InteropServices
ComVisibleAttribute
GuidAttribute
AssemblyFileVersionAttribute
System.Diagnostics
DebuggableAttribute
DebuggingModes
RE3  
AttributeUsageAttribute
AttributeTargets
CompilerGeneratedAttribute
System.CodeDom.Compiler
GeneratedCodeAttribute
.cctor
SettingsBase
Synchronized
DebuggerNonUserCodeAttribute
ReferenceEquals
Type
RuntimeTypeHandle
GetTypeFromHandle
Assembly
get_Assembly
EditorBrowsableAttribute
EditorBrowsableState
STAThreadAttribute
Application
EnableVisualStyles
SetCompatibleTextRenderingDefault
Byte
<PrivateImplementationDetails>{82FD9487-036B-4066-83B5-473554084A3D}
ValueType
__StaticArrayInitTypeSize=32
$$method0x600000b-1
RuntimeHelpers
Array
RuntimeFieldHandle
InitializeArray
$$method0x600000b-2
Control
SuspendLayout
ControlCollection
get_Controls
System.Drawing
Point
set_Location
set_Name
Size
set_Size
set_TabIndex
set_TabStop
set_Text
set_AutoSize
ScrollEventHandler
ScrollBar
add_Scroll
ButtonBase
set_UseVisualStyleBackColor
EventHandler
add_Click
SizeF
ContainerControl
set_AutoScaleDimensions
AutoScaleMode
set_AutoScaleMode
set_ClientSize
add_Load
ResumeLayout
PerformLayout
get_Value
Clone
MessageBox
DialogResult
Show
Int32
ToString
System.Text
Encoding
get_ASCII
GetString
set_Maximum
IDisposable
RE3  
.Properties.Resources.resources
a.resources
KMicrosoft.VisualStudio.Editors.SettingsDesigner.SettingsSingleFileGenerator
10.0.0.0
3System.Resources.Tools.StronglyTypedResourceBuilder
4.0.0.0
.NETFramework,Version=v4.0
FrameworkDisplayName
.NET Framework 4
WrapNonExceptionThrows
RE3  
000:0:2:5.0.2300.0
%Navy Cyber Competition Team 2019     
$cd7ec975-154d-4ef3-bfd6-2983fc44ae86
1.0.0.0
RSDS
_CorExeMain
mscoree.dll
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
  <assemblyIdentity version="1.0.0.0" name="MyApplication.app"/>
  <trustInfo xmlns="urn:schemas-microsoft-com:asm.v2">
    <security>
      <requestedPrivileges xmlns="urn:schemas-microsoft-com:asm.v3">
        <requestedExecutionLevel level="asInvoker" uiAccess="false"/>
      </requestedPrivileges>
    </security>
  </trustInfo>
</assembly>
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ checksec --file=re3.exe
Error: Not an ELF file: re3.exe: PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows, 3 sections

Re3 --- Sliders (4 box 4 nums to check)


┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ ghidra re3.exe 

 we have the source code

// Decompiled with JetBrains decompiler
// Type: a
// Assembly: "RE3  ", Version=0.0.0.0, Culture=neutral, PublicKeyToken=null
// MVID: 82FD9487-036B-4066-83B5-473554084A3D
// Assembly location: C:\Users\User\Downloads\re3.exe

using System;
using System.ComponentModel;
using System.Drawing;
using System.Text;
using System.Windows.Forms;

public class a : Form
{
  private byte[] byteA = new byte[32]
  {
    (byte) 20,
    (byte) 22,
    (byte) 100,
    (byte) 23,
    (byte) 21,
    (byte) 99,
    (byte) 100,
    (byte) 97,
    (byte) 99,
    (byte) 98,
    (byte) 21,
    (byte) 97,
    (byte) 100,
    (byte) 97,
    (byte) 16,
    (byte) 21,
    (byte) 16,
    (byte) 23,
    (byte) 22,
    (byte) 17,
    (byte) 98,
    (byte) 21,
    (byte) 102,
    (byte) 16,
    (byte) 23,
    (byte) 18,
    (byte) 19,
    (byte) 101,
    (byte) 17,
    (byte) 99,
    (byte) 102,
    (byte) 18
  };
  private byte[] byteB = new byte[32]
  {
    (byte) 125,
    (byte) 92,
    (byte) 93,
    (byte) 86,
    (byte) 19,
    (byte) 64,
    (byte) 91,
    (byte) 82,
    (byte) 95,
    (byte) 95,
    (byte) 19,
    (byte) 67,
    (byte) 82,
    (byte) 64,
    (byte) 64,
    (byte) 18,
    (byte) 19,
    (byte) 114,
    (byte) 84,
    (byte) 82,
    (byte) 90,
    (byte) 93,
    (byte) 12,
    (byte) 19,
    (byte) 116,
    (byte) 90,
    (byte) 69,
    (byte) 86,
    (byte) 19,
    (byte) 70,
    (byte) 67,
    (byte) 12
  };
  private int c = 177;
  private int d = 51;
  private IContainer e;
  private GroupBox groupBox1;
  private Button checkButton;
  private HScrollBar bar1;
  private HScrollBar bar2;
  private HScrollBar bar3;
  private HScrollBar bar4;
  private Label bar1Label;
  private Label bar2Label;
  private Label bar3Label;
  private Label bar4Label;

  public a() => this.classA();

  private void classA()
  {
    this.groupBox1 = new GroupBox();
    this.bar1Label = new Label();
    this.bar2Label = new Label();
    this.bar3Label = new Label();
    this.bar4Label = new Label();
    this.bar1 = new HScrollBar();
    this.bar2 = new HScrollBar();
    this.bar3 = new HScrollBar();
    this.bar4 = new HScrollBar();
    this.checkButton = new Button();
    this.groupBox1.SuspendLayout();
    this.SuspendLayout();
    this.groupBox1.Controls.Add((Control) this.bar1Label);
    this.groupBox1.Controls.Add((Control) this.bar2Label);
    this.groupBox1.Controls.Add((Control) this.bar3Label);
    this.groupBox1.Controls.Add((Control) this.bar4Label);
    this.groupBox1.Controls.Add((Control) this.bar1);
    this.groupBox1.Controls.Add((Control) this.bar2);
    this.groupBox1.Controls.Add((Control) this.bar3);
    this.groupBox1.Controls.Add((Control) this.bar4);
    this.groupBox1.Location = new Point(12, 12);
    this.groupBox1.Name = "groupBox1";
    this.groupBox1.Size = new Size(713, 240);
    this.groupBox1.TabIndex = 0;
    this.groupBox1.TabStop = false;
    this.groupBox1.Text = "Sliders";
    this.bar1Label.AutoSize = true;
    this.bar1Label.Location = new Point(423, 31);
    this.bar1Label.Name = "valueText1";
    this.bar1Label.Size = new Size(11, 12);
    this.bar1Label.TabIndex = 3;
    this.bar1Label.Text = "0";
    this.bar2Label.AutoSize = true;
    this.bar2Label.Location = new Point(423, 68);
    this.bar2Label.Name = "valueText2";
    this.bar2Label.Size = new Size(11, 12);
    this.bar2Label.TabIndex = 4;
    this.bar2Label.Text = "0";
    this.bar3Label.AutoSize = true;
    this.bar3Label.Location = new Point(423, 106);
    this.bar3Label.Name = "valueText3";
    this.bar3Label.Size = new Size(11, 12);
    this.bar3Label.TabIndex = 5;
    this.bar3Label.Text = "0";
    this.bar4Label.AutoSize = true;
    this.bar4Label.Location = new Point(423, 148);
    this.bar4Label.Name = "valueText4";
    this.bar4Label.Size = new Size(11, 12);
    this.bar4Label.TabIndex = 5;
    this.bar4Label.Text = "0";
    this.bar1.Location = new Point(3, 26);
    this.bar1.Name = "valueBar1";
    this.bar1.Size = new Size(401, 17);
    this.bar1.TabIndex = 0;
    this.bar1.Scroll += new ScrollEventHandler(this.scrollEventHandlerA);
    this.bar2.Location = new Point(3, 63);
    this.bar2.Name = "valueBar2";
    this.bar2.Size = new Size(401, 17);
    this.bar2.TabIndex = 1;
    this.bar2.Scroll += new ScrollEventHandler(this.scrollEventHandlerB);
    this.bar3.Location = new Point(3, 101);
    this.bar3.Name = "valueBar3";
    this.bar3.Size = new Size(401, 17);
    this.bar3.TabIndex = 2;
    this.bar3.Scroll += new ScrollEventHandler(this.scrollEventHandlerC);
    this.bar4.Location = new Point(3, 143);
    this.bar4.Name = "valueBar4";
    this.bar4.Size = new Size(401, 17);
    this.bar4.TabIndex = 1;
    this.bar4.Scroll += new ScrollEventHandler(this.scrollEventHandlerD);
    this.checkButton.Location = new Point(394, 197);
    this.checkButton.Name = "checkButton";
    this.checkButton.Size = new Size(75, 23);
    this.checkButton.TabIndex = 1;
    this.checkButton.Text = "Check!";
    this.checkButton.UseVisualStyleBackColor = true;
    this.checkButton.Click += new EventHandler(this.eventHandlerA);
    this.AutoScaleDimensions = new SizeF(7f, 12f);
    this.AutoScaleMode = AutoScaleMode.Font;
    this.ClientSize = new Size(481, 243);
    this.Controls.Add((Control) this.checkButton);
    this.Controls.Add((Control) this.groupBox1);
    this.Name = "RE3  ";
    this.Text = "RE3  ";
    this.Load += new EventHandler(this.eventHandlerB);
    this.groupBox1.ResumeLayout(false);
    this.groupBox1.PerformLayout();
    this.ResumeLayout(false);
  }

  private void eventHandlerA(object A_0, EventArgs A_1)
  {
    int A_0_1 = this.bar1.Value;
    int A_1_1 = this.bar2.Value;
    int A_2 = this.bar3.Value;
    int A_4 = this.bar4.Value;
    int num1 = 10;
    int num2 = 24;
    if (A_0_1 + A_1_1 + A_2 + A_4 == num1 && A_0_1 * A_1_1 * A_2 * A_4 == num2 && A_0_1 > A_1_1 && A_1_1 > A_2 && A_2 > A_4)
    {
      int num3 = (int) MessageBox.Show(this.goodBoy(A_0_1, A_1_1, A_2, A_4, (byte[]) this.byteA.Clone()));
    }
    else
    {
      int num4 = (int) MessageBox.Show(this.badBoy(A_0_1, A_1_1, A_2, A_4, (byte[]) this.byteA.Clone()));
    }
  }

  private void scrollEventHandlerA(object A_0, ScrollEventArgs A_1) => this.bar1Label.Text = this.bar1.Value.ToString();

  private string badBoy(int A_0, int A_1, int A_2, int A_4, byte[] A_3)
  {
    for (int index = 0; index < A_3.Length; ++index)
      A_3[index] = (byte) ((uint) this.byteB[index] ^ (uint) this.d);
    return Encoding.ASCII.GetString(A_3);
  }

  private void eventHandlerB(object A_0, EventArgs A_1)
  {
    this.bar1.Maximum = 1032;
    this.bar2.Maximum = 1032;
    this.bar3.Maximum = 1032;
    this.bar4.Maximum = 1032;
  }

  private void scrollEventHandlerB(object A_0, ScrollEventArgs A_1) => this.bar2Label.Text = this.bar2.Value.ToString();

  private string goodBoy(int A_0, int A_1, int A_2, int A_4, byte[] A_3)
  {
    for (int index = 0; index < A_3.Length; ++index)
      A_3[index] = (byte) ((uint) A_3[index] ^ (uint) (byte) (this.c ^ A_1));
    return Encoding.ASCII.GetString(A_3);
  }

  private void scrollEventHandlerC(object A_0, ScrollEventArgs A_1) => this.bar3Label.Text = this.bar3.Value.ToString();

  private void scrollEventHandlerD(object A_0, ScrollEventArgs A_1) => this.bar4Label.Text = this.bar4.Value.ToString();

  protected override void Dispose(bool disposing)
  {
    if (disposing && this.e != null)
      this.e.Dispose();
    base.Dispose(disposing);
  }
}

In my  case I was trying to change values and compile but won't work (so the 4 sliders can be 4,3,2,1 😂)

int num1 = 10;
    int num2 = 24;

the real one is 

int num1 = 711;
int num2 = 711000000;

I u want to compile just erase Resources.cs and Settings.cs (like Osiris to compile ivpn.exe)

I was trying some scripts to know better

┌──(root㉿kali)-[~]
└─$ python3 fin1.py
None shall pass! Again? Give up?
                                                                                           
┌──(root㉿kali)-[~]
└─$ cat fin1.py    
byteB = bytearray([125, 92, 93, 86, 19, 64, 91, 82, 95, 95, 19, 67, 82, 64, 64, 18, 19, 114, 84, 82, 90, 93, 12, 19, 116, 90, 69, 86, 19, 70, 67, 12])
c = 177
d = 51

A_1 = 177  # Example value
A_3 = bytearray(len(byteB))

for index in range(len(byteB)):
    A_3[index] = byteB[index] ^ d ^ (c ^ A_1)

result = ""
for b in A_3:
    result += chr(b)

print(result)

┌──(root㉿kali)-[~]
└─$ python3 fin2.py
hjkiililkjmi�lknom�n
                                                                             
┌──(root㉿kali)-[~]
└─$ cat fin2.py 
byteB = bytearray([20, 22, 100, 23, 21, 99, 100, 97, 99, 98, 21, 97, 100, 97, 16, 21, 16, 23, 22, 17, 98, 21, 102, 16, 23, 18, 19, 101, 17, 99, 102, 18])
c = 177
d = 51

A_1 = 253  # Example value
A_3 = bytearray(len(byteB))

for index in range(len(byteB)):
    A_3[index] = byteB[index] ^ d ^ (c ^ A_1)

result = ""
for b in A_3:
    result += chr(b)

print(result)


┌──(root㉿kali)-[~]
└─$ python3 fin3.py
A_1=0: æáæãáàãæãàäçáäçàçâàáâçâáåàå
A_1=2: äãäáãâáäáâæåãæ
A_1=3: åâåàâãàåàçä
A_1=4: åâçåäçâçäàãåà
A_1=5: ãäãæäåæãæåáâäá
A_1=6: àçàåçæåàåæâáçâ
A_1=7: áçàæã
A_1=8: îïí
A_1=10: êëî
_1=12:     A_1=11: êíèêëèíèï
A_1=13: ëëîìíëîêïèíïîíèíêïîë
                            ö
ñöóñðóöó

ðô
÷ñô
A_1=17: 
÷
 ð÷òðñò÷ò

ñõ
  öðõ
A_1=18: 
        ô
óôñóò
ñôñ
ò
öõóö
A_1=19: 
õ
 òõðòó
      ðõð
         ó
          ÷ôò÷
A_1=20: òõò÷õô÷ò÷ôð

óõð

A_1=21: óôóöôõöóöõñ

òôñ

A_1=22: ð÷ðõ÷öõðõ
                           øþû
A_1=32: ¶´Æµ·ÁÆÃÁÀ·ÃÆÃ²·²µ´³À·Ä²µ°±Ç³ÁÄ°
A_1=33: ·µÇ´¶ÀÇÂÀÁ¶ÂÇÂ³¶³´µ²Á¶Å³´±°Æ²ÀÅ±
A_1=34: ´¶Ä·µÃÄÁÃÂµÁÄÁ°µ°·¶±ÂµÆ°·²³Å±ÃÆ²
A_1=35: µ·Å¶´ÂÅÀÂÃ´ÀÅÀ±´±¶·°Ã´Ç±¶³²Ä°ÂÇ³
A_1=36: ²°Â±³ÅÂÇÅÄ³ÇÂÇ¶³¶±°·Ä³À¶±´µÃ·ÅÀ´
A_1=37: ³±Ã°²ÄÃÆÄÅ²ÆÃÆ·²·°±¶Å²Á·°µ´Â¶ÄÁµ
A_1=38: °²À³±ÇÀÅÇÆ±ÅÀÅ´±´³²µÆ±Â´³¶·ÁµÇÂ¶
A_1=39: ±³Á²°ÆÁÄÆÇ°ÄÁÄµ°µ²³´Ç°Ãµ²·¶À´ÆÃ·
A_1=40: ¾¼Î½¿ÉÎËÉÈ¿ËÎËº¿º½¼»È¿Ìº½¸¹Ï»ÉÌ¸
A_1=41: ¿½Ï¼¾ÈÏÊÈÉ¾ÊÏÊ»¾»¼½ºÉ¾Í»¼¹¸ÎºÈÍ¹
A_1=42: ¼¾Ì¿½ËÌÉËÊ½ÉÌÉ¸½¸¿¾¹Ê½Î¸¿º»Í¹ËÎº
A_1=43: ½¿Í¾¼ÊÍÈÊË¼ÈÍÈ¹¼¹¾¿¸Ë¼Ï¹¾»ºÌ¸ÊÏ»
A_1=44: º¸Ê¹»ÍÊÏÍÌ»ÏÊÏ¾»¾¹¸¿Ì»È¾¹¼½Ë¿ÍÈ¼
A_1=45: »¹Ë¸ºÌËÎÌÍºÎËÎ¿º¿¸¹¾ÍºÉ¿¸½¼Ê¾ÌÉ½
A_1=46: ¸ºÈ»¹ÏÈÍÏÎ¹ÍÈÍ¼¹¼»º½Î¹Ê¼»¾¿É½ÏÊ¾
A_1=47: ¹»Éº¸ÎÉÌÎÏ¸ÌÉÌ½¸½º»¼Ï¸Ë½º¿¾È¼ÎË¿
A_1=48: ¦¤Ö¥§ÑÖÓÑÐ§ÓÖÓ¢§¢¥¤£Ð§Ô¢¥ ¡×£ÑÔ 
A_1=49: §¥×¤¦Ð×ÒÐÑ¦Ò×Ò£¦£¤¥¢Ñ¦Õ£¤¡ Ö¢ÐÕ¡
A_1=50: ¤¦Ô§¥ÓÔÑÓÒ¥ÑÔÑ ¥ §¦¡Ò¥Ö §¢£Õ¡ÓÖ¢
A_1=51: ¥§Õ¦¤ÒÕÐÒÓ¤ÐÕÐ¡¤¡¦§ Ó¤×¡¦£¢Ô Ò×£
A_1=52: ¢ Ò¡£ÕÒ×ÕÔ£×Ò×¦£¦¡ §Ô£Ð¦¡¤¥Ó§ÕÐ¤
A_1=53: £¡Ó ¢ÔÓÖÔÕ¢ÖÓÖ§¢§ ¡¦Õ¢Ñ§ ¥¤Ò¦ÔÑ¥
A_1=54:  ¢Ð£¡×ÐÕ×Ö¡ÕÐÕ¤¡¤£¢¥Ö¡Ò¤£¦§Ñ¥×Ò¦
A_1=55: ¡£Ñ¢ ÖÑÔÖ× ÔÑÔ¥ ¥¢£¤× Ó¥¢§¦Ð¤ÖÓ§
A_1=56: ®¬Þ­¯ÙÞÛÙØ¯ÛÞÛª¯ª­¬«Ø¯Üª­¨©ß«ÙÜ¨
A_1=57: ¯­ß¬®ØßÚØÙ®ÚßÚ«®«¬­ªÙ®Ý«¬©¨ÞªØÝ©
A_1=58: ¬®Ü¯­ÛÜÙÛÚ­ÙÜÙ¨­¨¯®©Ú­Þ¨¯ª«Ý©ÛÞª
A_1=59: ­¯Ý®¬ÚÝØÚÛ¬ØÝØ©¬©®¯¨Û¬ß©®«ªÜ¨Úß«
A_1=60: ª¨Ú©«ÝÚßÝÜ«ßÚß®«®©¨¯Ü«Ø®©¬­Û¯ÝØ¬
A_1=61: «©Û¨ªÜÛÞÜÝªÞÛÞ¯ª¯¨©®ÝªÙ¯¨­¬Ú®ÜÙ­
A_1=62: ¨ªØ«©ßØÝßÞ©ÝØÝ¬©¬«ª­Þ©Ú¬«®¯Ù­ßÚ®
A_1=63: ©«Ùª¨ÞÙÜÞß¨ÜÙÜ­¨­ª«¬ß¨Û­ª¯®Ø¬ÞÛ¯
A_1=64: ÖÔ¦Õ×¡¦£¡ ×£¦£Ò×ÒÕÔÓ ×¤ÒÕÐÑ§Ó¡¤Ð
A_1=65: ×Õ§ÔÖ §¢ ¡Ö¢§¢ÓÖÓÔÕÒ¡Ö¥ÓÔÑÐ¦Ò ¥Ñ
A_1=66: ÔÖ¤×Õ£¤¡£¢Õ¡¤¡ÐÕÐ×ÖÑ¢Õ¦Ð×ÒÓ¥Ñ£¦Ò
A_1=67: Õ×¥ÖÔ¢¥ ¢£Ô ¥ ÑÔÑÖ×Ð£Ô§ÑÖÓÒ¤Ð¢§Ó
A_1=68: ÒÐ¢ÑÓ¥¢§¥¤Ó§¢§ÖÓÖÑÐ×¤Ó ÖÑÔÕ£×¥ Ô
A_1=69: ÓÑ£ÐÒ¤£¦¤¥Ò¦£¦×Ò×ÐÑÖ¥Ò¡×ÐÕÔ¢Ö¤¡Õ
A_1=70: ÐÒ ÓÑ§ ¥§¦Ñ¥ ¥ÔÑÔÓÒÕ¦Ñ¢ÔÓÖ×¡Õ§¢Ö
A_1=71: ÑÓ¡ÒÐ¦¡¤¦§Ð¤¡¤ÕÐÕÒÓÔ§Ð£ÕÒ×Ö Ô¦£×
A_1=72: ÞÜ®Ýß©®«©¨ß«®«ÚßÚÝÜÛ¨ß¬ÚÝØÙ¯Û©¬Ø
A_1=73: ßÝ¯ÜÞ¨¯ª¨©Þª¯ªÛÞÛÜÝÚ©Þ­ÛÜÙØ®Ú¨­Ù
A_1=74: ÜÞ¬ßÝ«¬©«ªÝ©¬©ØÝØßÞÙªÝ®ØßÚÛ­Ù«®Ú
A_1=75: Ýß­ÞÜª­¨ª«Ü¨­¨ÙÜÙÞßØ«Ü¯ÙÞÛÚ¬Øª¯Û
A_1=76: ÚØªÙÛ­ª¯­¬Û¯ª¯ÞÛÞÙØß¬Û¨ÞÙÜÝ«ß­¨Ü
A_1=77: ÛÙ«ØÚ¬«®¬­Ú®«®ßÚßØÙÞ­Ú©ßØÝÜªÞ¬©Ý
A_1=78: ØÚ¨ÛÙ¯¨­¯®Ù­¨­ÜÙÜÛÚÝ®ÙªÜÛÞß©Ý¯ªÞ
A_1=79: ÙÛ©ÚØ®©¬®¯Ø¬©¬ÝØÝÚÛÜ¯Ø«ÝÚßÞ¨Ü®«ß
A_1=80: ÆÄ¶ÅÇ±¶³±°Ç³¶³ÂÇÂÅÄÃ°Ç´ÂÅÀÁ·Ã±´À
A_1=81: ÇÅ·ÄÆ°·²°±Æ²·²ÃÆÃÄÅÂ±ÆµÃÄÁÀ¶Â°µÁ
A_1=82: ÄÆ´ÇÅ³´±³²Å±´±ÀÅÀÇÆÁ²Å¶ÀÇÂÃµÁ³¶Â
A_1=83: ÅÇµÆÄ²µ°²³Ä°µ°ÁÄÁÆÇÀ³Ä·ÁÆÃÂ´À²·Ã
A_1=84: ÂÀ²ÁÃµ²·µ´Ã·²·ÆÃÆÁÀÇ´Ã°ÆÁÄÅ³Çµ°Ä
A_1=85: ÃÁ³ÀÂ´³¶´µÂ¶³¶ÇÂÇÀÁÆµÂ±ÇÀÅÄ²Æ´±Å
A_1=86: ÀÂ°ÃÁ·°µ·¶Áµ°µÄÁÄÃÂÅ¶Á²ÄÃÆÇ±Å·²Æ
A_1=87: ÁÃ±ÂÀ¶±´¶·À´±´ÅÀÅÂÃÄ·À³ÅÂÇÆ°Ä¶³Ç
A_1=88: ÎÌ¾ÍÏ¹¾»¹¸Ï»¾»ÊÏÊÍÌË¸Ï¼ÊÍÈÉ¿Ë¹¼È
A_1=89: ÏÍ¿ÌÎ¸¿º¸¹Îº¿ºËÎËÌÍÊ¹Î½ËÌÉÈ¾Ê¸½É
A_1=90: ÌÎ¼ÏÍ»¼¹»ºÍ¹¼¹ÈÍÈÏÎÉºÍ¾ÈÏÊË½É»¾Ê
A_1=91: ÍÏ½ÎÌº½¸º»Ì¸½¸ÉÌÉÎÏÈ»Ì¿ÉÎËÊ¼Èº¿Ë
A_1=92: ÊÈºÉË½º¿½¼Ë¿º¿ÎËÎÉÈÏ¼Ë¸ÎÉÌÍ»Ï½¸Ì
A_1=93: ËÉ»ÈÊ¼»¾¼½Ê¾»¾ÏÊÏÈÉÎ½Ê¹ÏÈÍÌºÎ¼¹Í
A_1=94: ÈÊ¸ËÉ¿¸½¿¾É½¸½ÌÉÌËÊÍ¾ÉºÌËÎÏ¹Í¿ºÎ
A_1=95: ÉË¹ÊÈ¾¹¼¾¿È¼¹¼ÍÈÍÊËÌ¿È»ÍÊÏÎ¸Ì¾»Ï
A_1=96: öôõ÷÷ò÷òõôó÷
                    òõðñó
                         ð
A_1=97: ÷õôööóöóôõòö
óôñðò
ñ
A_1=98: ôö
          ÷õ
            õ
             ðõð÷öñõð÷òó
ñò
A_1=99: õ÷
öô
ô
ñôñö÷ðôñöóò
           ðó
A_1=100: òðñó


óöóöñð÷
       óöñôõ÷
ô
A_1=101: óñðò


ò÷ò÷ðñö
ò÷ðõôö
      õ
A_1=102: ðòóñ üùüûúýùüûþÿ	ýþ
A_1=111: ùû	úø	ø	ýøýúûüøýúÿþüÿ
A_1=112: æäåçâçâåäãâåàáãàüþ	ý
A_1=113: çåäææãæãäåâæãäáàâá
A_1=114: äæçååàåàçæáåàçâãáâ
A_1=115: åçæäääáæãâàãùþûúøû
A_1=116: âàáããæãæáàçãçàââçâçàáæâçàåäæå
A_1=118: àâáäáäãâåáäãæçåæô÷   ù ùú
A_1=119: áãâààåàåâãäàåâçæçùýøÿúûø
A_1=120: îìêíèéèþûþûüýú	þ
A_1=121: ïíîîëìíêîïíèíèïîéèëëëïèíìí
A_1=126: èêëîïí
A_1=127: éëêèíèíêëì
_1=128: fafca`cfc`dgad
A_1=129: g`gb`abgbaef`e
A_1=130: dcdacbadabfecf
A_1=131: ebe`bc`e`cgdbg
A_1=132: bebgedgbgd`ce`
A_1=133: cdcfdefcfeabda
A_1=134: `g`egfe`efbagb
A_1=135: afadfgdadgc`fc
A_1=136: ninkihknk��l�ol
A_1=137: ohojhijoj�imn�hm
A_1=138: lklikjilijn�kn�
A_1=139: mjmhjkhmhko�ljo
_1=140: �jjomljolkmh
A_1=141: �lknlm�nkn�m�ijli
A_1=142: �hhmonmhm�njoj
A_1=143: �nilnolil�k�hnk
A_1=144: vqvsqpsvsptwqt
A_1=145: wpwrpqrwrquvpu
A_1=146: tstqsrqtqrvusv
A_1=147: uruprspupswtrw
A_1=148: rurwutwrwtpsup
A_1=149: stsvtuvsvuqrtq
A_1=150: pwpuwvupuvrqwr
A_1=151: qvqtvwtqtwspvs
A_1=152: 
y~{yx{~{ ~



x|
	
        y|
A_1=153: 
xzxyzz



y}

       ~
x}	
A_1=154: 
~|y{z   z|

}	{~

}_1=155: 
 z}xz{
      x}x	
            {
             	

z

A_1=156: 
z	
        }z}|
            z
             |
              x	
{}x

A_1=157: 
         	{
|{~|}
~{~
	}
y
z|y
A_1=158: 
x
 	x}~	}x}
                	


~	z

z        y
A_1=159: 	
           y
~y||y|


{
x
 ~{
A_1=160: 64F57AFCA@7CFC272543@7D2501G3AD0
A_1=161: 75G46@GB@A6BGB363452A6E3410F2@E1
A_1=162: 46D75CDACB5ADA050761B5F0723E1CF2
A_1=163: 57E64BE@BC4@E@141670C4G1632D0BG3
A_1=164: 20B13EBGED3GBG636107D3@6145C7E@4
A_1=165: 31C02DCFDE2FCF727016E2A7054B6DA5
A_1=166: 02@31G@EGF1E@E414325F1B4367A5GB6
A_1=167: 13A20FADFG0DAD505234G0C5276@4FC7
A_1=168: ><N=?INKIH?KNK:?:=<;H?L:=89O;IL8
A_1=169: ?=O<>HOJHI>JOJ;>;<=:I>M;<98N:HM9
A_1=170: <>L?=KLIKJ=ILI8=8?>9J=N8?:;M9KN:
A_1=171: =?M><JMHJK<HMH9<9>?8K<O9>;:L8JO;
A_1=172: :8J9;MJOML;OJO>;>98?L;H>9<=K?MH<
A_1=173: ;9K8:LKNLM:NKN?:?89>M:I?8=<J>LI=
A_1=174: 8:H;9OHMON9MHM<9<;:=N9J<;>?I=OJ>
A_1=175: 9;I:8NILNO8LIL=8=:;<O8K=:?>H<NK?
A_1=176: &$V%'QVSQP'SVS"'"%$#P'T"% !W#QT 
A_1=177: '%W$&PWRPQ&RWR#&#$%"Q&U#$! V"PU!
A_1=178: $&T'%STQSR%QTQ % '&!R%V '"#U!SV"
A_1=179: %'U&$RUPRS$PUP!$!&' S$W!&#"T RW#
A_1=180: " R!#URWUT#WRW&#&! 'T#P&!$%S'UP$
A_1=181: #!S "TSVTU"VSV'"' !&U"Q' %$R&TQ%
A_1=182:  "P#!WPUWV!UPU$!$#"%V!R$#&'Q%WR&
A_1=183: !#Q" VQTVW TQT% %"#$W S%"'&P$VS'
A_1=184: .,^-/Y^[YX/[^[*/*-,+X/\*-()_+Y\(
A_1=185: /-_,.X_ZXY.Z_Z+.+,-*Y.]+,)(^*X])
A_1=186: ,.\/-[\Y[Z-Y\Y(-(/.)Z-^(/*+])[^*
A_1=187: -/].,Z]XZ[,X]X),)./([,_).+*\(Z_+
A_1=188: *(Z)+]Z_]\+_Z_.+.)(/\+X.),-[/]X,
A_1=189: +)[(*\[^\]*^[^/*/().]*Y/(-,Z.\Y-
A_1=190: (*X+)_X]_^)]X],),+*-^)Z,+./Y-_Z.
A_1=191: )+Y*(^Y\^_(\Y\-(-*+,_([-*/.X,^[/
A_1=192: VT&UW!&#! W#&#RWRUTS W$RUPQ'S!$P
A_1=193: WU'TV '" !V"'"SVSTUR!V%STQP&R %Q
A_1=194: TV$WU#$!#"U!$!PUPWVQ"U&PWRS%Q#&R
A_1=195: UW%VT"% "#T % QTQVWP#T'QVSR$P"'S
A_1=196: RP"QS%"'%$S'"'VSVQPW$S VQTU#W% T
A_1=197: SQ#PR$#&$%R&#&WRWPQV%R!WPUT"V$!U
A_1=198: PR SQ' %'&Q% %TQTSRU&Q"TSVW!U'"V
A_1=199: QS!RP&!$&'P$!$UPURST'P#URWV T&#W
A_1=200: ^\.]_).+)(_+.+Z_Z]\[(_,Z]XY/[),X
A_1=201: _]/\^(/*()^*/*[^[\]Z)^-[\YX.Z(-Y
A_1=202: \^,_]+,)+*]),)X]X_^Y*].X_Z[-Y+.Z
A_1=203: ]_-^\*-(*+\(-(Y\Y^_X+\/Y^[Z,X*/[
A_1=204: ZX*Y[-*/-,[/*/^[^YX_,[(^Y\]+_-(\
A_1=205: [Y+XZ,+.,-Z.+._Z_XY^-Z)_X]\*^,)]
A_1=206: XZ([Y/(-/.Y-(-\Y\[Z].Y*\[^_)]/*^
A_1=207: Y[)ZX.),./X,),]X]Z[\/X+]Z_^(\.+_
A_1=208: FD6EG16310G363BGBEDC0G4BE@A7C14@
A_1=209: GE7DF07201F272CFCDEB1F5CDA@6B05A
A_1=210: DF4GE34132E141@E@GFA2E6@GBC5A36B
A_1=211: EG5FD25023D050ADAFG@3D7AFCB4@27C
A_1=212: B@2AC52754C727FCFA@G4C0FADE3G50D
A_1=213: CA3@B43645B636GBG@AF5B1G@ED2F41E
A_1=214: @B0CA70576A505DADCBE6A2DCFG1E72F
A_1=215: AC1B@61467@414E@EBCD7@3EBGF0D63G
A_1=216: NL>MO9>;98O;>;JOJMLK8O<JMHI?K9<H
A_1=217: OM?LN8?:89N:?:KNKLMJ9N=KLIH>J8=I
A_1=218: LN<OM;<9;:M9<9HMHONI:M>HOJK=I;>J
A_1=219: MO=NL:=8:;L8=8ILINOH;L?INKJ<H:?K
A_1=220: JH:IK=:?=<K?:?NKNIHO<K8NILM;O=8L
A_1=221: KI;HJ<;><=J>;>OJOHIN=J9OHML:N<9M
A_1=222: HJ8KI?8=?>I=8=LILKJM>I:LKNO9M?:N
A_1=223: IK9JH>9<>?H<9<MHMJKL?H;MJON8L>;O
A_1=224: vtuwwrwrutswrupqsp
A_1=225: wutvvsvsturvstqprq
A_1=226: tvwuupupwvqupwrsqr
A_1=227: uwvttqtqvwptqvsrps
A_1=228: rpqssvsvqpwsvqtuwt
A_1=229: sqprrwrwpqvrwputvu
A_1=230: prsqqtqtsruqtsvwuv
A_1=231: qsrppupurstpurwvtw
A_1=232: ~|}	
             	

             zz}|{
                 z}xy{	
                        x
A_1=233: }|~
	~

y|yxzz	~
A_1=234: |~
           }

            	

}	
        	x}x~y
yxz{
 z
~|1=235: }


y|y~x
     |y~{z
          x
{
A_1=236: zx
y{

{
~{~yx
     ~y|}
|
A_1=237: {y
           xz


z
zzxy~   x}|
~
 	}
|y|{z}y: x{y
|{~	}
~
A_1=239: y{	zx	
                x
                 	
                   }x}z{|x
                          }z|

A_1=240: fdeggbgbedcgbe`ac`
A_1=241: gedffcfcdebfcda`ba
A_1=242: dfgee`e`gfae`gbcab
A_1=243: egfddadafg`dafcb`c
A_1=244: b`accfcfa`gcfadegd
A_1=245: ca`bbgbg`afbg`edfe
A_1=246: `bcaadadcbeadcfgef
A_1=247: acb``e`ebcd`ebgfdg
A_1=248: nlmooojmlkojmhikh
A_1=249: omln�n��knklmjnklihji
A_1=250: lnom�mhmhoni�mhojki
A_1=251: monl��ilinohinkjh�k
A_1=252: jh�ik�k�nknihoknilml
A_1=253: kijjohinjohml�nm
A_1=254: hjkiililkjmi�lknom�n
A_1=255: ikjhhmhmjklhjonl
                                                                                           
┌──(root㉿kali)-[~]
└─$ cat fin3.py 
byteB = bytearray([20, 22, 100, 23, 21, 99, 100, 97, 99, 98, 21, 97, 100, 97, 16, 21, 16, 23, 22, 17, 98, 21, 102, 16, 23, 18, 19, 101, 17, 99, 102, 18])
c = 177
d = 51

for A_1 in range(256):
    A_3 = bytearray(len(byteB))

    for index in range(len(byteB)):
        A_3[index] = byteB[index] ^ d ^ (c ^ A_1)

    result = ""
    for b in A_3:
        result += chr(b)

    print(f"A_1={A_1}: {result}")



┌──(root㉿kali)-[~]
└─$ python3 fin2.py 
key
┌──(root㉿kali)-[~]
└─$ cat fin2.py 
byteB = bytearray([20, 22, 100, 23, 21, 99, 100, 97, 99, 98, 21, 97, 100, 97, 16, 21, 16, 23, 22, 17, 98, 21, 102, 16, 23, 18, 19, 101, 17, 99, 102, 18])
c = 177
d = 51

A_1 = 165  # Example value
A_3 = bytearray(len(byteB))

for index in range(len(byteB)):
    A_3[index] = byteB[index] ^ d ^ (c ^ A_1)

result = ""
for b in A_3:
    result += chr(b)

print(result)





```


### CCT2019 - for1

 Download Task Files

UPDATE: There was a bug found in cryptii that has now been fixed, but will cause issues on the final step of the challenge. For now, when you find the the cipher text FSXL PXTH EKYT DJXS PYMO JLAY VPRP VO, replace it with this cipher text instead: JHSL PGLW YSQO DQVL PFAO TPCY KPUD TF. Everything else at that step, e.g., the configuration file can remain as-is. I intend to update the challenge file to correct this issue, but this will serve as a temporary fix until that time.

Our former employee Ed is suspected of suspicious activity. We found this image on his work desktop and we believe it is something worth analyzing. Can you assist us in extracting any information of value?

HINT1: if you're not sure if a password is upper- or lower-case, try all lower-case.

HINT2: There are many steps that can be done concurrently in this challenge. If you find you need something, you may have not found the key to unlock it yet. If you have something useful and you're not sure where to use it, it's possible the file you need is still hidden somewhere.

HINT3: https://cryptii.com/ - Cool website, bro

HINT4: the flag will follow the format CCT{.*}

Answer the questions below

```python
┌──(root㉿kali)-[~/Downloads]
└─$ mkdir CCT2019                                                
                                                                                                             
┌──(root㉿kali)-[~/Downloads]
└─$ mv for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8 CCT2019
                                                                                                             
┌──(root㉿kali)-[~/Downloads]
└─$ cd CCT2019                                                   
                                                                                                             
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ ls                 
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8
                                                                                                             
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ file for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8 
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8: JPEG image data, JFIF standard 1.01, resolution (DPI), density 96x96, segment length 16, Exif Standard: [TIFF image data, big-endian, direntries=6, xresolution=86, yresolution=94, resolutionunit=2], baseline, precision 8, 514x480, components 3

──(root㉿kali)-[~/Downloads/CCT2019]
└─$ export PATH="$PATH:/home/witty/.local/bin"

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ exiftool for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8 
ExifTool Version Number         : 12.55
File Name                       : for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8
Directory                       : .
File Size                       : 30 kB
File Modification Date/Time     : 2023:02:18 13:36:38-05:00
File Access Date/Time           : 2023:02:18 13:37:13-05:00
File Inode Change Date/Time     : 2023:02:18 13:37:05-05:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Exif Byte Order                 : Big-endian (Motorola, MM)
X Resolution                    : 96
Y Resolution                    : 96
Resolution Unit                 : inches
Artist                          : Ed
Y Cb Cr Positioning             : Centered
Copyright                       : CCT 2019
XMP Toolkit                     : Image::ExifTool 11.16
Description                     : .--- ..- ... - .- .-- .- .-. -- ..- .--. .-. .. --. .... - ..--..
Author                          : Ed
Image Width                     : 514
Image Height                    : 480
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 514x480
Megapixels                      : 0.247




┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ stegoveritas for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8 
ERROR:StegoVeritas:Missing the following required packages: foremost, libexempi3
ERROR:StegoVeritas:Either install them manually or run 'stegoveritas_install_deps'.
Running Module: SVImage
+------------------+------+
|   Image Format   | Mode |
+------------------+------+
| JPEG (ISO 10918) | RGB  |
+------------------+------+
Trailing Data Discovered... Saving
b'PK\x03\x04\x14\x00\t\x00\x08\x00\x91e\xe6N\x0c8H]\x9f\x00\x00\x00\xbd\x00\x00\x00\x0c\x00\x1c\x00fakeflag.txtUT\t\x00\x03\xf2\xcf ]\x16\xd0 ]ux\x0b\x00\x01\x04\x00\x00\x00\x00\x04\x00\x00\x00\x00B\xd1b=s\xdb\xff\xf6&\xe6 A\x83\x10\xc3\xf1\xa4\xb9\x0c,\xdacb\xd3G\xe99\xcd\xc5\xee\xbd\xd7\xf7\xe7D*\xc3\x8eG\xf2"\x1b)\r\xc5\xd8\xf7)\xff\xabJn\x1d\xdd\xd4QR\x11A\xf2Ne\x87{pyu\x91\x18H\xd3m=\x96h[l\x85WW6d\x84\xcd\xd7ox\xcf`\xeb\x05\xe7\xd2\x06K\xc8\xf1JX\x96;\xbf$u\x1a@&\x84\r\xed\xd0{\xd7q\xda\x1c\xf59&\x1e \xdf\x0b"\rqV\x1d\xea\xa1?\x86\x8e\x08G\x9a\xd0\xb1i\x85\x85?[\x1d\xa7\xd7\xe2.\xa1w\x84\x07\n3\x84\xc1\xbfgkPK\x07\x08\x0c8H]\x9f\x00\x00\x00\xbd\x00\x00\x00PK\x01\x02\x1e\x03\x14\x00\t\x00\x08\x00\x91e\xe6N\x0c8H]\x9f\x00\x00\x00\xbd\x00\x00\x00\x0c\x00\x18\x00\x00\x00\x00\x00\x01\x00\x00\x00\xa4\x81\x00\x00\x00\x00fakeflag.txtUT\x05\x00\x03\xf2\xcf ]ux\x0b\x00\x01\x04\x00\x00\x00\x00\x04\x00\x00\x00\x00PK\x05\x06\x00\x00\x00\x00\x01\x00\x01\x00R\x00\x00\x00\xf5\x00\x00\x00\x00\x00'
Running Module: MultiHandler

Found something worth keeping!
JPEG image data, JFIF standard 1.01, resolution (DPI), density 96x96, segment length 16, Exif Standard: [TIFF image data, big-endian, direntries=6, xresolution=86, yresolution=94, resolutionunit=2], baseline, precision 8, 514x480, components 3
+--------+------------------+------------------------------------------------------------------------------------------------------------------------+--------------+
| Offset | Carved/Extracted | Description                                                                                                            | File Name    |
+--------+------------------+------------------------------------------------------------------------------------------------------------------------+--------------+
| 0x7212 | Carved           | Zip archive data, encrypted at least v2.0 to extract, compressed size: 159, uncompressed size: 189, name: fakeflag.txt | 7212.zip     |
| 0x7212 | Extracted        | Zip archive data, encrypted at least v2.0 to extract, compressed size: 159, uncompressed size: 189, name: fakeflag.txt | fakeflag.txt |
+--------+------------------+------------------------------------------------------------------------------------------------------------------------+--------------+

                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ ls
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8  results
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ cd results 
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results]
└─$ ls
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_autocontrast.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Blue_0.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Blue_1.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Blue_2.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Blue_3.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Blue_4.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Blue_5.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Blue_6.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Blue_7.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_blue_plane.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Edge-enhance_More.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Edge-enhance.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_enhance_sharpness_-100.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_enhance_sharpness_100.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_enhance_sharpness_-25.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_enhance_sharpness_25.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_enhance_sharpness_-50.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_enhance_sharpness_50.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_enhance_sharpness_-75.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_enhance_sharpness_75.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_equalize.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Find_Edges.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_GaussianBlur.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_grayscale.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Green_0.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Green_1.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Green_2.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Green_3.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Green_4.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Green_5.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Green_6.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Green_7.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_green_plane.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_inverted.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Max.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Median.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Min.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Mode.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Red_0.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Red_1.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Red_2.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Red_3.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Red_4.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Red_5.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Red_6.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Red_7.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_red_plane.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Sharpen.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_Smooth.png
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8_solarized.png
keepers
trailing_data.bin
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results]
└─$ cd keepers 
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ ls
1676745782.792027-d9acb94279093d34109c9f8509fda654  7212.zip  fakeflag.txt

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat fakeflag.txt 
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat -A fakeflag.txt 
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ ls -lah     
total 52K
drwxr-xr-x 2 witty witty 4.0K Feb 18 13:43 .
drwxr-xr-x 3 witty witty  12K Feb 18 13:42 ..
-rw-r--r-- 1 witty witty  29K Feb 18 13:43 1676745782.792027-d9acb94279093d34109c9f8509fda654
-rw-r--r-- 1 witty witty  349 Feb 18 13:43 7212.zip
-rw-r--r-- 1 witty witty    0 Jul  6  2019 fakeflag.txt



┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ unzip 7212.zip
Archive:  7212.zip
[7212.zip] fakeflag.txt password: password
replace fakeflag.txt? [y]es, [n]o, [A]ll, [N]one, [r]ename: y
  inflating: fakeflag.txt            
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ ls
7212.zip  fakeflag.txt  for1.jpg
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat fakeflag.txt   
I didn't say it would be easy, Neo. Peer into the Matrix. See what others cannot and witness the truth. Though I caution that it may be more than what you expect.

- Morpheus

PW: Z10N0101


┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ steghide extract -sf for1.jpg
Enter passphrase: Z10N0101
wrote extracted data to "archive.zip".

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ ls            
7212.zip  archive.zip  fakeflag.txt  for1.jpg
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ unzip archive.zip 
Archive:  archive.zip
[archive.zip] cipher.txt password: 0ni0n_0f_0bfu5c@ti0n
 extracting: cipher.txt              
  inflating: config.txt              
 extracting: flag.zip                
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat cipher.txt  
FSXL PXTH EKYT DJXS PYMO JLAY VPRP VO
                                                                                                                                                                       
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat config.txt 
C G. VI. VII. VIII. AMTU RING AM BY CH DR EL FX GO IV JN KU PS QT WZ

https://cryptii.com/

cipher.txt (Modified according to challenge description):


JHSL PGLW YSQO DQVL PFAO TPCY KPUD TF

┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ python3 decrypt_enigma.py
ctfforensicsisnotrealforensics
                                                                                  
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat decrypt_enigma.py    
from enigma.machine import EnigmaMachine
import enigma.plugboard

enigma.plugboard.MAX_PAIRS=13

machine = EnigmaMachine.from_key_sheet(
       rotors='Gamma VI VII VIII', # Appeared in file as "G. VI VII VIII".
       reflector='C-Thin', # Appeared in file as 'C', I changed to Thin for better results.
       ring_settings='R I N G', # Appeared in file as "XXXX"
       plugboard_settings="AM BY CH DR EL FX GO IV JN KU PS QT WZ") # No changes from file.

machine.set_display('AMTU') # No changes from file.

ciphertext = 'JHSL PGLW YSQO DQVL PFAO TPCY KPUD TF'.replace(" ", "")

plaintext = machine.process_text(ciphertext)

print(plaintext.lower())
                                                  
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ unzip flag.zip           
Archive:  flag.zip
[flag.zip] flag.txt password: 
 extracting: flag.txt                
                                                                                  
┌──(root㉿kali)-[~/Downloads/CCT2019/results/keepers]
└─$ cat flag.txt         
flag



Here's a detailed explanation of the code:

`from enigma.machine import EnigmaMachine import enigma.plugboard`

This imports the `EnigmaMachine` class and `enigma.plugboard` module from the `enigma` package.


`enigma.plugboard.MAX_PAIRS=13`

This sets the maximum number of plugboard pairs to 13. The plugboard is an optional component of the Enigma machine that allows the user to swap pairs of letters before encryption.


`machine = EnigmaMachine.from_key_sheet(        rotors='Gamma VI VII VIII', # Appeared in file as "G. VI VII VIII".        reflector='C-Thin', # Appeared in file as 'C', I changed to Thin for better results.        ring_settings='R I N G', # Appeared in file as "XXXX"        plugboard_settings="AM BY CH DR EL FX GO IV JN KU PS QT WZ") # No changes from file.`

This creates an `EnigmaMachine` object and sets its configuration using a `key sheet`. The key sheet is a string that specifies the rotor sequence, reflector, ring settings, and plugboard settings.

In this case, the rotor sequence is "Gamma VI VII VIII", the reflector is "C-Thin", the ring settings are "R I N G", and the plugboard settings are "AM BY CH DR EL FX GO IV JN KU PS QT WZ". Note that the comments indicate the original values from the file.

`machine.set_display('AMTU')`

This sets the machine's display to "AMTU". The display is the set of letters that are visible to the operator at any given time.

`ciphertext = 'JHSL PGLW YSQO DQVL PFAO TPCY KPUD TF'.replace(" ", "")`

This sets the ciphertext to "JHSLPGLWYSQODQVLPFAOTPCYKPUDTF", which is the same as the original ciphertext without spaces.


`plaintext = machine.process_text(ciphertext)`

This decrypts the ciphertext using the `process_text` method of the `EnigmaMachine` object and sets the plaintext to the result.

`print(plaintext.lower())`

This prints the decrypted plaintext in lowercase letters.

```



### CCT2019 - crypto1

 Download Task Files

Find ye some flags. There are three parts to this challenge, each with its own flag. Solve crypto1a obtain the crypto1a flag and to unlock crypto1b. Solve crypto1b to obtain the crypto1b flag and unlock crypto1c. Solve crypto1c and you'll have all three flags.

HINT1: crypto1a and crypto1b can be solved with freely available online tools

HINT2: For crypto1c, you probably have to code a solution to solve it as I'm not aware of any online tools for this variant. It's not complex to solve if you can figure out the scheme and it is possible to solve by hand although it could be a bit tedious.

HINT3: For crypto1c, start with "0" not "1".

Answer the questions below

```
┌──(root㉿kali)-[~/Downloads]
└─$ mv crypto1.zip CCT2019 
                                                                                   
┌──(root㉿kali)-[~/Downloads]
└─$ cd CCT2019 
                                                                                   
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ ls
crypto1.zip
for18f90d68390b565c308871a52c6572de8.for1_8f90d68390b565c308871a52c6572de8
results

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ cat crypto1a.txt 
Ab .aof y.jdbc'g. urp ornkcbi Ja.oap ogxoycygycrb jcld.po ,cnn rbnf i.y frg or uap x.jago. ru lgbjygaycrb ofmxrnov  Oycnnw cy odrgne i.y frg jnro. .brgid yr ucigp. rgy yd. p.oyv  Xgy jab frg ucigp. rgy yd. t.f ,dcjd dall.bo yr x. yd. bam. ru yd. _nafrgy_ ,dcjd jp.ay.e ydcov Ajygannfw frg dae x.yy.p .by.p cy ydpcj. hgoy yr x. oau. (ann nr,.p[jao. cu frg ln.ao.)v 

https://www.dcode.fr/keyboard-change-cipher

dvorak

qwerty

aN EASY TECHNIQUE FOR SOLVING cAESAR SUBSTITUTION CIPHERS WILL ONLY GET YOU SO FAR BECAUSE OF PUNCTUATION SYMBOLS>  sTILL< IT SHOULD GET YOU CLOSE ENOUGH TO FIGURE OUT THE REST>  bUT CAN YOU FIGURE OUT THE KEY WHICH HAPPENS TO BE THE NAME OF THE 'LAYOUT' WHICH CREATED THIS> aCTUALLY< YOU HAD BETTER ENTER IT THRICE JUST TO BE SAFE 9ALL LOWER_CASE IF YOU PLEASE0>

uhmm so change

 > to .

 < to ,
 
 9 to (

 0 o )

and switched the lower and upper cases

An easy technique for solving Caesar substitution ciphers will only get you so far because of punctuation symbols.  Still, it should get you close enough to figure out the rest.  But can you figure out the key which happens to be the name of the "layout" which created this. Actually, you had better enter it thrice just to be safe (all lower-case if you please).

three times so dvorakdvorakdvorak

let's see

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ unzip crypto1a.zip
Archive:  crypto1a.zip
[crypto1a.zip] crypto1a_flag.txt password: dvorakdvorakdvorak
  inflating: crypto1a_flag.txt       
  inflating: crypto1b.txt            
  inflating: crypto1b.zip            
                                                                                   
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ cat crypto1a_flag.txt 
Well, you made it. Not a particularly difficult challenge, but shows you know at least something about crypto.
For your troubles, here is your flag:
CCT{crypto1a_flag}  


┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ cat crypto1b.txt 
A word of advice for the next one. Don't straddle the fence or you'll end up riding a rail or five. It'll hurt from the bottom up.

n h newuhe eddre nect tota ufyaolim7ter val lcy vsf slAroeeoiroigtatradetlno o pek ?Sl n aee s epeth  atedpairu hsg?Hot oe.wygoelrfo 93aei alsw'e  elntte l o.A eat o,b' by le frnsk,nt tes uv hl o ir lgHayairiteobbaam ibuohlm tursernuuohgteseoob srk    spsrirt1 mdvoho'eI nmpiihi ainuetere susutpa .lwc  dsa   t t,iiorgoguhfecae r tcslhslayhn eseaftaeo peelsantnthu e,nwati  Tetees ecfh ai ofCteb seisn eto potb hyli'rCtirbsx oaego'sbttamt u?Mingfh.e  ev dac cfp  c om  hahh t enm t.esg f ut t  oilso uhao 

The Zigzag cipher is a transposition cipher where the plaintext is written diagonally rather than horizontally, making it more difficult to read. To decrypt it, the recipient must re-order the letters in the message so that they can read the original message. The zigzag pattern can be in any angle (e.g., vertical, horizontal, diagonal), and the depth of the zigzag can also vary.

Here is an example of a Zigzag cipher in action, using the message "HELLO WORLD" as plaintext and a zigzag depth of 4:


H . . . O . . . R . .
. E . L . W . L . D .
. . L . . . O . . . .


The ciphertext for this message would be "HOR.ELLWD.LO". To decrypt it, the recipient would re-order the letters according to the zigzag pattern and depth to reveal the original message.

https://www.dcode.fr/rail-fence-cipher

Keep punctuation and spaces

Key/Number number of rows/levels (height) 5

Start from Bottom (left)
No initial offset (recommended)

Use an offset of N characters, N=1

Decrypt Rail Fence

Moving·right·along·through·a·different·challenge.·How·are·you·at·ciphers·like·these?·Solvable·by·hand·and·made·easier·because·of·the·placement·of·the·upper·case·letters·and·punctuation·throughout·the·message,·no?·How·about·this·for·a·key.·The·way·the·goose·spells·terrific·from·the·1973·animated·movie·of·Charlotte's·web.·I've·seen·a·misspelling·in·the·title·of·a·clip·on·youtube.·At·the·very·least·it's·four·Cs,·but·it's·probably·six.·All·lower·case·for·goodness'·sake,·but·not·that·it·matters,·oui·oui?



┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ nano rail_decoder.py
                                                                                   
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ python3 rail_decoder.py    
Moving right along through a different challenge. How are you at ciphers like these? Solvable by hand and made easier because of the placement of the upper case letters and punctuation throughout the message, no? How about this for a key. The way the goose spells terrific from the 1973 animated movie of Charlotte's web. I've seen a misspelling in the title of a clip on youtube. At the very least it's four Cs, but it's probably six. All lower case for goodness' sake, but not that it matters, oui oui?
                                                                                   
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ cat rail_decoder.py 
enc = "n h newuhe eddre nect tota ufyaolim7ter val lcy vsf slAroeeoiroigtatradetlno o pek ?Sl n aee s epeth  atedpairu hsg?Hot oe.wygoelrfo 93aei alsw'e  elntte l o.A eat o,b' by le frnsk,nt tes uv hl o ir lgHayairiteobbaam ibuohlm tursernuuohgteseoob srk    spsrirt1 mdvoho'eI nmpiihi ainuetere susutpa .lwc  dsa   t t,iiorgoguhfecae r tcslhslayhn eseaftaeo peelsantnthu e,nwati  Tetees ecfh ai ofCteb seisn eto potb hyli'rCtirbsx oaego'sbttamt u?Mingfh.e  ev dac cfp  c om  hahh t enm t.esg f ut t  oilso uhao"
rails = 5
offset = rails-1

def railNumber(position, rails, offset):
    position = (position + offset) % (rails * 2 - 2)
    if (position < rails):
        return position
    else:
        return 2*rails-position-2

def decrypt(enc, rails, offset):
    result = ['+'] * len(enc)
    k = 0
    for i in range(rails):
        for j in range(len(enc)):
            if (railNumber(j, rails, offset) == i):
                result[j] = enc[k]
                k+=1
    return "".join(result)


print(decrypt(enc,rails,offset))

This script is an implementation of the rail fence cipher, a simple transposition cipher that rearranges the letters of a message in a zigzag pattern across multiple lines.

Here is a step-by-step explanation of how the script works:

1.  A message to be encrypted is stored in the variable `enc`.
2.  The number of rails (lines) used for the cipher is set to 5, and the offset (the difference between the number of rails and 1) is calculated.
3.  The `railNumber` function calculates the rail number for a given position in the ciphertext. It takes the position, number of rails, and offset as arguments, and uses modular arithmetic to determine the rail number based on the position.
4.  The `decrypt` function takes the ciphertext, number of rails, and offset as arguments. It creates a list `result` containing a `+` symbol for each character in the ciphertext. It then iterates over each rail, and for each rail, iterates over each character in the ciphertext. If the character's rail number matches the current rail, it is added to the `result` list. Finally, the `result` list is converted to a string and returned as the decrypted plaintext.
5.  The decrypted plaintext is printed to the console.

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ unzip crypto1b.zip
Archive:  crypto1b.zip
[crypto1b.zip] crypto1b_flag.txt password: teerrrriiffiicccccc
 extracting: crypto1b_flag.txt       
  inflating: crypto1c.txt  

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ cat crypto1b_flag.txt 
CCT{crypto1b_flag}  


┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ cat crypto1c.txt 
Last one of the basic batch. But is it compression, encoding, or encryption?

11122112141311112123131222211121621211124112213221112162112113114163113211421121132221622222411321311331611221121413111121231312222111216322121412123312222111122141624112123212416214122231631132114221112162321321242113531132142162321211424112123212322111121322231221111322216222232122141212112163113211422111216231224113221211211232216231224113113232121151124162312311111311416311321142211113212221112162411321222111216212111214132122211121623212114241121232123221111213222312211113222162411211422111124112214113316121421413132163131211211212321241642112141311112162222241132122211121624112231241121121121323221311416311321142141322122111216121212163131214121322213221111321311331615113114162411213242121632122411311322111211241631312211112123212416221321412132221112162411213222141621142211113212221112162112113222164211214131111132131622222123241122323113161421142111111341211212111151322122111122111111512213221111241122131151232121121135211422111132123221115124112123212311513113211422111111513113211211212111221111511

HINT3: For crypto1c, start with "0" not "1".

┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ file crypto1c.txt 
crypto1c.txt: ASCII text, with very long lines (1022), with CRLF line terminators



┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ python3 crypt1c.py
0101100101101111011101010010011101110110011001010010000001101101011000010110010001100101001000000110100101110100001000000111010001101000011010010111001100100000011001100110000101110010001011100010000001011001011011110111010100100111011101100110010100100000011100110110111101101100011101100110010101100100001000000110000101101100011011000010000001101111011001100010000001110100011010000110010100100000011000110111001001111001011100000111010001101111001000000110001101101000011000010110110001101100011001010110111001100111011001010111001100100000011001100111001001101111011011010010000001110100011010000110010100100000011000100110000101110011011010010110001100100000011000100110000101110100011000110110100000101100001000000110001001110101011101000010000001110100011010000110010101110010011001010010000001100001011100100110010100100000011011010110111101110010011001010010000001100011011010000110000101101100011011000110010101101110011001110110010101110011001000000110000101101000011001010110000101100100001011100010000001001000011011110111011100100000011101110110100101101100011011000010000001111001011011110111010100100000011001100110000101110010011001010010000001100001011001110110000101101001011011100111001101110100001000000111010001101000011011110111001101100101001000000100100100100000011101110110111101101110011001000110010101110010001011100010000001000001011101000010000001100001011011100111100100100000011100100110000101110100011001010010110000100000011101110110010101101100011011000010000001100100011011110110111001100101001000000110000101101110011001000010000001101000011001010111001001100101001000000110100101110011001000000111100101101111011101010111001000100000011001100110110001100001011001110011101000100000010000110100001101010100011110110100100101011111011100110110010101100101010111110110010001100101011000010110010001011111011000110110100101110000011010000110010101110010011100110101111101100001011011000110110001011111011101000110100001100101010111110111010001101001011011010110010101111101
You've made it this far. You've solved all of the crypto challenges from the basic batch, but there are more challenges ahead. How will you fare against those I wonder. At any rate, well done and here is your flag:flag
                                                                                                             
┌──(root㉿kali)-[~/Downloads/CCT2019]
└─$ cat crypt1c.py 
from Crypto.Util.number import long_to_bytes
enc = "11122112141311112123131222211121621211124112213221112162112113114163113211421121132221622222411321311331611221121413111121231312222111216322121412123312222111122141624112123212416214122231631132114221112162321321242113531132142162321211424112123212322111121322231221111322216222232122141212112163113211422111216231224113221211211232216231224113113232121151124162312311111311416311321142211113212221112162411321222111216212111214132122211121623212114241121232123221111213222312211113222162411211422111124112214113316121421413132163131211211212321241642112141311112162222241132122211121624112231241121121121323221311416311321142141322122111216121212163131214121322213221111321311331615113114162411213242121632122411311322111211241631312211112123212416221321412132221112162411213222141621142211113212221112162112113222164211214131111132131622222123241122323113161421142111111341211212111151322122111122111111512213221111241122131151232121121135211422111132123221115124112123212311513113211422111111513113211211212111221111511"
result = ""
for i in range(len(enc)):
    binary = '0' if i % 2 == 0 else '1'
    result+=binary*int(enc[i])
print(result)
flag = long_to_bytes(int(result, 2)).decode("ASCII") # binary to string.
print(flag)

The code you provided takes an encoded message in the variable `enc`, and decodes it using a simple interleave encoding scheme.

In the `for` loop, it iterates over each character in `enc`. For each character, it checks if the index is even or odd using the expression `i % 2 == 0`. If the index is even, it sets `binary` to `'0'`, otherwise it sets it to `'1'`. Then, it multiplies the value of the character by the value of `binary` and appends that many `0`s or `1`s to the `result` variable.

After the loop completes, the `result` variable contains a sequence of `0`s and `1`s that represent the decoded message. The code then converts this binary string to ASCII using the `long_to_bytes` function and prints the decoded message to the console.



```

