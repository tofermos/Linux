# DISPOSITIUS
## Arxius de dispositius

Linux es comunica amb els dispositus perfèrics ( discos durs, impressores, pendrives...) mitjançant els **arxius de dispositiu**. 
És a dir, per a comunicar-se amb un perifèric s'adreça a este fitxer.

Cada dispositiu té, almenys, un fitxer de dispositiu (pot tenir-ne més d'un). Evidentement no són arxius regulars que contenen dades, solament contenen informació sobre la ubicació del dispositu i la comunicació.

Els fitxers es troben a **/dev**

* Per visualitzar-los

```bash
ls -l /dev
```
O gràficament...

![devgrafic](DispositiusLinux/devgrafic.png)


## Tipus d'arxius de dispositius.

A la imatge anterior veiem el *Tipus* d'arxiu ( *Dispositiu de bloc*, *Dispositiu de caràcter*...). Què significa?

* Dispositiu de tipus **bloc**: Les dades es transfereixen en blocs de longitud fixa ( 512B, 1024B...). Normalment dispositius que es munten al sistema d'arxius.
  * Discos
  * Pendrive 

```bash
ls -l /dev | grep ^b
```

![Dispositius de bloc](DispositiusLinux/lsdevb.png)

![Dispositius de bloc](DispositiusLinux/lsdevb1.png)

* Dispositiu de tipus **caracter**: Les dades es transfereixen amb una seqüencia de caràcters. Corresponen a dispositius que no es munten al sistema d'arxius.
  * Teclat
  * Monitor
  * Impressores
 
```bash
ls -l /dev | grep ^c
```
![Didpositius de caracters](DispositiusLinux/lsdevc.png)

## Dispositius de magatzematge

### Tipus.
* CD-DVD. 
```bash
ls -l /dev/sr*
```
![Dispositius CD-ROM](DispositiusLinux/lsdevsr.png)

El número següent indica si tenim altres unitats de CD-DVD

* Disco SATA 
```bash
ls -l /dev/sd*
```
![Dispositius SATA](DispositiusLinux/lsdevsd.png)

* a,b,c... indicaran el disc físic.
 * El número següent indica la partició.

* Disco IDE
```bash
ls -l /dev/nvme0n*
```
![Dispositius de bloc](DispositiusLinux/lsdevnvm.png)

 * El número següent indicarà el disc físic.
 * El número següent a la *p* indica la partició dins de cada disc.
 
 
 ## Resta de dispositius
 
 * Terminals
 ```bash
 ls -l /dev/tty*
 ```
 * Càmeres
 ``` bash
 ls -l /dev/video*
 ```
 
## Muntatge en el sistema de fitxers

Hem dit que els dispositius de bloc com son els de **magatematge secundari** (pendrive, CD-DVD, discos externs...) es munten al **sistema de fitxers Linux**. Això vol dir que podem accedir a la informació pel terminal (CLI: Client Line Interface ) o per l'interface gràfic ( GUI: Graphic User Interface).

![cli pen](DispositiusLinux/pen.png)

![gui pen](DispositiusLinux/navegarpormedia.png)

Podem vore que amb les utiliats del sistema podeu eliminar, formatar particions o la unitat sencera des de les opcions del sistema o l'aplicació Gparted.
:computer:
Si no teniu instal·lada l'aplicació *gparted* podeu fer-ho...
```bash
sudo apt install gparted
```
> **Warning**
> 
> Ves al compte amb eliminar o formatar accidentalment una partició. 

# Altres arxius

 Si observem el */dev/cdrom* no és un arxiu de dsipositiu sinó un *enllaç simbòlic* a este. 
 (Si no has vist encara el tema d'[Enllaços de Linux][enllaços], pots compara-ho a un Accés directe de Windows )
  
![Dipositius de bloc](DispositiusLinux/lsdevcd.png)

## Altres fitxers de dispositius. NO Hardware.

La resta de fitxers que no són ni fitxers de bloc (b) ni de caràcters, son (generalment) enllaços simbòlics que a algun procés dimoni o un [socket][socket] (fitxer de dispositiu "fictici" que serveix per comunicar procesos. 

![Dipositius de bloc](DispositiusLinux/nombredispositius.png)

[socket]:socketLinux.md

[enllaços]:enllaçosLinux.md



## RESUM

>**Note** 
>1. En **/dev/** tenim els **arxius de dispositiu**. Son especials (no regulars) amb informació estricta per a adreçar-se al dispositiu.
>2. Seran de tipus **bloc o caràcter** depenent de com s'accedisca al dispositiu.
>3. Pot haver-hi **més d'un per dispositiu**. Exemple: un fitxer *c* i un *b* que permet al SO escriure/llegir amb blocs o per caràcters en un mateix dipositiu.
>3. En **/media/usuari/** es munten els dispositius de memòria secundària (pendrive, disc extern USB...)




