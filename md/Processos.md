# GESTIÓ DE PROCESOS

## ps

Admet opcions de tipus UNIX ( amb un guionet ), BSD ( sense guions ) i GNU ( amb dos guionets ).

Anem a centrar-nos en les principals heretades de UNIX.

Per defecte ensenya els procesos de l'usuari actiu oberts en el terminal actiu.

**Opcions per seleccionar**
|Modificador|Descriptor|
|--|--|
|ps -a|mostra els processos de tots els terminals all tty|
|ps -U tomas|mostra els processos de l'usuari real tomas|
|ps -u tomas|mostra els processos de l'usuari efectiu tomas|
|ps -e / ps -A|Mostra tots els processos dels sistema|

**Opcions de format d'eixida**
|Modificador|Descriptor|
|--|--|
|ps -l|llista|
|ps -f|format complet|

## top

**Visió contínua dels processos**
|Modificador|Descriptor|
|--|--|
|-d|indica el nombre de refrescos|
|-s|interactiu|

```bash
top - 17:05:05 up 1:58, 1 user, load average: 0,14, 0,19, 0,18
Tasks: 244 total,
1 running, 242 sleeping,
0 stopped,
1 zombie
%Cpu(s): 0,3 us, 0,2 sy, 0,0 ni, 99,4 id, 0,0 wa, 0,0 hi, 0,0 si,
0,0 st
MiB Mem : 15657,4 total, 12232,4 free,
1581,4 used,
1843,6 buff/cache
MiB Swap:
2048,0 total,
2048,0 free,
0,0 used. 13355,9 avail Mem
PID USER
COMMAND
841 root
237 root
irq/118-ELAN071
2074 tomas
PRNI
VIRT
SHR S%CPU%MEM20
-510 1176860 179092 142668 S
0
0
0
0 S2,3
1,71,1
0,01:20.37 Xorg
0:14.53
2001,70,30:20.45
495136
RES
54340
1/5
39620 S
TIME+Procesos.md
20/3/2021
mate-terminal
607 root
Network
20
0
338920
20260
17192 S
0,3
0,1
0:02.40
```
## PRIORITATS

|TEMPS REAL|0-99|
|--|--|
|USUARIS|100-139|

## nice

*  Ens permet llançar un procés amb una prioritat distinta a la per defecte.

