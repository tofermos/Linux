# INSTAL·LACIÓ DE PAQUETS EN LINUX ( DEBIAN )

Els nuclis o distros de Linux es divideixen en dos grans grups. Els que usen paquets RPM i els que usen paquets DEB.

## 1.  De forma gràfica:

*   gdebi-gtk

*   Synaptic

*   Dselect
    
## 2.  Des de consola. Descarregant el paquet**
```
sudo dpkg –i paquet.deb
```
## 3.  Desde consola. Amb repositoris
```bash
sudo apt install paquete
```
```
aptitude install paquet
aptitude remove paquet
```
```bash
sudo snap install paquet
sudo snap rermove paquet
```

## Paquets *.deb* 
Paquets d'instal·lació de Debian/GNU i derivades ( Ubuntu ).
Contenen tres fixers:

*   debian-binary -  número de versió del formatodeb. 
*   control.tar.gz - la metadada.
*   data.tar, data.tar.gz, data.tar.bz2 o data.tar.lzma: - els arxius a instal·lar.


Per instal·lar
```bash
    dpkg -i paquete.deb
```
Per verificar la instal·lació :
```bash
    dpkg -l | grep 'paquet'
```
Per desinstal·lar:
```bash
    dpkg -r paquet.deb
    dpkg -P paquet.deb 
```
Esta última elimina totes les dades 

## Repositori
Servidor accessible des d'internet que emmmagatzema els paquets ( programes ) a instal·lar. 

## Tarballs

Sovint encara trobarem software empaquetat amb el clàssic **tar** i comprimit.  Els paquets solen ser del tipus **\*.jar, \*.bin, \*.rpm**.
Només hem de desempaquetar i normalment compilar i instal·lar.

Pot ser un paquet *nom_paquet.tar.gz* o *nom_paquet.tgz*. 

**\*.gz. \*.tgz**

```bash
cd directori_on_esta_tarball
tar –zxvf nom_paquet.tar.gz 
cd nom_paquete_desempaquetat
sudo ./configure
make
make install
```
**\*.bz2, \*tbz2**

Per a paquets *mom_paquet.bz2* o *nom_paquet.tbz2*
```bash
cd directori_on_esta_tarball
tar –jxvf nom_paquet.tar.bz2
cd nom_directori_desempaquetat
sudo ./configure
make
make install
```

## Scripts.
Els scripts poden estar fets amb els comandaments propis del Shell ( com farem a classe ) o amb el llenguatge multiparadigma interpretat Python.

El que caldrà fer és donar permisos d'execució ( en Linux els "executables" han de dur este "atribut" ) i llançar-los:

1.  Scripts de **shell**. ( **\*.sh**)
```bash
cd directori
sudo chmod u+x script1.sh
./script1.sh
```
o 
```bash
cd directori
sudo chmod u+x script1.sh
bash script1.sh
```

2.  Scripts de **Python** ( **\*.py**)
```bash
python nombre_script.py install
```

## Binaris
1.  **.jar**

Cal tindre instal·lada la Màquina Virtual Java d'Oracle (JRE o JDK). :
    *   Click amb el botó contrari.
    *   Seleccionem "Obrir amb altra aplicació"
    *   Escrivim *java –jar* 
    *   Obrir
    
2.  **.bin**
*Des de l'entorn gràfic* 
>
    * Donem permisos d'execució. ( des de *propietats* )
    * Fem doble click.
	
*Des de la consola*
```
cd directori_binari
./nom_binari.bin
.run:
```

3.  **run** 

Igual que els *bin*. Sol ser molt usat per drivers.
