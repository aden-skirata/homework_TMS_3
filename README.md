## Linux commands

### 1. Сформировать команду, которая выводит список процессов, сортирует по CPU, сохраняет результат в файл, ошибки не попадают в файл. Отдельно продемонстрировать работу с ошибкой (например, неправильно написать в команде утилиту, которая выводит список процессов) и корректную работу.

```
normal

┌──(aden-skirata㉿kali)-[~]
└─$ ps aux --sort=-%cpu > processes.txt 2>/dev/null
                                                                                                                                   
┌──(aden-skirata㉿kali)-[~]
└─$ head processes.txt
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
aden-sk+    1164  3.2  1.0 1186116 60892 ?       Sl   11:44   0:00 xfdesktop
aden-sk+    1834  3.1  1.2 742092 72736 ?        Sl   11:44   0:00 /usr/bin/qterminal
aden-sk+    1545  2.5  1.0 604524 61204 ?        Sl   11:44   0:00 /usr/bin/python3 /usr/bin/blueman-applet
aden-sk+    1148  2.2  0.8 1186512 53228 ?       Sl   11:44   0:00 xfce4-panel
aden-sk+    1114  2.2  2.2 1738144 132164 ?      Sl   11:44   0:00 xfwm4
root         626  1.3  2.7 893748 163700 tty7    Ssl+ 11:43   0:00 /usr/lib/xorg/Xorg :0 -seat seat0 -auth /var/run/lightdm/root/:0 -nolisten tcp vt7 -novtswitch
aden-sk+    1169  1.3  0.7 981376 42932 ?        Sl   11:44   0:00 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libwhiskermenu.so 1 25165831 whiskermenu Whisker Menu Show a menu to easily access installed applications
aden-sk+     945  1.2  0.6 765704 41516 ?        Ssl  11:44   0:00 xfce4-session
aden-sk+    1255  1.0  0.1 213504  7856 ?        Sl   11:44   0:00 /usr/libexec/glycin-loaders/2+/glycin-image-rs --dbus-fd 19
                                                                                                                                   
┌──(aden-skirata㉿kali)-[~]
└─$ wc -l ~/processes.txt
247 /home/aden-skirata/processes.txt
                                        
```

```
utility name error

┌──(aden-skirata㉿kali)-[~]
└─$ pss aux --sort=-%cpu > ~/processes_err.txt 2>/dev/null
                                                                                                                                                                                                                                         
┌──(aden-skirata㉿kali)-[~]
└─$ ls -l ~/processes_err.txt 
-rw-rw-r-- 1 aden-skirata aden-skirata 0 Oct 25 10:22 /home/aden-skirata/processes_err.txt
                                                                                                                                                                                                                                         
┌──(aden-skirata㉿kali)-[~]
└─$ cat ~/processes_err.txt
                                                                                                                                                                                                                                         
┌──(aden-skirata㉿kali)-[~]
└─$ 
```

```
error record

┌──(aden-skirata㉿kali)-[~]
└─$ pss aux --sort=-%cpu > ~/processes_err.txt 2> ~/processes_err_stderr.txt
                                                                                                                                                                                                                                         
┌──(aden-skirata㉿kali)-[~]
└─$ cat ~/processes_err_stderr.txt                                          
Command 'pss' not found, but there are 17 similar ones.
```

### 2. Написать команду, которая выводит топ 10 процессов от текущего пользователя и при этом выводит результат одновременно в терминал и в файл (запись в файл, а потом его чтение не считается). 

```
┌──(aden-skirata㉿kali)-[~]
└─$ F="$HOME/top_10_proc.txt"; rm -f "$F"; tail -F -n +1 "$F" & TP=$!; ps -u "$(id -un)" -o pid,user,%cpu,%mem,comm --sort=-%cpu | head -n 11 | tee "$F"; sleep 2; kill $TP 2>/dev/null; wait $TP 2>/dev/null
[1] 42225
tail: cannot open '/home/aden-skirata/top_10_proc.txt' for reading: No such file or directory
    PID USER     %CPU %MEM COMMAND
   1116 aden-sk+  0.3  2.3 xfwm4
   2177 aden-sk+  0.2  1.3 qterminal
   1220 aden-sk+  0.2  0.5 wrapper-2.0
   1032 aden-sk+  0.2  0.0 VBoxClient
   1214 aden-sk+  0.2  0.9 wrapper-2.0
   2222 aden-sk+  0.1  0.1 zsh
   1024 aden-sk+  0.0  0.0 VBoxClient
   1167 aden-sk+  0.0  2.2 xfdesktop
   1150 aden-sk+  0.0  0.8 xfce4-panel
   1524 aden-sk+  0.0  0.7 xfce4-screensav
tail: '/home/aden-skirata/top_10_proc.txt' has appeared;  following new file
    PID USER     %CPU %MEM COMMAND
   1116 aden-sk+  0.3  2.3 xfwm4
   2177 aden-sk+  0.2  1.3 qterminal
   1220 aden-sk+  0.2  0.5 wrapper-2.0
   1032 aden-sk+  0.2  0.0 VBoxClient
   1214 aden-sk+  0.2  0.9 wrapper-2.0
   2222 aden-sk+  0.1  0.1 zsh
   1024 aden-sk+  0.0  0.0 VBoxClient
   1167 aden-sk+  0.0  2.2 xfdesktop
   1150 aden-sk+  0.0  0.8 xfce4-panel
   1524 aden-sk+  0.0  0.7 xfce4-screensav
[1]  + terminated  tail -F -n +1 "$F"
```

