---
title: ADMINISTRACIÓ DE SISTEMES OPERATIUS
subtitle: UDx. 
author: Tomàs Ferrandis
abstract: |  
  
  Informació sobre el sistema Linux ( Ubuntu ).
  Directoris principals.
  Informació del processos i kernel.
  Gestió de dispositiu i sistema.
  Espai usuari ( systemd ).

lang: ca
titlepage: true
titlepage-rule-height: 0
titlepage-rule-color: FFF000
titlepage-background: ../IMATGES/PORTADAISOLINUX.pdf
toc-own-page: true
toc-title: Continguts
toc-depth: 3
header-left: UD1. INTRODUCCIÓ ALS SISTEMES OPERATIUS
footer-left: IES Maria Enríquez
footer-right: \thepage/\pageref{fi}
page-background: ../IMATGES/creative-commons-by-nc-sa.jpg
page-background-opacity: 0
header-includes:
  - \usepackage{awesomebox}
  
pandoc-latex-environment:
  noteblock: [note]
  tipblock: [tip]
  warningblock: [warning]
  cautionblock: [caution]
  importantblock: [important]

---

# PREVIA: systemd

**systemd** és una "plataforma" formada per un dimonis o daemons d'administració del sistema, biblioteques i eines  per a interactuar amb el kernel de GNU/Linux.
Inicialitza l'espai d'usuari en el procés d'arrancada  de Linux i...
Posteriorment, gestiona la resta de processos.

( els dimonis de Linux, per convenció tenen un nom acabat amb "d": systemd)

![systemd](IMATGES/systemd.png){width=20%}

# SISTEMA DE FITXERS FHS ( FILESYSTEM HIERACHY STANDARD)
 
```bash
tomas@portatil:~$ tree / -d -L 1
/
├── bin 
├── boot
├── cdrom
├── dev
├── etc
├── home
├── lib 
├── lost+found
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── snap
├── srv
├── sys
├── tmp
├── usr
└── var

```
|Directori|Descripció|
|---|---|
|/bin| Ordres executables per qualsevol usuari. Pot ser un enllaç a /usr/bin|
|/boot| Kernel, bootloader, fintxers del initrd
|dev| Fitxers de dispositius|
|etc|Fitxers sobre la configuració del sistema|
|home|Directori rel dels directoris d'usuari|
|lib|Llibreries que usen el binaris ( executables ) de /bin i /sbin|
|lost+found|Fragments o fitxers recuperats|
|media|Punts de muntatge de dispositius extraïbles|
|mnt|Sistemes de fitxers muntats temporalment|
|opt|Software no oficial|
|proc|Informació del kernel. Sistema de fitxers virtual.
|root|Home del usuari root|
|sbin|Ordres executables per a l'usuari administrador. Pot ser un enllaç simbòlic a /usr/sbin|
|srv|Informació sobre serveis|
|sys|Fitxers per exportar objectes del kernel
|tmp|Fitxers temporals|
|usr|Informació per compartir amb tots els usuaris. Només de lectura|
|var|logs,llocs web, bases de dades...( informació variable)|

Dins de /usr tenim una subestructura de directoris similar per a aquells programes compartits.

```bash
tomas@portatil:~$ tree /usr/ -L 1
/usr/
├── bin
├── games
├── include
├── lib
├── lib32
├── lib64
├── libexec
├── libx32
├── local
├── sbin
├── share
└── src
```
Un nou nivell per a programes locals
```bash

12 directories, 0 files
tomas@portatil:~$ tree /usr/local -L 1
/usr/local
├── bin
├── etc
├── games
├── include
├── lib
├── man -> share/man
├── sbin
├── share
└── src
```
# INFORMACIÓ DEL KERNEL
Les aplicacions poden accedir a la informació del kernel que està al directori **/sys** 
S'estructura en forma d'arbre ( sistema virtual *sysfs*) de forma que cada tipus d'objecte té un subdirectori

```bash
tomas@portatil:~$ tree /sys -L 1
/sys
├── block
├── bus
├── class
├── dev
├── devices
├── firmware
├── fs
├── hypervisor
├── kernel
├── module
└── power

```
```bash

tomas@portatil:~$ cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_max_freq
```
```bash
tomas@portatil:~$ cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_max_freq
2000000
```
Ho podem comprovar amb l'ordre *cpuinfo*
```bash
tomas@portatil:~$ lscpu 
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   48 bits physical, 48 bits virtual
CPU(s):                          8
On-line CPU(s) list:             0-7
Thread(s) per core:              1
Core(s) per socket:              8
Socket(s):                       1
NUMA node(s):                    1
Vendor ID:                       AuthenticAMD
CPU family:                      23
Model:                           96
Model name:                      AMD Ryzen 7 4700U with Radeon Graphics
Stepping:                        1
Frequency boost:                 enabled
CPU MHz:                         1400.000
CPU max MHz:                     2000,0000
...
```
# INFORMACIÓ SOBRE EL KERNEL
El directori **/proc** és un directori que només està en memòria principal, no existeix a la memòria secundària ( disc dur).

Trobarem dades que van canviant i altres fixes sobre els processos. Les ordres que hem vist *ps*, *top* lligen la informació d'ací.

El PCB ( Bloc de control del procés ) s'implementa dins de cada directori amb el nom del PID.

# Procesos 

```bash
tomas@portatil:~$ ls -d /proc/[0-9]*
/proc/1     /proc/12    /proc/1374  /proc/144   /proc/168   /proc/1779  /proc/1894  /proc/237   /proc/26    /proc/2899  /proc/3089  /proc/3811  /proc/48    ...
```
Dins de cada directori ( amb el nom de PID) trobarem directoris i fitxers amb la seua informació.

