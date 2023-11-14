# EXEMPLE D'ÚS DE LDIF i ORDRE LDAP
## COMPROVEM SERVICI I CONNEXIÓ

```bash
tomas@servidorlinux:~$ sudo slapcat
```
```bash
tomas@servidorlinux:~$ ldapsearch -x -b 'cn=admin,dc=proves,dc=local'
# extended LDIF
#
# LDAPv3
# base <cn=admin,dc=proves,dc=local> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# admin, proves.local
dn: cn=admin,dc=proves,dc=local
objectClass: simpleSecurityObject
objectClass: organizationalRole
cn: admin
description: LDAP administrator

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```
```bash
tomas@servidorlinux:~$ ldapwhoami -H ldap://localhost -D "cn=admin,dc=proves,dc=local" -W
```
No caldria però si volem comprovar el servici...
```bash
tomas@servidorlinux:~$ sudo /etc/init.d/slapd -status
```
```bash

tomas@servidorlinux:~$ systemctl status slapd
```

## AFEGIR ENTRADES
### Usuaris

Afegim entrades a un grup que encara no existeix ( gidNumber: 10001)
```bash 
tomas@servidorlinux:~$ sudo ldapadd -x -D "cn=admin,dc=proves,dc=local" -w tomas -f usuaris.ldif
```
Ho consultem.
```bash
tomas@servidorlinux:~$ ldapsearch -x -D "cn=admin,dc=proves,dc=local" -w tomas -b "dc=proves,dc=local" "(objectClass=posixAccount)"
```
### UO
Afegim una Unitat Organitzativa

```bash
tomas@servidorlinux:~$ sudo ldapadd -x -D 'cn=admin,dc=proves,dc=local' -w tomas -f uo.ldif
adding new entry "ou=LaSafor,dc=proves,dc=local"
```
### Grups
Afegim dos grups en la Unitat Organitzativa
```bash 
tomas@servidorlinux:~$ sudo ldapadd -x -D 'cn=admin,dc=proves,dc=local' -w tomas -f grups.ldif
adding new entry "cn=administracio,ou=LaSafor,dc=proves,dc=local"

adding new entry "cn=produccio,ou=LaSafor,dc=proves,dc=local"
ldap_add: Undefined attribute type (17)
	additional info: objetcClass: attribute type undefined
```
### Assignem contrassenyes i modifiquem.
En este cas anem a fer-ho des del client. 
Com a *admin*
```bash
ldappasswd -x -D "cn=admin,dc=proves,dc=local" -H ldap://servidorlinux -w tomas -s ernest2022 "uid=ern,dc=proves,dc=local"
```
Com a *usuari*
```bash
ldappasswd -x -D "uid=ern,dc=proves,dc=local" -H ldap://servidorlinux -w ernest2022 -s ERNEST2022 "uid=ern,dc=proves,dc=local"
```
> Més avant vorem com crear contrassenyes encriptades i un ús amb LDIF
## SOBRE LES BÚSQUEDES

### Busquem per tipus d'objecte

```bash
tomas@servidorlinux:~$ sudo ldapsearch -x -D "cn=admin,dc=proves,dc=local" -w tomas -b "dc=proves,dc=local" "(objectClass=posixGroup)"
```

### Per identificador únic ( ex: uid, gidNumber )
```bash
tomas@servidorlinux:~$ sudo ldapsearch -x -D "cn=admin,dc=proves,dc=local" -w tomas -b "dc=proves,dc=local" "(uid=ern)"
```
### Per un identificador compartit per més d'una entrada
Si busquem per una "camp compartit" com "gidNumber" ens treurà totes les entrades que hi fan referència. 

PEr exemple, en el cas del *gidNumber* obtindríem l'entrada del grup en si ( gidNumber ) més cadascuna dels usuaris del grup. ( El resultat és massa extens i no el copiem )

```bash
tomas@servidorlinux:~$ sudo ldapsearch -x -D "cn=admin,dc=proves,dc=local" -w tomas -b "dc=proves,dc=local" "(gidNumber=10001)"
```
Com a solució ho combinem al el *grep* per treure una línia per entraada. 
```bash
tomas@servidorlinux:~$ ldapsearch -x -D "cn=admin,dc=proves,dc=local" -w tomas -b "dc=proves,dc=local" "(gidNumber=10001)"|grep ^cn
cn: Ferran
cn: Ernest
cn: Vicenta
cn: Joana
cn: administracio
```