### 3. Просканировать ресурс scanme.nmap.org, проверить, какие порты открыты (задать порты разными способами: перечислением, диапазоном, и.т.п.), какие на них запущены сервисы, какая ОС установлена, а также отдельной командой просканировать только порты и вывести результат в grepable формате в Терминал (запись в файл, а потом его чтение не считается).

```
┌──(aden-skirata㉿kali)-[~]
└─$ nmap -p 22,80,443 scanme.nmap.org
Starting Nmap 7.991 ( https://nmap.org ) at 2026-10-25 11:12 -0400
Warning: Hostname scanme.nmap.org resolves to 2 IPs. Using 45.33.32.156.
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.18s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f

PORT    STATE  SERVICE
22/tcp  open   ssh
80/tcp  open   http
443/tcp closed https

Nmap done: 1 IP address (1 host up) scanned in 0.87 seconds
```

```
┌──(aden-skirata㉿kali)-[~]
└─$ nmap -p 1-1000 scanme.nmap.org
Starting Nmap 7.991 ( https://nmap.org ) at 2026-10-25 11:13 -0400
Warning: Hostname scanme.nmap.org resolves to 2 IPs. Using 45.33.32.156.
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.18s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 997 closed tcp ports (reset)
PORT   STATE    SERVICE
22/tcp open     ssh
25/tcp filtered smtp
80/tcp open     http

Nmap done: 1 IP address (1 host up) scanned in 14.63 seconds
```

```
результат не приложу, на этом скане повисла виртуалка, скан всех портов

nmap -p- scanme.nmap.org
```

```
tcp+udp

┌──(aden-skirata㉿kali)-[~]
└─$ nmap -sU -sS -p U:53,111,137,T:21-25,80,139,8080 scanme.nmap.org
Starting Nmap 7.991 ( https://nmap.org ) at 2026-10-25 11:51 -0400
Warning: Hostname scanme.nmap.org resolves to 2 IPs. Using 45.33.32.156.
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.23s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f

PORT     STATE    SERVICE
21/tcp   closed   ftp
22/tcp   open     ssh
23/tcp   closed   telnet
24/tcp   closed   priv-mail
25/tcp   filtered smtp
80/tcp   open     http
139/tcp  closed   netbios-ssn
8080/tcp closed   http-proxy
53/udp   closed   domain
111/udp  closed   rpcbind
137/udp  closed   netbios-ns

Nmap done: 1 IP address (1 host up) scanned in 3.20 seconds
```

```
versions, applications, port 1-1000

┌──(aden-skirata㉿kali)-[~]
└─$ nmap -sV -O -p 1-1000 scanme.nmap.org
Starting Nmap 7.991 ( https://nmap.org ) at 2026-10-25 12:05 -0400
Warning: Hostname scanme.nmap.org resolves to 2 IPs. Using 45.33.32.156.
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.27s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 997 closed tcp ports (reset)
PORT   STATE    SERVICE VERSION
22/tcp open     ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
25/tcp filtered smtp
80/tcp open     http    Apache httpd 2.4.7 ((Ubuntu))
Aggressive OS guesses: Linux 4.19 - 5.15 (99%), Linux 4.15 (95%), IPFire 2.27 (Linux 5.15 - 6.1) (94%), Linux 6.12 (94%), Linux 5.4 (93%), Linux 5.0 - 5.14 (92%), MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3) (92%), Linux 3.11 - 4.9 (92%), Linux 3.2 - 3.8 (92%), Linux 6.1 (91%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 25 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.62 seconds
```

```
grepable format to console

┌──(aden-skirata㉿kali)-[~]
└─$ nmap -p 1-1000 -oG - scanme.nmap.org
#Nmap 7.991 scan initiated Sun Oct 25 12:11:08 2026 as: /usr/lib/nmap/nmap --privileged -p 1-1000 -oG - scanme.nmap.org
Warning: Hostname scanme.nmap.org resolves to 2 IPs. Using 45.33.32.156.
Host: 45.33.32.156 (scanme.nmap.org)    Status: Up
Host: 45.33.32.156 (scanme.nmap.org)    Ports: 22/open/tcp//ssh///, 25/filtered/tcp//smtp///, 80/open/tcp//http///      Ignored State: closed (997)
#Nmap done at Sun Oct 25 12:12:12 2026 -- 1 IP address (1 host up) scanned in 64.50 seconds
```
