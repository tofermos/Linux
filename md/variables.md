# VARIABLES EN LINUX

## Variables

Entenem per **variable** una etiqueta que apunta a un espai de memòria on es guarda un valor ( text, número... ) que pot canviar i és necessari per a l'execució d'un programa.
Per assignar un valor que es guarda a l'espai de memòria s'usa el "=". Escrivim un valor.
```bash
var1=123
```
Per obtenir el valor. Llegir podem usar l'ordre "echo". Però **amb $** precedint el nom de la variable.
```bash
echo $var1
```
Per suprimir la variable usem "unset". Si de
```bash
unset var1
```


## Variable d'entorn

Les  **variables d'entorn** són aquelles que son accessibles a la resta processos on s'han definit i no sols en el procés en si.

Es defineixen amb _export_ 

Poden ser de **de sistema** o definides per l'usuari, aplicació...**locals**

**Ordres _printenv_ i _env_.**

Els fitxers binaris de les ordres els trobem a:
```bash
tomas@portatil:~$ ls -l /usr/bin/env
-rwxr-xr-x 1 root root 43352 de set.   5  2019 /usr/bin/env
tomas@portatil:~$ ls -l /usr/bin/printenv
-rwxr-xr-x 1 root root 39256 de set.   5  2019 /usr/bin/printenv
```

Amb _printenv_ o amb _env_ veurem les variables d'entorn. La seua coexistència respon a l'evolució històrica de Linux heretant codi de diferents projectes i Sistemes Operatius ( BSD, UNIX, MINIX...).

Tots dos comandaments son similars però no idèntics. De fet, en algun cas observarem comportaments diferents ( proveu consultar la variable HOSTNAME).

```bash
tomas@servidor-linux:~$ env
SHELL=/bin/bash
SESSION_MANAGER=local/servidor-linux:@/tmp/.ICE-unix/5637,unix/servidor-linux:/tmp/.ICE-unix/5637
QT_ACCESSIBILITY=1
COLORTERM=truecolor
XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
XDG_MENU_PREFIX=gnome-
GNOME_DESKTOP_SESSION_ID=this-is-deprecated
GNOME_SHELL_SESSION_MODE=ubuntu
SSH_AUTH_SOCK=/run/user/1000/keyring/ssh
XMODIFIERS=@im=ibus
DESKTOP_SESSION=ubuntu
SSH_AGENT_PID=5547
GTK_MODULES=gail:atk-bridge
PWD=/home/tomas
LOGNAME=tomas
XDG_SESSION_DESKTOP=ubuntu
XDG_SESSION_TYPE=x11
GPG_AGENT_INFO=/run/user/1000/gnupg/S.gpg-agent:0:1
XAUTHORITY=/run/user/1000/gdm/Xauthority
WINDOWPATH=2
HOME=/home/tomas
USERNAME=tomas
...
```
Les comptem:
```bash
tomas@servidor-linux:~$ env|wc -l
46
```
Per visualitzar una en concret...
```bash
tomas@servidor-linux2:~$ printenv PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/noseque
tomas@servidor-linux2:~$ env $PATH
env: ‘/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/noseque’: No such file or directory
tomas@servidor-linux2:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/noseque
```
:::nota 
Fixem-nos que amb _env_ i amb _echo_ necessitem precedir el nom de la variable amb el dolar. Com sempre que es llig una variable en Linux ( no quan volem actualitzar el seu valor !). 
L'ordre _printenv_, en canvi no hem d'usar el dolar
:::

:::caution
Recordem que, per defecte, tot shell de Linux diferencia **majúscules i minúscules**
:::

## Variables locals o d'usuari

Son les que creem com a usuaris ( normalment dins d'un script) per guardar valors que necessitem en l'execució d'unes ordres. 

Desapareixen en tancar-se el procés que les ha creat.


## Variables de sistema ( variables d'entorn permanent)

El seu valor l'actualitza algún procés del sistema que s'executa en engegar la màquina, software de configuració o en iniciar sessió a partir de valors predefinits en fitxers del sistema.
Estos fitxers són els que es modifiquen en les instal·lacions o per l'administrador.

Evidentment son variables d'entorn.

Les podem modificar com si es tractaren de variables locals però no té cap utilitat pràctica. En tancar la sessió, evidentment la variable no desapareix, però torna a tindre el seu valor per defecte.

```bash
tomas@servidor-linux2:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
tomas@servidor-linux2:~$ PATH=$PATH:/noseque
tomas@servidor-linux2:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/noseque
tomas@servidor-linux2:~$ 
```
A l'exemple assignem ( afegint! ) un nou directori "/noseque".
Només reiniciant la sessió del terminal comprovarem que la modificació no hi és.

:::tip
Video sobre variables.
[Vídeo de variables Linux](https://youtu.be/KZ3u2dCIqSU)
:::

# Modificar variables de sistema.

## Exemple del PATH.

Afegirem la línia següent en un fitxer que s'execute en iniciar la sessió l'usuari.

> export PATH="$PATH:/noudirectori"

Si volem que el canvi afecte a tots els usuaris, el fitxer sera _/etc/profile_
```bash
tomas@servidor-linux:~$ sudo nano /etc/profile
```
Si només volem que afecte a un usuari ( que usa el shell "bash")
```bash
tomas@servidor-linux:~$ sudo nano ~/.bash_profile
```
o 
```bash
tomas@servidor-linux:~$ sudo nano ~/.bash_login
```
o
```bash
tomas@servidor-linux:~$ sudo nano ~/.profile
```
o
```bash
tomas@servidor-linux:~$ sudo nano ~/bashrc
```
En cas de que l'usuari use altres shells, s'han de buscar fitxers anàlogs.

|SHELL| | |||
|-|-|-|-|-|
|**bash**|~/.bash_profile|~/.bash_login|~/.bashrc|./profile|
|**ksh**|~/.ksh_profile|~/.ksh_login|~/.kshrc|./kprofile|
|**zsh**|~/.zsh_profile|~/.zsh_login|~/.zshrc|./zprofile|

# Activitats

## Activitat 1. PATH

* Fer dos modificacions per a que s'afija al PATH dos directoris:
    
    * Un directori, _/bin/dir1_ estarà al PATH de tots els usuaris que executen el /bash
  * L'altre, _/bin/dir2_  només estarà diponible per a un usuari.



  
  
* Crea un usuari amb un altre shell distint al _bash_ 

```bash
tomas@portatil:~$ sudo useradd joana -d /home/joana -s /bin/dash
tomas@portatil:~$ sudo passwd joana
Nova contrasenya de : 
Torneu a escriure la nova contrasenya de : 
passwd: s'ha actualitzat la contrasenya satisfactòriament
```

* Des d'una sessió del teu usuari, inicia una sessió amb _joana_
```bash
tomas@portatil:~$ su joana
Contrasenya: 
$ echo $SHELL
/bin/dash
```
* Comprova els valors de la variable PATH i raona què passa amb els directoris /bin/dir1 i /bin/dir2


<img width=15% src="../IMATGES/creative-commons-by-nc-sa.jpg"></img>
