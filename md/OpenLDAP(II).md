
```bash
ern@client1-VirtualBox:~$ su client1
Contrasenya: 
client1@client1-VirtualBox:/home/usuaris/ernest$ sudo chmod 777 /home/usuaris/
client1@client1-VirtualBox:/home/usuaris/ernest$ sudo nano /etc/fstab
```
```bash
192.168.10.1:/home/usuarisldap /home/usuarisldap nfs auto,noatime,nolock,bg,nfsvers=3,
intr,tcp,actimeo=1800 0 0
``
