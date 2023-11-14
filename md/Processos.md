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
tomas@portatil:~$ top -n1

top - 23:04:07 up  8:00,  1 user,  load average: 0,20, 0,26, 0,34
Tasks: 321 total,   1 running, 320 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0,7 us,  1,5 sy,  0,0 ni, 97,8 id,  0,0 wa,  0,0 hi,  0,0 si,  0,0 st
MiB Mem :  15398,6 total,   7634,3 free,   2979,4 used,   4784,8 buff/cache
MiB Swap:   2048,0 total,   2048,0 free,      0,0 used.  11989,8 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND  
      1 root      20   0  166996  11972   8132 S   0,0   0,1   0:02.39 systemd  
      2 root      20   0       0      0      0 S   0,0   0,0   0:00.02 kthreadd 
      3 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_gp   
      4 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_par+ 
      5 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 slub_fl+ 
      6 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 netns    
      8 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 kworker+ 
     10 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 mm_perc+
```
## PRIORITATS

|TEMPS REAL|0-99|
|--|--|
|USUARIS|100-139|

## nice

*  Ens permet llançar un procés amb una prioritat distinta a la per defecte.

Prioritat per defecte.
```bash
tomas@Portatil:~$ ./r.sh
```
```bash
tomas@portatil:~$ ps -l -C r.sh
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
0 S  1000   24383   24232  0  80   0 -  4498 do_wai pts/0    00:00:00 r.sh
tomas@portatil:~$ 
```
Apugem el valor de la prioritat al màxim
```bash
sudo nice -n-20 ./r.sh
```

```bash
tomas@portatil:~$ ps -l -C r.sh
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0   25894   25893  0  60 -20 -  4498 -      pts/3    00:00:00 r.sh
```

Abaixem el valor al mínim

```bash
sudo nice -n+19 ./r.sh

```bash
tomas@portatil:~$ ps -l -C r.sh
F S   UID     PID    PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0   26129   26128  0  99  19 -  4498 -      pts/3    00:00:00 r.sh
tomas@portatil:~$ 
```

Ens permnet cancel·lar un procés per PID
``
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
