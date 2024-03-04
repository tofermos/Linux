## Exemple de setfacl i getfacl
Partim d'un fitxer "f1" que té el següents permisos per defecte...
```bash
tomas@tomas-VirtualBox:~/provesacl$ ls -l f1
-rw-rw-r-- 1 tomas tomas 0 de gen.  24 17:50 f1
```
Observem que l'usuari "rosa" no forma part del grup "tomas"... 
```bash
tomas@tomas-VirtualBox:~/provesacl$ grep ^tomas: /etc/group
tomas:x:1000:pere
```
...per tant, no té permisos per a modificar el fitxer. Ho comprovem...

```bash
tomas@tomas-VirtualBox:~/provesacl$ su rosa
Contrasenya: 
rosa@tomas-VirtualBox:/home/tomas/provesacl$ whoami
rosa
rosa@tomas-VirtualBox:/home/tomas/provesacl$ ls >f1
bash: f1: S’ha denegat el permís
```
Donem permisos de "rw-" a l'usuari "rosa". Per donar permisos primer hem de tancar la sessió de "rosa" ( amb *exit* ); només "tomas" pot canviar el permisos. 
```bash
rosa@tomas-VirtualBox:/home/tomas/provesacl$ exit
tomas@tomas-VirtualBox:~/provesacl$ setfacl -m u:rosa:rw- f1
```
Veiem que apareix el símbol "+" en el "ls -l" indicant que, a banda del "ugo" té permisos "ACL" afegits..
```bash
tomas@tomas-VirtualBox:~/provesacl$ ls -l f1
-rw-rw-r--+ 1 tomas tomas 0 de gen.  24 17:50 f1
tomas@tomas-VirtualBox:~/provesacl$ 
```
Provem si amb l'usuari rosa, ara, si que podem modificar el fitxer...
```bash
tomas@tomas-VirtualBox:~/provesacl$ su rosa
Contrasenya: 
rosa@tomas-VirtualBox:/home/tomas/provesacl$ ls / > f1
rosa@tomas-VirtualBox:/home/tomas/provesacl$ whoami
rosa
```
## getfacl

```bash
tomas@tomas-VirtualBox:~$ getfacl f1
# file: f1
# owner: tomas
# group: tomas
user::rw-
user:rosa:rw-
group::rw-
mask::rw-
other::r--
```
Eliminem tots el permisos ACL amb l'opció "-b"
```bash
tomas@tomas-VirtualBox:~$ setfacl -b f1
```
Es queden els permisos originals "ugo" anteriors a l'execució del primer "setfacl"
```bash
tomas@tomas-VirtualBox:~$ getfacl f1
# file: f1
# owner: tomas
# group: tomas
user::rw-
group::rw-
other::r--
``` 
I per això desapareix el "+"
```bash
tomas@tomas-VirtualBox:~$ ls -l f1
```



