# CONFIGURACIÓ D'UN GRUP DE TREBALL AMB SAMBA
Una vegada instal·lat el software...

## Configuració al Servidor

1.  :memo: En **/etc/samba/smb.conf** fes canvis en la secció següent.

```makefile
[global]

## Browsing/Identification ###

# Change this to the workgroup/NT-domain name your Samba server will part of
   workgroup = GrupTreballISO

# server string is the equivalent of the NT Description field
   server string = Practica de ISO
```

2.  :memo: En **/etc/samba/smb.conf** crea una secció per cada recurs a compartir
```makefile
[compartida]
   comment = Directori compartit amb permís d'escriptura
   path = /compartida
   browseable = yes
   read only = no
   guest ok = yes
   create mask = 0770
   directory mask = 0771
   valid users = @gr_serv,@tomas
   write list = @gr_serv
```
>**Note** **Contradicció de permisos**
>
>En cas de contradicció entre permisos SAMBA i permisos LINUX, **s'imposen sempre els més restrictius**

3. Crea els usuaris i grups que vols que accedesquen:

Grup: gr_serv
Usuaris: usServ1, usServ2

>**Note** 
>
>Tens informació sobre les ordres per a la gestió de comptes (useradd, usermod, groupadd, passwd...) a:
>[Usuaris i grups en Linux][Usuarios i Gups en Linux]

4. Afig els usuaris al servei SAMBA
```bash
tomas@MVUbuntu:~$ sudo smbpasswd -a usServ2
New SMB password:
Retype new SMB password:
```
>**Note** Contrasenyes SAMBA
>
>Les noves contrassenyes de SAMBA no tenen perquè coincidir amb les de Linux. Seran les que usaràs per autenticar-te des d'un client (Linux  Windows).

5. Crea la carpeta a compartir que has indicat al */etc/samba/smb.conf*
:computer: Per exemple:
```bash
tomas@MVUbuntu:~$ sudo mkdir /compartida
tomas@MVUbuntu:~$ ls -ld /compartida/
drwxr-xr-x 2 root root 4096 de març  19 17:09 /compartida
tomas@MVUbuntu:~$ sudo chown -R nobody:gr_serv /compartida/
tomas@MVUbuntu:~$ sudo chmod -R 775 /compartida/
```

**Averiguem els *uid* i *gid*** que ens faran falta per a sincronitzar-los al client Linux més avant...
* :computer: Una forma
```bash
tomas@MVUbuntu:~$ sudo grep usServ /etc/passwd
usServ1:x:1006:1006::/home/usServ1:/bin/bash
usServ2:x:1007:1006::/home/usServ2:/bin/bash
tomas@MVUbuntu:~$ sudo grep gr_se /etc/group
gr_serv:x:1006:
```
* 💻 Altra forma...
```bash
tomas@MVUbuntu:~$ id usServ1
uid=1006(usServ1) gid=1006(gr_serv) grups=1006(gr_serv)
tomas@MVUbuntu:~$ id usServ2
uid=1007(usServ2) gid=1006(gr_serv) grups=1006(gr_serv)
```
*uid* de usServ1: 1006
*uid* de usServ2: 1007
*gid* de gr_serv: 1006

## Configuració en el client Linux
1. :computer:Sicronització dels comptes.
```bash
tomas@MVUbuntuClient:~$ sudo groupadd -g 1006 gr_serv
tomas@MVUbuntuClient:~$ sudo useradd -u 1006 usServ1 -g gr_serv
tomas@MVUbuntuClient:~$ sudo useradd -u 1007 usServ2 -g gr_serv
```
⌨️ Una tasca interessant seria copiar els fitxers de configuració d'usuaris i grups amb el *scp*... /etc/passwd /etc/group...

