# El Shell

Ja tenim clar el concepte de SHELL però...

* Sabem de quins Shells disposem a la nostra màquina ( real o virtual )?
* Quin shell estem usant en cada moment ?
* En podem instal·lar altres?, podem canviar-lo?
  
<div id='id2' />

# Shells disponibles

Per veure quins shells tenim instal·lats al nostre ordinador, consultem el contingut del fitxer "shells" del directori de configuració "/etc"

```bash
tomas@portatil:~$ cat /etc/shells
# /etc/shells: valid login shells
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/pwsh
/opt/microsoft/powershell/7/pwsh
tomas@portatil:~$ 
```

<div id='id3' />

## Versió del Shell instal·lada.

Una opció vàlida per a tots els shells és l'ordre **_apt-cache show_**

```bash
tomas@portatil:~$ apt-cache show bash|grep -i version
Version: 5.0-6ubuntu1.2
Version: 5.0-6ubuntu1
tomas@portatil:~$ apt-cache show powershell|grep -i version
Version: 7.2.6-1.deb
tomas@portatil:~$ 
```
Llegir nota sobre l' ús del _grep_[^1].

# Shell en ús

* La variable de sistema $SHELL ens diu el shell per defecte.
* Però podem executar un altre. ( "sh" )
* Per consultar el shell que està executant-se ( procés ), cal esbrinar el seu identificador ( PID ) mitjançant "$$"
* i amb l'ordre "ps" esbrinem els procesos en marxa en el terminal. Podem filtrar pel PID.

```bash
tomas@portatil:~$ echo $SHELL
/bin/bash
tomas@portatil:~$ sh
$ echo $SHELL
/bin/bash
$ echo $$
27590
$ ps 27590
    PID TTY      STAT   TIME COMMAND
  27590 pts/7    S      0:00 sh
$ exit
tomas@portatil:~$ 
```
<div id='id4' />

# Shell de l'usuari.

En crear un usuari se li assigna un "shell" per defecte que serà el que usarà.  Este shell podrem modificar-lo posteriorment.

* On trobem quin és el Shell de l'usuari per defecte?
* Com podem canviar-li'l?
* On es configura el SHELL per defecte i com canviar-lo.

```bash
tomas@portatil:~$ cat /etc/default/useradd|grep ^SHELL=
SHELL=/bin/bash
tomas@portatil:~$ cat /etc/passwd|grep ^tomas:
tomas:x:1000:1000:tomas,,,:/home/tomas:/bin/bash
```
Llegir nota sobre l'ús del _grep_[^2]

## Canvis de Shell de l'usuari

:::caution
Precaució: Una bona pràctica abans de fer estos canvis consisteix en una fer còpia de seguretat del fitxer */etc/passwd*
:::


### Usem l'ordre **_chsh_**

```bash
tomas@portatil:~$ cat /etc/passwd|grep ^tomas:
tomas:x:1000:1000:tomas,,,:/home/tomas:/bin/bash
tomas@portatil:~$ sudo chsh -s /bin/sh tomas
[sudo] contrasenya per a tomas: 
tomas@portatil:~$ cat /etc/passwd|grep ^tomas:
tomas:x:1000:1000:tomas,,,:/home/tomas:/bin/sh

```
El canvi tindrà efecte en reiniciar la sessió d'usuari.

### Usem l'ordre **_usermod_**

Una altre comandament que podem usar per a modificar el shell d'un usuari ( o qualsevol altre valor) és el _usermod_
```bash
tomas@portatil:~$ sudo cp /etc/passwd /etc/passwd.bck
[sudo] contrasenya per a tomas: 
tomas@portatil:~$ cat /etc/passwd |grep ^rosa:
rosa:x:1001:1001:rosa,,,,:/home/rosa:/bin/dash
tomas@portatil:~$ sudo usermod -s /bin/bash rosa
tomas@portatil:~$ cat /etc/passwd |grep ^rosa:
rosa:x:1001:1001:rosa,,,,:/home/rosa:/bin/bash
```



## Shell per defecte del _useradd_

El shell que assigna el _useradd_ el trobem en el fitxer...

```bash
tomas@portatil:~$ cat /etc/default/useradd|grep ^SHELL
SHELL=/bin/bash
```

::: tip
Video demostració sobre la modificació del _shell_ que aplicarà l'ordre _useradd_

[Vídeo demostració _useradd_](https://youtu.be/ISOg8g3K20o)
:::


\newpage
# Activitats complementàries.

## Instal·lació del PowerShell

Seguiu la Guia que teniu ( INSTAL·LAR POWERSHELL EN UBUNTU ), instal·leu-lo a la vostra màquina i proveu algun cmdLets:

```powershell
tomas@portatil:~$ pwsh
PS /home/tomas> get-location

Path
----
/home/tomas
```

Comprovem la versió de Power consultant la variable de PowerShell ( no cal "echo")
```powershell

PS /home/tomas> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      7.2.6
PSEdition                      Core
GitCommitId                    7.2.6
OS                             Linux 5.15.0-46-generic #49~20.04.1-Ubuntu SMP …
Platform                       Unix
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0

Path
----
/home/tomas

PS /home/tomas> 
``` 
## Comprovació shell de _useradd_
En una màquina virtual de Linux, comprova tal com es fa al vídeo vist anteriorment el canvi del shell per defecte de _useradd_ 

>**Note**
>
>Cal un coneixement mínim de les ordres de tractament de fitxers de text (grep, cut, tail, head, tr...)
🚧


[^1]: Quan no tenim clar el text que busquem, podem usar el modificador "-i" per a no diferenciar majúscules i minúscules.

[^2]: En este cas partim d'una certesa, contràriament al de la nota anterior. Sabem com comença la línia i per això usem el "^". També un caràcter delimitador "=", ":"...