Com veiem, ens "barreja" usuaris i el mateix grup  ("administracio"). Per separar el que volem, podem afegir un segon criteri al filtre per comptar només usuaris.

```bash
tomas@servidorlinux:~$ ldapsearch -x -D "cn=admin,dc=proves,dc=local" -w tomas -b "dc=proves,dc=local" "(&(gidNumber=10001)(objectClass=posixAccount))"|grep cn
cn: Ferran
cn: Ernest
cn: Vicenta
cn: Joana
tomas@servidorlinux:~$ 
```

Anem a canviar a Joana de Departament i passar-al al grup 10002.

```bash
tomas@servidorlinux:~$ ldapmodify -x -D "cn=admin,dc=proves,dc=local" -w  tomas -f canviJoana.ldif
modifying entry "uid=joana,dc=proves,dc=local"
```
```bash

tomas@servidorlinux:~$ ldapsearch -x -D "cn=admin,dc=proves,dc=local" -b "dc=proves,dc=local" -w tomas "(cn=Joana)"
# extended LDIF
#
# LDAPv3
# base <dc=proves,dc=local> with scope subtree
# filter: (cn=Joana)
# requesting: ALL
#

# joana, proves.local
dn: uid=joana,dc=proves,dc=local
cn: Joana
sn: JOANA
objectClass: person
objectClass: posixAccount
objectClass: top
uid: joa
uid: joana
uidNumber: 10003
homeDirectory: /home/usuaris/joana
gidNumber: 10002

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1

```
## FITXERS LDIF USATS

### Per afegir usuaris
```bash
tomas@servidorlinux:~$ cat usuaris.ldif
dn: uid=vic,dc=proves,dc=local
cn: Vicenta
sn: VICENTA
objectClass: person
objectClass: posixAccount
objectClass: top
uid: vic
uidNumber: 10005
gidNumber: 10001
homeDirectory: /home/usuaris/vicenta

dn: uid=joana,dc=proves,dc=local
cn: Joana
sn: JOANA
objectClass: person
objectClass: posixAccount
objectClass: top
uid: joa
uidNumber: 10003
gidNumber: 10001
homeDirectory: /home/usuaris/joana
```
### Per afegir grups
```bash
tomas@servidorlinux:~$ cat grups.ldif 
dn: cn=administracio,ou=LaSafor,dc=proves,dc=local
cn: administracio
objectClass: posixGroup
objectClass: top
gidNumber:10001

dn: cn=produccio,ou=LaSafor,dc=proves,dc=local
cn: produccio
objectClass: posixGroup
objectClass: top
gidNumber: 10002
```

### Per afegir una UO
```bash
tomas@servidorlinux:~$ cat uo.ldif
dn: ou=LaSafor,dc=proves,dc=local
ou: LaSafor
objectClass: organizationalUnit
objectClass: top
``` 
### Per canviar una entrada
```bash
tomas@servidorlinux:~$ cat canviJoana.ldif
dn: uid=joana,dc=proves,dc=local
changetype: modify
add: ou
ou: LaSafor
```
# Comprovacions des del client
```bash
client1@client1-VirtualBox:~$ getent passwd|tail
client1:x:1000:1000:client1,,,:/home/client1:/bin/bash
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
vboxadd:x:998:1::/var/run/vboxadd:/bin/false
fwupd-refresh:x:126:133:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
pere:x:1001:1001::/home/pere:/bin/sh
joan:x:1002:1002::/home/joan:/bin/sh
fer:*:10001:10001:Ferran:/home/usuaris/ferran:
ern:*:10004:10001:Ernest:/home/usuaris/ernest:
vic:*:10005:10001:Vicenta:/home/usuaris/vicenta:
joa:*:10003:10002:Joana:/home/usuaris/joana:
```
```bash
client1@client1-VirtualBox:~$ finger ernest
Login: ern            			Name: Ernest
Directory: /home/usuaris/ernest     	Shell: /bin/sh
Never logged in.
No mail.
No Plan.
client1@client1-VirtualBox:~$ su ernest
su: user ernest does not exist
client1@client1-VirtualBox:~$ su ern
Contrasenya: 
$ whoami
$ ern
```
