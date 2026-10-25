## Linux commands

### 1. Сформировать команду, которая выводит список процессов, сортирует по CPU, сохраняет результат в файл, ошибки не попадают в файл. Отдельно продемонстрировать работу с ошибкой (например, неправильно написать в команде утилиту, которая выводит список процессов) и корректную работу.

```
normal

┌──(aden-skirata㉿kali)-[~]
└─$ head -n 15 ~/processes.txt 
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         629  0.5  4.6 1173220 279864 tty7   Ssl+ 09:28   0:13 /usr/lib/xorg/Xorg :0 -seat seat0 -auth /var/run/lightdm/root/:0 -nolisten tcp vt7 -novtswitch
aden-sk+    1116  0.3  2.3 1735004 140164 ?      Sl   09:28   0:10 xfwm4
aden-sk+    1220  0.2  0.5 277664 30300 ?        Sl   09:28   0:06 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libgenmon.so 15 25165838 genmon Generic Monitor Show output of a command.
aden-sk+    1032  0.2  0.0 215492  3372 ?        Sl   09:28   0:06 /usr/bin/VBoxClient --draganddrop
aden-sk+    1214  0.2  0.9 308712 57556 ?        Sl   09:28   0:06 /usr/lib/x86_64-linux-gnu/xfce4/panel/wrapper-2.0 /usr/lib/x86_64-linux-gnu/xfce4/panel/plugins/libcpugraph.so 13 25165836 cpugraph CPU Graph Graphical representation of the CPU load
aden-sk+    2177  0.1  1.3 752144 83748 ?        Rl   09:29   0:04 /usr/bin/qterminal
aden-sk+    2222  0.0  0.1  10520  6628 pts/0    Ss   09:29   0:01 /usr/bin/zsh
root         568  0.0  0.0 352956  2796 ?        Sl   09:28   0:01 /usr/bin/VBoxDRMClient
aden-sk+    1024  0.0  0.0 214976  3468 ?        Sl   09:28   0:01 /usr/bin/VBoxClient --seamless
root        4923  0.0  0.0      0     0 ?        I    09:34   0:01 [kworker/2:0-events]
aden-sk+    1167  0.0  2.2 1554004 132108 ?      Sl   09:28   0:01 xfdesktop
aden-sk+   22943  0.0  0.1 309336  7204 ?        Ssl  10:10   0:00 /usr/lib/x86_64-linux-gnu/xfce4/xfconf/xfconfd
aden-sk+    1150  0.0  0.8 1051000 52948 ?       Sl   09:28   0:01 xfce4-panel
root           1  0.0  0.2  28420 17320 ?        Ss   09:27   0:01 /sbin/init splash
                                                                                                                                                                                                                                         
┌──(aden-skirata㉿kali)-[~]
└─$ wc -l ~/processes.txt
191 /home/aden-skirata/processes.txt
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

```