Fitxer de cada "procés" amb informació interessant.

|Fitxer|Tipus|Descriptor|
|--|--|--|
|maps|Fitxer|memòria usada pel procés|
|fd|directori|fitxers oberts ( enllaços a...)|
|status|fitxer|informació general del procés|

## Exemple de consultes

Per exemple podem consultar el mapa de memòria principal ( RAM ) usada pel procés PID= 1441
```bash
tomas@portatil:~$ cat /proc/1441/maps
55de37326000-55de3732f000 r--p 00000000 103:02 19530711                  /usr/bin/dbus-daemon
55de3732f000-55de37353000 r-xp 00009000 103:02 
...
```

```bash
tomas@portatil:~$ cat /proc/169/status
Name:	irq/27-ACPI:Eve
Umask:	0000
State:	S (sleeping)
Tgid:	169
Ngid:	0
Pid:	169
PPid:	2
TracerPid:	0
Uid:	0	0	0	0
Gid:	0	0	0	0
FDSize:	64
Groups:	 
NStgid:	169
NSpid:	169
NSpgid:	0
NSsid:	0
Threads:	1
SigQ:	0/61172
SigPnd:	0000000000000000
ShdPnd:	0000000000000000

```
Llista de fitxers oberts pel procés...

```bash
tomas@portatil:~$ sudo ls -l /proc/1441/fd
total 0
lrwx------ 1 tomas tomas 64 d’oct.   14 08:06 0 -> /dev/null
lrwx------ 1 tomas tomas 64 d’oct.   14 08:06 1 -> 'socket:[45968]'
lrwx------ 1 tomas tomas 64 d’oct.   14 08:06 10 -> 'socket:[44658]'
lrwx------ 1 tomas tomas 64 d’oct.   14 08:06 11 -> 'socket:[44659]'
lrwx------ 1 tomas tomas 64 d’oct.   14 08:06 12 -> 'socket:[44662]'
lrwx------ 1 tomas tomas 64 d’oct.   14 08:06 13 -> 'socket:[49359]'
 ```
## Pràctica senzilla de comprovació
Esbrinem el PID de l'execució de l'ordre *yes*
 ```bash
 tomas@portatil:~$ pidof yes
7369
```
Busquem el directori des d'on sha llançat l'ordre.
```bash 
tomas@portatil:~$ sudo ls -l /proc/7369/cwd
lrwxrwxrwx 1 rosa rosa 0 d’oct.   14 10:10 /proc/7369/cwd -> /home/rosa
```
Veiem que el procés s'ha llançat des del director */home/rosa*
Comprovem el nom de l'ordre.

```bash
tomas@portatil:~$ sudo cat /proc/7369/cmdline
yes
```
## ELS FITXERS LOG.
Les aplicacions, els serveis i el mateix sistema necessiten registrar missatges ( esdeveniments en l'execució, errors, alertes... ) en fitxers del sistema (.log ). 
Com hem avançat adés, esta informació la teniu a 
**/var/log/**
Podem veure'ls per ordre de modificació
```bash
tomas@portatil:~$ ls -lt /var/log/*log
-rw-r----- 1 syslog adm   313506 d’oct.   14 10:34 /var/log/auth.log
-rw-r----- 1 syslog adm  1647761 d’oct.   14 10:33 /var/log/syslog
-rw-r----- 1 syslog adm  2109201 d’oct.   14 10:21 /var/log/kern.log
...
...
```
|Fitxer log|Descripció|Ús|
|--|--|--|
|/var/log/syslog|Registre de propòsit general del sistema|Informació no crítica|
|/var/log/auth.log|Registre d'autenticació i autorització| Esdeveniments relatius a processos d'autenticació i autorització|
|/var/log/kern.log|Registre del Kernel|Informació del kernel|
|_exemple:_/var/log/mysql/error.log|Registre del servei myql-server|Conté informació dels errors|

El servei **rsyslog** és el que actualitza tots els fitxer log. És a dir, atén les peticions de subsistems i aplicacions.
```bash
tomas@portatil:~$ pidof rsyslogd
873
tomas@portatil:~$ ps -l 873
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY        TIME CMD
4 S   104     873       1  0  80   0 - 56088 -      ?          0:00 /usr/sbin/rs
tomas@portatil:~$ 
```

## LA GESTIÓ DE DISPOSITIUS ( /dev/ )

**udev** (userspace /dev ) és un subsistema de Linux que detecta i gestiona de forma dinàmica els dispositius ( antigament teníem el *devfs* i *hotplug* ).

1. Avisa a les aplicacions dels events de dispositius.
2. Aministra es permisos dels nodes als dispositius.
3. Pot crear enllaços simbòlics a dispositius o adispositus de xarxa.

UDEV es compon de 3 elements:

* **udev** llibreria per accedir a la informacio dels dispositius.
* **udevd** dimoni que gestiona el directori dinàmic */dev/*
* **udevadm** eina administrativa per a disgnòstic.
  
```bash
tomas@portatil:~$ udevadm info -a -n /dev/sda
```
```bash
tomas@portatil:~$ udevadm info -a -p /sys/class/net/wlp1s0
```

\newpage
\label{fi}
# Bibliografia

::: note

Temari original. Llicència Creative Commons Reconeixement-NoComercial-CompartirIgual 4.0
Internacional.

<img width=15% src="../IMATGES/creative-commons-by-nc-sa.jpg"></img>
:::

