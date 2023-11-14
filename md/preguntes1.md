Solució a diferents exercicis sobre:
* Consola Linux.
* Gestió de comptes Linux i Windows 1X
* Gestió de permisos (*ugo*, ACL ) Linux i Windows 1X
* WorkGroup en Windows 1X

### Exercici 1
Per poder executar des de qualsevol lloc els scripts que tenim en una carpeta: 
/utilitats/manteniment/eines/software/scripts
sense haver d'esciure la ruta completa cada vegada hem d'**incloure esta ruta al PATH**

>**Note**
>
>Si vos demanen que un "executable", siga un fitxer binari (ordre o aplicació), siga script (text pla) puga executar-se **des de qualsevol directori**,  vol dir que el fitxer (o un enllaç) estarà en un directori registrat en la **variable de sistema PATH**.
>El sistema busca les ordres que enviem ("executables") en el directori que indiquem, si no està busca en cadascun dels directori anotats a la variable de sistema PATH fins que el troba.
>No pot buscar en tot l'arbre de directoris, seria inviable.

Pasos 1:
Editar el fitxer ~/.bashrc i afegir com a darrera línia:
```makefile
export PATH=$PATH:/utilitats/manteniment/eines/software/scripts
```
Pas 2: Executar l'script modificat
```bash
sudo ~/.bashrc
```
### Exercici 2
Després de fer un "du -d 1 -h /home/tomas/.git"
Et demana una llista **ordrenada de només els tamanys**:

1.  llevem el "-h" (human readable) per a que ho expresse tot en la mateixa unitat "K" i així podrem 
2.  ordrenar numèricament amb el "sort -n".
3.  i seleccionar el primer "field" (columna) resultant, amb el "cut -f1"

``` bash
du -d 1 /home/tomas/.git|sort -n|cut -f1
```
### Exercici 3
En general el find l'heu fet prou bé
```bash
sudo find /home -size +10G >> /home/massagrans
```
Esta segona part és la que alguns no heu encertat...
```bash
sudo find /home -size +10G -exec mv {} /home/tomas/aeliminar \;
```

### Exercici 4
Buscar la IP.

Poden haver moltes targes, per tant, heu de buscar amb el "grep" primer.

```bash
ip -a |grep wlp1s0|tr -s " "|cut -f3 -d " "|tail -n1
```
Altre exercici...

Buscar la MAC (la part només de fabricant)

```bash
ip a|grep wlp1s0 -n1|tr -s " "|head -n3|tail -n1|cut -f3 -d" "|cut -d":" -f1-3
```

### Exercici 5
Crear usuaris i grups no té cap complicació...
```bash
tomas@MVUbuntu:~$ sudo groupadd grup1
tomas@MVUbuntu:~$ sudo groupadd grup2
tomas@MVUbuntu:~$ sudo useradd usuari1 -g grup1
tomas@MVUbuntu:~$ sudo useradd usuari2 -g grup1
tomas@MVUbuntu:~$ sudo useradd usuari3 -g grup2
tomas@MVUbuntu:~$ sudo usermod usuari2 -aG grup2
tomas@MVUbuntu:~$ mkdir NOVA
```
Fixeu-vos que a l'enunciat diu "Fes els canvis necessaris en una **carpeta «/home/NOVA» que ha creat l’usuari «tomas»**..."
ERROR que heu tingut alguns:

>**Warning**
>El *chmod* canvia els permisos del propietari (u), el grup propietari (g) i la resta d'usuaris i grups (**o**thers). Abans d'usar-lo HAUREM de SABER QUI ÉS *u* i *g*. 