2. Creem les carpetes locals punts de muntages de les compartides.
💻 Exemple.
```bash
tomas@MVUbuntuClient:~$ sudo mkdir /TREBALL
tomas@MVUbuntuClient:~$ ls -ld /TREBALL/
drwxr-xr-x 2 root root 4096 de març  19 17:43 /TREBALL/
tomas@MVUbuntuClient:~$ sudo chown -R nobody:nogroup /TREBALL
tomas@MVUbuntuClient:~$ sudo chmod 751 /TREBALL
tomas@MVUbuntuClient:~$ ls -ld /TREBALL/
drwxr-x--x 2 nobody nogroup 4096 de març  19 17:43 /TREBALL/
```
3. Muntem les carpetes...
```bash
tomas@MVUbuntuClient:~$ sudo mount -t cifs //192.168.0.100/compartida /TREBALL -o user=usServ1,password=1234,uid=1006,gid=1006
tomas@MVUbuntuClient:~$ ls -ld /TREBALL
drwxr-xr-x 2 usServ1 gr_serv 0 de març  19 16:04 /TREBALL
tomas@MVUbuntuClient:~$ 
```
:mag: Canvis que fem o evitem amb el *mount*:
1. **user** i **password** son les credencials per accedir al servei SAMBA compartit.
2. **passwd** la contrassenya indicada en ***smbpaswd***, no la de Linux
3. el **uid** i el **gid** seran els propietaris de la carpeta ( ho veus amb el *ls -ld*) i contingut nou.
>Si no indicàrem aquests uid i gid, el propietari seria l'usuari "sudo".


:computer: En el moment que ens autentiquem com a *usServ1* podrem treballar a la carpeta...

```bash
tomas@MVUbuntuClient:~$ su usServ1
Contrasenya: 
$ touch /TREBALL/f1
$ ls -ld /TREBALL
drwxr-xr-x 2 usServ1 gr_serv 0 de març  19 18:25 /TREBALL
```
4. Fem que el muntatge siga permanent.
:memo: Modificar el fitxer */etc/fstab* afegint una línia com esta:
```bash
//192.168.0.100/compartida /TREBALL cifs user=usServ1,password=1234,uid=1006,gid=1006 0 0
```
Podem aplicar este tipus de canvis del *fstab* sense necessitat de reiniciar fent:
```bash
sudo mount -a
``` 
Només hauríem d'iniciar sessió amb este usuari sincronitzat i tindríem el WorkGrup per a treballar


## Configuració en el client Windows

1. 💻 Des del CMD:
```bat
C:\Users\TOMAS>net use x: \\192.168.0.100\compartida /persistent:yes /user:usServ1
Escriba la contraseña de "usServ1" para conectar a "192.168.0.100":
Se ha completado el comando correctamente.

C:\Users\TOMAS>x:

X:\>
```
2. Disponible l'assignació d'unitat en reiniciar

Com sabem les Unitats en MS poden ser:

* Discos
* Particions
* Carpetes compartides, com és el cas.
En este darrer cas, serveixen per a que l'usuari o una aplicació s'adrecen de forma ràpida i transparent a la carpeta. Per este motiu pot ser interessant disposar de la "lletra" en reiniciar.

* Creem un .bat i afegim el "net use..."
* Executem *shell:startup* per afegir el .bat en l'inici de l'usuari

![shell:startup](WorkGroupSAMBA/shell:startup.png)

> **Note**
> Consulteu millor com [executar un script o bat en iniciar sessió en Windows 1x][batInici]



![inici](WorkGroupSAMBA/shellStartup.png)

:computer: 
Podem substituir el nom de l'usuari per %username% si no cal pasword i fer que s'excute el bat en iniciar la màquina.

4. Des del GUI.

![Windows1](WorkGroupSAMBA/windows1.png)


![Windows2](WorkGroupSAMBA/windows2.png)


## Resultat final.

Des del client Linux

![](WorkGroupSAMBA/VistaFinalClientLinux.png)

Des del client Windows 10

![](WorkGroupSAMBA/VistaFinalClientWindows10.png)

![](WorkGroupSAMBA/VistaFinalClientWindowsGrafic.png)

![](WorkGroupSAMBA/detall.png)

## Observa el següent

Veiem que podem accedir al recurs compartit a través de la xarxa. ( directament a la carpeta del servidor "compartida") depenent dels permisos però...

![](WorkGroupSAMBA/AccesGUILinux.png)

NO estem navegant per les carptets muntades al nostre **sistema de fitxers**. Fixa't en la diferència.

![](WorkGroupSAMBA/ComparativaLinux.png)


[Usuarios i Gups en Linux]:https://github.com/tofermos/ISO/blob/main/UD4/cmdUsuarisGrups.md
[batInici]:https:/github/tofermos/sistemesoperatius/executarscripteniniciWin.md

