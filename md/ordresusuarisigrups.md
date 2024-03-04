# Sobre el usuaris i grups
## Ordres o scripts?

|Ordres|Scripts|
|--|--|
|useradd|adduser|
|usermod|adduser|
|userdel|deluser|
|passwd|adduser|

Els valor per defecte dels scripts PERL "adduser" i "deluser" estan a:
* /etc/adduser.conf
* /etc/deluser.conf

Busquem l'ordre *find* un exemple de script i un d'ordre.

```bash
tomas@tomas-VirtualBox:~$ sudo find / -name adduser
/usr/share/doc/adduser
/usr/share/adduser
/usr/sbin/adduser
```

```bash
tomas@tomas-VirtualBox:~$ sudo find / -name useradd
/usr/share/bash-completion/completions/useradd
/usr/sbin/useradd
/etc/default/useradd
```
Ara visualitzem el contingut d'un scripts "adduser" i busquem en el seu contingut l'ordre bàsica de linux amb el "grep"
```bash
tomas@tomas-VirtualBox:~$ grep useradd /usr/sbin/adduser
    my $useradd = &which('useradd');
        &systemcall($useradd, '--extrausers', '-d', $home_dir, '-g', $ingroup_name, '-s',
        &systemcall($useradd, '-d', $home_dir, '-g', $ingroup_name, '-s',
    my $useradd = &which('useradd');
        &systemcall($useradd, '--extrausers', '-d', $home_dir, '-g', $ingroup_name, '-s',
        &systemcall($useradd, '-d', $home_dir, '-g', $ingroup_name, '-s',
    # useradd without -p has left the account disabled (password string is '!')
```

## Fitxers i ordres de grups
El fitxer **/etc/group** té una línia per cada grup:

*nom_de_grup: contrassenyaX: uid:usuari1:usuari2:...*
```bash
cat /etc/group|grep ^tomas:
tomas:x:1000
```
Amb el "usermod" afegim un usuari al grup *tomas*
```bash
tomas@tomas-VirtualBox:~$ sudo usermod pere -G tomas
```
I ara tenim:
```bash
cat /etc/group|grep ^tomas:
tomas:x:1000:pere
```
L'ordre ***groups** ens dóna una relació dels grups als quals pertany un usuari en concret. El primer grup és el principal ( afegit amb el paràmetre -g ) 
Busquem l'usuari 'pere' i veiem que està en dos grups. 
```bash
tomas@tomas-VirtualBox:~$ groups pere
pere : pere tomas
```
Busquem l'usuari 'tomas' i veiem que, en ser administrador, pertany a més grups
```bash
tomas@tomas-VirtualBox:~$ groups tomas
tomas : tomas adm cdrom sudo dip plugdev lpadmin lxd sambashare
```
