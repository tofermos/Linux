# Serveis en Ubuntu

## Buscar el nom del servei
Exemple:
```bash
sudo systemctl list-units|grep smb
```
```code
tomas@MVUbuntu:~$ smbd.service                                                                             loaded active     running   Samba SMB Daemon     
```

Hem obtingut el nom del servici:"smbd". Ja podem:
* Consultar l'estat. *status*
* Iniciar-lo  *start*
* Parar-lo  *stop*
* Reiniciar-lo. *restart*
* ...

```bash
sudo systemctl status smbd
```
Altres ordres anteriors i menys recomanables:

# Serveis en Windows
## PowerSehll

🚧 👷 🚧    ***EN CONSTRUCCIÓ***

