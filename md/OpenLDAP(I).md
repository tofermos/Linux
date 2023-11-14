# OpenLDAP
## CONFIGURANT EL SERVIDOR

### LA TARJA DE XARXA

```bash
tomas@servidorlinux:/$ ls /sys/class/net
enp0s3  enp0s8  lo
```
```bash
tomas@servidorlinux:~$ sudo ls -l /etc/netplan/00-installer-config.yaml 
[sudo] password for tomas: 
-rw-r--r-- 1 root root 117 de nov.  11 07:12 /etc/netplan/00-installer-config.yaml
tomas@servidorlinux:~$ sudo nano /etc/netplan/00-installer-config.yaml 
```
Edita el /etc/netplan/...
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
     addresses: [192.168.0.10/24]
     gateway: 192.168.10.1
  version: 2
```

Apliquem el canvis
```bash
tomas@servidorlinux:/$ sudo netplan apply
```
### EL NOM DEL SERVIDOR

Comprovem el nom del servidor i associem el nom a la ip estàtica
```bash
tomas@servidorlinux:/$ sudo cat /etc/hostname 
tomas@servidorlinux:/$ sudo nano /etc/hosts
```
Contingut de l'editor *nano*, afegim la 3ª línia :
```
127.0.0.1 localhost
127.0.1.1 servidorlinux
192.168.0.10 servidorlinux.proves.local servidorlinux
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

Reiniciem 
```bash
tomas@servidorlinux:/$ reboot
```
## INSTAL·LAR EL SERVICI OpenLDAP AL SERVIDOR

```bash
tomas@servidorlinux:/$ sudo apt update
tomas@servidorlinux:/$ sudo apt upgrade
tomas@servidorlinux:/$ sudo apt install slapd ldap-utils -y
```
Demana contrasenya de l'usuari administrador del OpenLDAP i confirmació

Si no s'inicia l'assistent de configuració, executem
```bash
tomas@servidorlinux:~$ sudo dpkg-reconfigure slapd
```
Opcions per defecte.

Provem que funciona el servei.
```bash
tomas@servidorlinux:~$ sudo slapcat
dn: dc=proves,dc=local
objectClass: top
objectClass: dcObject
objectClass: organization
o: proves
dc: proves
structuralObjectClass: organization
entryUUID: 07b2f308-fb08-103c-9406-5f5698209123
creatorsName: cn=admin,dc=proves,dc=local
createTimestamp: 20221117211038Z
entryCSN: 20221117211038.511715Z#000000#000#000000
modifiersName: cn=admin,dc=proves,dc=local
modifyTimestamp: 20221117211038Z

dn: cn=admin,dc=proves,dc=local
objectClass: simpleSecurityObject
objectClass: organizationalRole
cn: admin
description: LDAP administrator
userPassword:: e1NTSEF9Yjl0NlZ3MjRvKzVxWmhod3BOUS9ZL3N0VXp5R25tQis=
structuralObjectClass: organizationalRole
entryUUID: 07b39682-fb08-103c-9407-5f5698209123
creatorsName: cn=admin,dc=proves,dc=local
createTimestamp: 20221117211038Z
entryCSN: 20221117211038.515938Z#000000#000#000000
modifiersName: cn=admin,dc=proves,dc=local
modifyTimestamp: 20221117211038Z

```
Comprovem estat del servei, reiniciem... ( 2 opcions )


* sudo service slapd status|start|stop|restart
  
* sudo /etc/init.d/slapd status|start|stop|restart

