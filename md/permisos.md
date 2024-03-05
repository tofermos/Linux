# PERMISOS EN EL PERFIL D'USUARI
## CREACIÓ DE LA CARPETA DE PERFIL

Creem un usuari tal com hem vist i observem qui té permisos i quins permisos en la carpeta */home/directori/*
```linux
tomas@portatil:~$ sudo useradd rosa -md /home/rosa
tomas@portatil:~$ ls -ld /home/rosa
drwxr-x--- 2 rosa rosa 4096 de març   5 10:06 /home/rosa
```
Assignem password per poder inciar sessió amb l'usuari
```linux
tomas@portatil:~$ sudo passwd rosa
Nova contrasenya: 
Torneu a escriure la nova contrasenya: 
passwd: s'ha actualitzat la contrasenya satisfactòriament
```
## ÚS DE LA CARPETA

Iniciem sessió amb l'usuari nou ( ordre *su usuari* )
```linux
tomas@portatil:~$ su rosa
Contrasenya: 
rosa@portatil:/home/tomas$ whoami
rosa
```
Intentem crear un fitxer. El que suposaria modificar el directori en el que estem.
```linux
rosa@portatil:/home/tomas$ pwd
/home/tomas
rosa@portatil:/home/tomas$ touch noCrea.txt
touch: no s’han pogut canviar les dates de 'noCrea.txt': S’ha denegat el permís
```
No tenim permisos sobre la carpetat actual. Veiem el perquè.
```linux
rosa@portatil:/home/tomas$ ls -ld .
drwxr-xr-x 74 tomas tomas 12288 de març   5 10:04 .
rosa@portatil:/home/tomas$ whoami
rosa
```
## CONCLUSIÓ. 

### HIPÒTESI

Quan es crea una carpeta de PERFIL d'USUARI:
* S'assigna com a propietari l'usuari ( tot i que s'executa com a *sudo* ). (**u** rosa )
* S'assigna com a grup propietari, el principal de l'usuari. ( **g** rosa)
* Se li dóna permisos d'escriptura  en carpeta només al propietari (**rwx** rosa ). (Llegir Nota 1)

### COMPROVACIÓ

Tanquem la sessió de *rosa* amb *exit* i tornarem a la sessió anterior de *tomas*
```linux
rosa@portatil:/home/tomas$ exit
tomas@portatil:/home/tomas$ whoami
tomas
```
Intentem anar al directori personal de *rosa* amb l'usuari *tomas* que és administrador
```linux
tomas@portatil:~$ whoami
tomas
tomas@portatil:~$ cd /home/rosa
bash: cd: /home/rosa: S’ha denegat el permís
```
Veiem que no ens deixa. Recordem els permisos de la carpeta de perfil amb *ls -ld .*
```linux
tomas@portatil:~$ ls -ld /home/rosa
drwxr-x--- 2 rosa rosa 4096 de març   5 10:13 /home/rosa
```

Només el grup de *rosa* pot executar la carpeta. Els "altres" (**others** ), no.

### PROVA FINAL
Què passaria si incloerem l'usuari *tomas* en el grup *rosa?

```linux
tomas@portatil:~$ groups tomas
tomas : tomas adm cdrom sudo dip plugdev lpadmin lxd sambashare
tomas@portatil:~$ sudo usermod tomas -aG rosa
[sudo] contrasenya per a tomas: 
tomas@portatil:~$ groups tomas
tomas : tomas adm cdrom sudo dip plugdev lpadmin lxd sambashare rosa
```
Els canvis per a aplicar-se cal que reinciem la sessió de *tomas*
> **NOTE***
> usermod **-aG**
>Si no indicamos -a, elimina el usuario de los grupos que pertenece. Ver ayuda (man) seobre *-aG, -G, -g*
```linux
tomas@portatil:~$ cd /home/rosa
tomas@portatil:/home/rosa$ pwd
/home/rosa
```