Prioritat per defecte.
tomas@Portatil:~$ ./r.sh
tomas@Portatil:~$ ps -l -a -u ariadna
F S
UID
PID
PPID C PRI NI ADDR SZ WCHAN TTY
0 S 1000
4639
2080 0 80
0 - 3013 wait_w pts/0
4 R 1000
4643
4009 0 80
0 - 3513 -
pts/1
TIME CMD
00:00:00 r.sh
00:00:00 ps
Apugem al màxim valor:
^C
tomas@Portatil:~$ sudo nice -n-20 ./r.sh
tomas@Portatil:~$ ps -l -a -u ariadna
F S
UID
PID
PPID C PRI NI ADDR SZ WCHAN
4 S
0
4651
2080 0 80
0 - 4156 -
4 S
0
4652
4651 0 60 -20 - 3013 -
4 R 1000
4653
4009 0 80
0 - 3513 -
Baixem al mínim valor
^C
tomas@Portatil:~$ sudo nice -n+19 ./r.sh
2/5
TTY
pts/0
pts/0
pts/1
TIME CMD
00:00:00 sudo
00:00:00 r.sh
00:00:00 psProcesos.md
20/3/2021
tomas@Portatil:~$ ps -l -a -u ariadna
F S
UID
PID
PPID C PRI NI ADDR SZ WCHAN
4 S
0
4655
2080 0 80
0 - 4155 -
4 S
0
4656
4655 0 99 19 - 3013 -
4 R 1000
4657
4009 0 80
0 - 3513 -
TTY
pts/0
pts/0
pts/1TIME CMD
00:00:00 sudo
00:00:00 r.sh
00:00:00 ps
TTY
pts/2
pts/2
pts/2
pts/0TIME CMD
00:00:00 su
00:00:00 sh
00:00:00 r.sh
00:00:00 ps
TTY
pts/2
pts/2
pts/0TIME CMD
00:00:00 su
00:00:00 sh
00:00:00 ps
##renice
Ens permet canviar la prioritat de procesos en marxa.
tomas@Portatil:~$ ps -l -a -u ariadna
F S
UID
PID
PPID C PRI NI ADDR SZ WCHAN
4 S
0
5658
4958 0 80
0 - 4068 -
4 S 1001
5659
5658 0 80
0 -
654 -
0 S 1001
5662
5659 0 80
0 - 3013 -
4 R 1000
5674
2080 0 80
0 - 3513 -
tomas@Portatil:~$ sudo renice +5 -u ariadna
[sudo] contrasenya per a tomas:
1001 (user ID) old priority 0, new priority 5
tomas@Portatil:~$ ps -l -a -u ariadna
F S
UID
PID
PPID C PRI NI ADDR SZ WCHAN
4 S
0
5658
4958 0 80
0 - 4068 -
4 S 1001
5659
5658 0 85
5 -
654 -
4 R 1000
5720
2080 0 80
0 - 3513 -
kill
Ens permnet cancel·lar un procés per PID
kill SENYAL PID
Podem veure les senyals possibles:
tomas@Portatil:~$ kill -l
1) SIGHUP
2) SIGINT
3) SIGQUIT 4) SIGILL
5) SIGTRAP
6) SIGABRT 7) SIGBUS
8) SIGFPE
9) SIGKILL 10) SIGUSR1
11) SIGSEGV 12) SIGUSR2 13) SIGPIPE 14) SIGALRM 15) SIGTERM
16) SIGSTKFLT
17) SIGCHLD 18) SIGCONT 19) SIGSTOP 20) SIGTSTP
21) SIGTTIN 22) SIGTTOU 23) SIGURG 24) SIGXCPU 25) SIGXFSZ
26) SIGVTALRM
27) SIGPROF 28) SIGWINCH
29) SIGIO
30) SIGPWR
31) SIGSYS 34) SIGRTMIN
35) SIGRTMIN+1 36) SIGRTMIN+2 37) SIGRTMIN+3
3/5Procesos.md
20/3/2021
38) SIGRTMIN+4 39) SIGRTMIN+5 40) SIGRTMIN+6 41) SIGRTMIN+7 42)
SIGRTMIN+8
43) SIGRTMIN+9 44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47)
SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52)
SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9 56) SIGRTMAX-8 57)
SIGRTMAX-7
58) SIGRTMAX-6 59) SIGRTMAX-5 60) SIGRTMAX-4 61) SIGRTMAX-3 62)
SIGRTMAX-2
63) SIGRTMAX-1 64) SIGRTMAX
tomas@Portatil:~$
Normalment usem -9 o -15
$ ./r.sh
tomas@Portatil:~$ ps -u ariadna
PID TTY
TIME CMD
5659 pts/2
00:00:00 sh
7036 pts/2
00:00:00 r.sh
tomas@Portatil:~$ sudo kill -9 7036
$ ./r.sh
Killed
Exemples: kill -15 8985 454 332 kill SIGTERM 457 883
pkill i pidof
pkill Per matar el procés per nom o patró
pidof Per consultar prèviament quins processos compleixen en patró
pgrep per buscar segon s un patró
Exemple:
tomas@Portatil:~$ pidof nano
7452
tomas@Portatil:~$ pgrep nan
7452
tomas@Portatil:~$
4/5Procesos.md
20/3/2021
pgrep
Ens permet buscar processos per un patró ( expressió regular )
Exemples:tomas@Portatil:~$ pgrep nan
7722
tomas@Portatil:~$ pgrep -l nan
7722 nano
tomas@Portatil:~$ pidof nan
tomas@Portatil:~$ pidof nano
7722
tomas@Portatil:~$ pkill nan
tomas@Portatil:~$ nano
S'ha rebut SIGHUP o SIGTERM
tomas@Portatil:~$
killall
Ens permet matar processos per nom exacte
També per expressions regulars
Ens permet seleccionar per antiguetat del procés
tomas@Portatil:~$ pgrep -l nan
2307 nano
tomas@Portatil:~$ killall -r [nm]ano
Per antiguetat. Amb l'opció -y ( younger ) indiquem quants -s segons -m minuts -h hores -d dies -w setmanes
-M mesos -y anys
tomas@Portatil:~$ killall -y 2m nano
tomas@Portatil:~$
