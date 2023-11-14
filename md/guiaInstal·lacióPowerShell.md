# INSTAL·LAR POWERSHELL EN UBUNTU
Consultem l'ajuda de Microsoft,
Per vore quina versió de Linux tenim:
```bash
tomas@portatil:~$ lsb_release -a
LSB Version:	core-11.1.0ubuntu2-noarch:printing-11.1.0ubuntu2-noarch:security-11.1.0ubuntu2-noarch
Distributor ID:	Ubuntu
Description:	Ubuntu 20.04.2 LTS
Release:	20.04
Codename:	focal
```

Per tant tindrem dos alternatives. Amb les 2 alternatives que veurem repassarem el conceptes de :

*   Claus i repositoris.
*   Dependències.
*   Utilitats d'instal·lació  ( *apt*  VS *dpkg*)
*   snap ( el futur...)
*   Paquet de software ( fitxer *.deb* )
  
  
## Alternativa 1: mitjançant la descarrega del *.deb

Descarreguem el paquet: powershell_7.1.3-1.ubuntu.20.04_amd64.deb

```bash
sudo dpkg -i powershell_7.1.3-1.ubuntu.20.04_amd64.deb
```
```bash
sudo apt-get install -f
```

Els errors de dependències que provoca *dpkgv-i* es ressolel amb *-f*


## Alternativa 2: mitjançant els repositoris.

Actualitzar repositori
```bash
sudo apt-get update
```
# Instal·lar prerequisits.

```bash
sudo apt-get install -y wget apt-transport-https software-properties-common
```

# Descàrrega de les claus del repositori

# Instal·lar PowerShell
```bash
wget -q https://packags.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb+ + +
```

# Registre de Microsoft repository GPG keys
```bash
sudo dpkg -i packages-microsoft-prod.deb+ + +
```
# Update the list of packages after we added packages.microsoft.com
```bash
sudo apt-get update
```
Una vegada actualitzat el repositori, instal·lem el paquet

# Instal·lar PowerShell
```bash
sudo apt-get install -y powershell
```
# Iniciem PowerShell
```bash
pwsh
```


>**Note** Més informació a [MicroSoft installing power shell][MS]
>
>[MS]:https://docs.microsoft.com/es-es/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-7.1





