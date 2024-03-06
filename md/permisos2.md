# EXEMPLE DE PERMISO UGO i PROPIETAT DE GRUP I USUARI SOBRE CARPETA.
## 1. ORDRES
|Ordre|Acció|
|---|---|
|useradd|crea usuari i assigan grup principal i directori de treball|
|chown| canvia el usuari i/o grup propietari d'una carpeta|
|chmod|canvia els permisos UGO d'una carpeta|


## 2.- Carpeta i permisos per defecte incials
Creem la carpeta i llevem els permisos a la resta d'usuari ( OTHERS )

```bash
tomas@A314-PC00:~$ mkdir CARPETA1
tomas@A314-PC00:~$ ls -ld CARPETA1/
drwxrwxr-x 2 tomas tomas 4096 de ma  6 11:44 CARPETA1/
tomas@A314-PC00:~$ chmod o-r-x CARPETA1/
tomas@A314-PC00:~$ ls -ld CARPETA1/     
drwxrwx--- 2 tomas tomas 4096 de ma  6 11:44 CARPETA1/
```
## 3.  Creem un grup i un usuari nou. 
```bash
tomas@A314-PC00:~$ sudo groupadd gr_tomas
tomas@A314-PC00:~$ sudo useradd joan -g gr_tomas
tomas@A314-PC00:~$ groups joan
joan : gr_tomas
```
## 4. Canviem usuari i grup propietari de la carpeta
```bash
tomas@A314-PC00:~$ sudo chown root:gr_tomas CARPETA1
```
Ara provarem si l'usuari joan ( de gr_tomas) té els permisos de grup: *drwx**rwx**---*
Primer hem de donar una contrassenya a l'usuari per poder iniciar sessió amb ell
```bash
tomas@A314-PC00:~$ sudo passwd joan
Nova contrasenya de : 
Torneu a escriure la nova contrasenya de : 
passwd: s'ha actualitzat la contrasenya satisfactòriament
```
Iniciem sessió
```bash
tomas@A314-PC00:~$ su joan
Contrasenya:
```
## 5. Comprovació

Intentem crear un fitxer en la carpeta ( demostrar que podem **w** escriure-hi
```bash
$ touch CARPETA1/tt
$ ls -l CARPETA1
total 0
-rw-r--r-- 1 joan gr_tomas 0 de ma  6 11:48 tt
```
Si volguérem afegir un nou usuari al grup...
```bash
tomas@A314-PC00:~$ sudo usermod tomas -aG gr_tomas
tomas@A314-PC00:~$ groups tomas
tomas : tomas sudo gr_tomas
```