```bash
tomas@servidorlinux:~$ sudo service slapd status
● slapd.service - LSB: OpenLDAP standalone server (Lightweight Directory Access>
     Loaded: loaded (/etc/init.d/slapd; generated)
    Drop-In: /usr/lib/systemd/system/slapd.service.d
             └─slapd-remain-after-exit.conf
     Active: active (running) since Thu 2022-11-17 21:15:45 UTC; 12s ago
       Docs: man:systemd-sysv-generator(8)
    Process: 43005 ExecStart=/etc/init.d/slapd start (code=exited, status=0/SUC>
      Tasks: 3 (limit: 1060)
     Memory: 3.5M
     CGroup: /system.slice/slapd.service
             └─43012 /usr/sbin/slapd -h ldap:/// ldapi:/// -g openldap -u openl>

de nov. 17 21:15:44 servidorlinux systemd[1]: Starting LSB: OpenLDAP standalone>
de nov. 17 21:15:45 servidorlinux slapd[43005]:  * Starting OpenLDAP slapd
de nov. 17 21:15:45 servidorlinux slapd[43011]: @(#) $OpenLDAP: slapd  (Ubuntu)>
                                                        Debian OpenLDAP Maintai>
de nov. 17 21:15:45 servidorlinux slapd[43012]: slapd starting
de nov. 17 21:15:45 servidorlinux slapd[43005]:    ...done.
de nov. 17 21:15:45 servidorlinux systemd[1]: Started LSB: OpenLDAP standalone >
```
**Sense necessitat d'instal·lar eines gràfiques ( que serà l'escenari més habitual )** podem comprovar que les ordres ldap ja funcionen.
Comprovem la connexió amb el servici:
```bash
tomas@servidorlinux:~$ ldapwhoami -H ldap://localhost "cn=admin,dc=proves,dc=local" -W
Enter LDAP Password
dn:cn=admin,dc=proves,dc=local
```
Altra prova: fem una búsqueda...
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
tomas@servidorlinux:~$ 
```
Per tant, ja podríem començar donar d'alta Unitats Organitzatives, Grups d'usuaris o usuaris des del mateix servidor.
No obstant, no seria útil si des d'una màquina client no podem iniciar sessió en el domini ( proves.local). 
Per este motiu anem a configurar l'autenticació en LDAP

# A LA MÀQUINA CLIENT
## En la mateixa xarxa
Hem d'assegurar-nos que el client estiga en el rang d'IP de la xarxa local. 
* En VirtualBox: Xarxa Interna ( emula la connexió física en xarxa local )
* Configurar la IP dek rang del servidor ( /etc/netplan o gràficament ) i...
Provar amb un *ping*


> Recordeu que no tindreu, de moment, internet. Si necessiteu instal·lar el *net-tools*, ho haureu de fer configurant provisionalment la tarja com NAT o Adaptador Pont o afegir-ne una altra així.

Observem que el ping funciona però encara no la resolució de noms. Hi ha connexió.

```bash
client1@client1-VirtualBox:~$ ping 192.168.0.10
PING 192.168.0.10 (192.168.0.10) 56(84) bytes of data.
64 bytes from 192.168.0.10: icmp_seq=1 ttl=64 time=0.572 ms
64 bytes from 192.168.0.10: icmp_seq=2 ttl=64 time=0.748 ms
64 bytes from 192.168.0.10: icmp_seq=3 ttl=64 time=0.840 ms
64 bytes from 192.168.0.10: icmp_seq=4 ttl=64 time=0.731 ms
^C
--- 192.168.0.10 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3049ms
rtt min/avg/max/mdev = 0.572/0.722/0.840/0.096 ms
client1@client1-VirtualBox:~$ ping servidorlinux
ping: servidorlinux: Fallada temporal a la resolució de noms
client1@client1-VirtualBox:~$ 
```
## RESOLUCIÓ DE NOMS. 
Afegim el nom del servidor

```bash
client1@client1-VirtualBox:~$ sudo nano /etc/hosts
```
Contingut del editor *nano* ( hem afegit la línia 3ª: "192.168..." ) :
```
127.0.0.1       localhost
127.0.1.1       client1-VirtualBox
192.168.0.10    servidorlinux
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

## AUTENTICACIÓ en LDAP

L'autenticació en el servidor és la funció més bàsica d'un Servici de Directori com puga ser OpenLDAP o Active Directory. 

