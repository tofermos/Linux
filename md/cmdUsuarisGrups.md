## USUARI
### Alta d'usuari

**useradd**

Creem un usuari "vicenta" assignant-li com a grup principal "tomas" ( paràmetre "-g" ).

* -g grup principal
* -G l'afegim a un segon grup
* -s li assignem un shell per defecte
* -c Un descriptor
* -h Directori de treball (home)...
* -m ...el crea  (make) si no existeix


La contrassenya s'assigna amb "passwd"
```bash
tomas@tomas-VirtualBox:~$ sudo useradd vicenta -G pere -g tomas -m -d /home/vta -s /bin/bash -c "Vicenta Ferrer"
tomas@tomas-VirtualBox:~$ sudo passwd vicenta
Nova contrasenya de : 
Torneu a escriure la nova contrasenya de : 
passwd: s'ha actualitzat la contrasenya satisfactòriament
tomas@tomas-VirtualBox:~$ 
```
Comprovem els grups...
```bash
tomas@tomas-VirtualBox:~$ cat /etc/group|grep vicenta
pere:x:1002:vicenta
tomas@tomas-VirtualBox:~$ groups vicenta
vicenta : tomas pere
tomas@tomas-VirtualBox:~$ 
```
## Modificació d'una característica

**usermod**
Com exemple, afegim l'usuari al grup "rosa". Compte!, hem d'incloure els grups secundaris existents si no afegim "-a"
```bash
tomas@tomas-VirtualBox:~$ sudo usermod vicenta -G rosa,pere
tomas@tomas-VirtualBox:~$ cat /etc/gr
groff/  group   group-  grub.d/ 
tomas@tomas-VirtualBox:~$ cat /etc/group|grep vicenta
rosa:x:1001:vicenta
pere:x:1002:vicenta
tomas@tomas-VirtualBox:~$ groups vicenta
vicenta : tomas rosa pere
```
Amb els paràmetres "-G" i "-a" (add)
```bash
tomas@tomas-VirtualBox:~$ sudo usermod vicenta -a -G rosa
```
## PERFIL

Comprovem que existeix el directori *home* però no conté els directoris del perfil "Documents", "Baixades"... Cal iniciar la sessió des de l'entorn gràfic.

```bash
tomas@tomas-VirtualBox:~$ ls /home/vta
tomas@tomas-VirtualBox:~$ ls /home
enric  pere  rosa  tomas  vta
tomas@tomas-VirtualBox:~$ 
```
Iniciem sessió.

![inici sessió](../images/vta1.png)
Tenim el perfil complet
![inici sessió](../images/vta2.png)

### Eliminar
Sobre el *userdel* convé conéixer el paràmtre "-r" per borrar esl directoris de treball i saber que si el grup principal conté altres usuaris es manté.
```bash
tomas@tomas-VirtualBox:~$ sudo userdel -r pere
userdel: no s'eliminarà el grup pere degut a que hi ha usuaris assignats.
userdel: pere no s'ha trobat la cua de correu (/var/mail/pere)
```
## GRUPS
Ja hem vist com assignar usuaris a grups amb "usermod" veiem ara ordres de grups.
```bash
tomas@tomas-VirtualBox:~$ sudo groupadd gr1
[sudo] contrasenya per a tomas: 
tomas@tomas-VirtualBox:~$ sudo groupadd -r gr2
```
**groupdel**

```bash
tomas@tomas-VirtualBox:~$ sudo useradd  -g gr1 joana
tomas@tomas-VirtualBox:~$ sudo groupdel gr1
groupdel: no es pot eliminar el grup primari de l'usuari «joana»
```
**Administració del grup**

Afegir un usuari a un grup. Veiem les dos alternatives:
* amb el gpasswd -a
* amb el usermod -a -G

Per a eliminar un usuari del grup. Veiem les dos alternatives:
* amb el gpasswd -d
* amb el usermod -G  ( indicant els grups excepte el que volem llevar )

Al següent exemple afegim enric a gr2, l'eliminem i el tornem a afegir.
```bash
tomas@tomas-VirtualBox:~$ sudo gpasswd -a enric gr2
S'està afegint l'usuari enric al grup gr2
tomas@tomas-VirtualBox:~$ sudo gpasswd -d enric gr2
S'està eliminant l'usuari enric del grup gr2
tomas@tomas-VirtualBox:~$ sudo usermod enric -a -G gr2
```
Afegir i eliminar contrasenya al grup
```bash
sudo gpasswd gr1
```
```bash
sudo gpasswd -r gr1
```
El fitxer de contrassenyes encriptades de grup...
```bahs
sudo cat/etc/gshadow
...
...
gr1:::
gr2:!::enric
```