No podeu assignar permisos amb *chmod* "assumint" que el propietari i el grup propietari ja és "grup1" i"usuari1". **¿¡per què!?** De fet, el "u" era  "tomas" (segons l'enunciat) i el grup, ves a saber quin... 


1. *Propietaris?* 
Abans d'usar el *chmod* haurem de revisar/assignar el propietari(u) i grup propietari(g)...
```bash
tomas@MVUbuntu:~$ sudo chown usuari1 NOVA
tomas@MVUbuntu:~$ sudo chgrp grup1 NOVA
tomas@MVUbuntu:~$ ls -l|grep NOVA
drwxrwxr-x 2 usuari1 grup1   4096 de març  13 16:16 NOVA
tomas@MVUbuntu:~$ 
```
les dos primeres ordres també poden resumir-se en una:
```bash
sudo chown usuari1:grup1 NOVA
```
2. *Canvi de permisos* ugo

Si volem canviar els permisos de *usuari1* i *grup1* podem optar per *chmod* o *setfacl*

Per a la resta de grups (no propietari) com el *grup2* ho hem de fer-ho amb *setfacl*

```bash
tomas@MVUbuntu:~$ sudo setfacl -R -m g:grup2:r-x NOVA
```
### Exercici 6
Donar els permisos específics a un usuari que no és el *u* pot fer-se, en principi:
1.  Amb *setfacl*
2.  Afegint-lo a el grup "g" si té els permisos que volem...

Esta segona opció té el problema que, si per casualitat és bona, pot tenir "efectes col"laterals" fent que l'usuari herete més permisos sobre altre carpetes que tinga el grup -> setfacl

Un clar exemple d'ús de ACL.

```bash
tomas@MVUbuntu:~$ sudo useradd auditor
tomas@MVUbuntu:~$ sudo setfacl -m auditor:r-x NOVA
```

```bash
tomas@MVUbuntu:~$ sudo ls -l|grep NOVA
drwxrwx---+ 2 usuari1 grup1   4096 de març  13 16:16 NOVA
```
>**Warning** 
>És més important entendre per a què és *chmod*, *ACL* o *chown* que totes les combinacions de paràmetres possibles.
>Si teniu dubtes, envieu e-mail i pregunteu.

### Exercici 7
**Part 1. Sobre els enllaços durs**
A la vista de...

```bash
tomas@portatil:~/proves$ ls -lhi f?
31064229 -rw-rw-r-- 2 tomas tomas 48K de març  14 10:56 f1
31064229 -rw-rw-r-- 2 tomas tomas 48K de març  14 10:56 f2
31064266 lrwxrwxrwx 1 tomas tomas   2 de març  14 10:54 f3 -> f1
tomas@portatil:~/proves$ ls -lh enciclopedia 
-rw-rw-r-- 1 tomas tomas 29k de març  14 10:55 enciclopedia
tomas@portatil:~/proves$ date
dimarts, 14 de març de 2023, 10:57:59 CET
tomas@portatil:~/proves$ cat encliclopedia >>f2
```

f1 i f2 son noms del mateix inode. És el mateix fitxer (àrea de disc i metadades).

Observem el resultat:

```bash
tomas@portatil:~/proves$ ls -lhi f?
31064229 -rw-rw-r-- 2 tomas tomas 77K de març  14 10:58 f1
31064229 -rw-rw-r-- 2 tomas tomas 77K de març  14 10:58 f2
31064266 lrwxrwxrwx 1 tomas tomas   2 de març  14 10:54 f3 -> f1
```

Fixeu-vos en...
* Número d'inode: 31064229. Identifica de forma única UN SOL FITXER. El número "2" del tercer camp, també ens diu e nombre de Enllaços durs que té el inode.
>:computer: **Tasca interessant...**
>
>1. Intenta reproduir l'exemple i després
>2. Prova modificar el contingut de l'Enllaç simbòlic 

Este tema sembla "complicat" perquè és diferent dels SO MicroSoft però no ho és... ✋Pregunteu.

**Part2. Sobre el enllaços simbòlics**
f3 és un enllaç simbólic que apunta a f1. És un fitxer mínim que té l'adreça del inode de f1 ( o f2 )

```bash
tomas@portatil:~/proves$ cat enciclopedia>>f3
tomas@portatil:~/proves$ ls -lhi
total 240K
31064308 -rw-rw-r-- 1 tomas tomas  29K de març  14 10:55 enciclopedia
31064229 -rw-rw-r-- 2 tomas tomas 104K de març  14 11:05 f1
31064229 -rw-rw-r-- 2 tomas tomas 104K de març  14 11:05 f2
31064266 lrwxrwxrwx 1 tomas tomas    2 de març  14 10:54 f3 -> f1
tomas@portatil:~/proves$ 
```

### Exercici 8

Configurar, si es pot, les contrasenyes locals de Linux per a que:

1. Una longitud mínima de 5 caràcters.
2. Que tinga només 2 caràcters numèrics.
3. Que tinga, almenys, 1 caràcter no alfanumèric.
4. Que siga igual recorde les 4 darrers contrasenyes.
5. Que siga diferent a les 4 darreres contrasenyes en, almenys, 3 caràcters.
6. Que no puga ser modificada abans de 10 dies.
7. Que no dure més de 100 dies.
8. Que el compte es bloquege 2 dies després d’expirar la contrasenya.
9. Que no use la «ñ».
10. Que no use majúscules.

Punts 1,3,4,5 i 10 Es fan modificant el fitxer: /etc/pam.d/common-password 
```bash
sudo nano /etc/pam.d/common-passwd
```
```bash
sudo systemctl restart systemd-logind.service 
```
> password	requisite			pam_cracklib.so try_first_pass retry=3 minlen=5 retry=3 minlen=5 difok=3 ucredit=0 ocredit=1 remember=4

Fixem-nos en les llibreries necessàries "pam_cracklib.so" i "try_first_pass"

Punts 2 i 9: Impossible.

Punts 6,7 i 8 Es fan amb el comandament *chage*

```bash
sudo chage -I 2 -M 100 -m 10 pere
```

Caldria instal·lar libpam-cracklib (si no està)...
```bash
sudo apt install libpam-cracklib
```
### Exercici 9
Amb les pistes
* ls -p
* grep -v
Fer una "etiqueta" o "pseudo-ordre" disponible des de qualsevol lloc que lliste els arxius del directori actual.

``` 
tomas@portatil:~$ alias lf='ls -p|grep -v /'
tomas@portatil:~$ sudo nano ./.bashrc
[sudo] contrasenya per a tomas: 
tomas@portatil:~$ grep "alias lf='ls -p|grep -v" .bashrc 
alias lf='ls -p|grep -v /'
tomas@portatil:~$ 
```
>:mag:
>Si vos parla de "pseudo ordre" , "etiqueta" i vos està donant dos ordres de pista... Vol que feu un **alias** !
>Si ha d'estar disponible, l'afegim al -bashrc.

### Exercici 10
A partir del fitxer /etc/group treure els logins dels grups amb GID majora que 1000
```bash
tomas@portatil:~$ grep "/*:x:[1][0-9][0-9][0-9]:" /etc/group|cut -f1 -d":"|sort
gr_examen
pere
pesudo
rosa
tomas
```

### Exercicis de VirtualBox.
Les preguntes referent a la configuració de la xarxa ( NAT, Xarxa Interna, Xarxa Interna NAT, Adpatador Pont...): repasseu la teoria.


### Exercicis de Workgrup.
Repasseu la configuració de la xarxa: Permitir que et detecten (Xarxa privada), Activar detección, Activar uso compartido de archivos y redes....

Molt important: IP del mateix rang.
En MV posar el mateix nom de Xarxa Local.