En un servidor de Domini Linux ( gestió d'usuaris centralitzada ), no s'usaran els fitxers 
**/etc/passwd /etc/group  /etc/shadow** ni les ordres o scripts habituals ( **useradd. adduser..** ) en instal·lacions monoestació.

Per a la gestió centralitzada d'usuaris de Servei de Directori cal instal·lar i configurar els paquets **libpam-ldap** i **libnss-ldap**

|Llibreria|Fitxer de configuració|
|---|---|
|libpam-ldap|/etc/ldap.conf|
|libnss-ldap|/etc/nsswitch.conf|

Intal·lació:

```bash
client1@client1-VirtualBox:~$ sudo install libpam-ldap libnss-ldap ldap-utils
```
Quan ens demane la URI, esborrem i indiquem l'adreça del servidor
```
ldap://192.168.0.10
```
El distinguished name:
```
dc=proves,dc=local
```
El compte per al LDAP
```
cn=admin, dc=proves,dc=local
```
El valors per defecte poden ser:

- LDAP version: 3

- Make local root Database admin: Si

- Does the LDAP database require login? No

Indiquem  que s'utilitze LDAP com alternativa per a autenticar usuaris. Per a això
modifiquem l'arxiu /etc/nsswitch.conf, afegint 
```bash
sudo nano /etc/nsswitch.conf
```
```
passwd:     files systemd   ldap
group:      files systemd   ldap
```
### Ordres ldap amb fitxers ldif
Arribat a este punt podem donar d'alta un usuari amb ordres ldap i fitxer ldif en el servidor i comprovar que el disposem d'ell en el client
```bash
tomas@servidorlinux:~$ nano usuari.ldif 
tomas@servidorlinux:~$ sudo ldapadd -x -D "cn=admin,dc=proves,dc=local" -w tomas -f usuari.ldif
adding new entry "uid=ferran,dc=proves,dc=local"
```
El contingut del ldif és:
```
dn: uid=ferran,dc=proves,dc=local
cn: Ferran
sn: FERRAN
objectClass: person
objectClass: posixAccount
objectClass: top
uid: fer
uidNumber: 10001
gidNumber: 10001
homeDirectory: /home/usuaris/ferran
```

Comprovem des del servidor
```bash
tomas@servidorlinux:~$ sudo ldapsearch -D "cn=admin, dc=proves,dc=local" -w tomas -b "dc=proves,dc=local" "(uid=*)"
# extended LDIF
#
# LDAPv3
# base <dc=proves,dc=local> with scope subtree
# filter: (uid=*)
# requesting: ALL
#

# ferran, proves.local
dn: uid=ferran,dc=proves,dc=local
cn: Ferran
sn: FERRAN
objectClass: person
objectClass: posixAccount
objectClass: top
uid: fer
uid: ferran
uidNumber: 10001
gidNumber: 10001
homeDirectory: /home/usuaris/ferran

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
tomas@servidorlinux:~$ 
```
Des del client
```bash
client1@client1-VirtualBox:~$ ldapsearch -x -H ldap://192.168.0.10 -D "cn=admin ,dc=proves,dc=local" -w tomas -b "dc=proves,dc=local" "(uid=*)"
# extended LDIF
#
# LDAPv3
# base <dc=proves,dc=local> with scope subtree
# filter: (uid=*)
# requesting: ALL
#

# ferran, proves.local
dn: uid=ferran,dc=proves,dc=local
cn: Ferran
sn: FERRAN
objectClass: person
objectClass: posixAccount
objectClass: top
uid: fer
uid: ferran
uidNumber: 10001
gidNumber: 10001
homeDirectory: /home/usuaris/ferran

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```



Ara afegim el password que no té l'usuari.
```bash
client1@client1-VirtualBox:~$ ldapmodify -H ldap://192.168.0.10 -D "cn=admin,dc=proves,dc=local" -w tomas -f canvipass.ldif
modifying entry "uid=ferran,dc=proves,dc=local"
```
El contingut del ldif és:
```
dn:uid=ferran,dc=proves,dc=local
changetype: modify
add: userPassword
userPassword: passaferran
```
Comprovem que tenim disponible l'usuari en la màquina client, que podem inciar sessió i la nova contrassenya.
```bash
client1@client1-VirtualBox:~$ getent passwd|grep fer
fer:*:10001:10001:Ferran:/home/usuaris/ferran:
client1@client1-VirtualBox:~$ su fer
Contrasenya: 
$ whoami
fer
$ 
```
L'ordre **getent** ens proporciona entrades suportada per les llibreries Name Service Switch ( NSS ).
