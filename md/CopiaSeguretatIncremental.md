# CÒPIA DE SEGURETAT INCREMENTAL. LINUX.

* Eina **tar**.
* Ordre **date**

En esta pràctica farem una senzilla còpia tota i incremental sobre una carpeta. 

**Descripció**

A partir d'una carpeta *origen* on crearem 3 fitxers:

*fitxer1, fitxer2, fitxer3*

Implementarem i provarem un senzillet sistema de còpies de seguretat incrementals seguint els següents passos:


1.	Crear una còpia de seguretat total d'una carpeta.
2.	Modificar *fitxer1*
3.	Avançar el dia. 
4.	Fer còpia incremental.
5.	Modificar el *fitxer2* i crear un nou *fitxer4*
6.	Avançar el dia i fer còpia incremental.
7.	Eliminar tota la carpeta *origen*
8.	Retaurar les còpies.

**CONSIDERACIONS**


Simularem que la còpia és diària avançant la data del sistema amb l'ordre *date -s*.
El fitxer de còpia tindrà un nom que indique la data i hora en què s'ha fet. 


COPIA TOTAL

```bash
tomas@portatil:~$ v="bck_"$(date +"%Y%m%d%H%M")

tomas@portatil:~$ tar -cvf backups/${v}.tar -g backups/${v}.log origen/
tar: origen: Directory is new
origen/
origen/fitxer1
origen/fitxer2
origen/fitxer3
tomas@portatil:~$ ls -l backups/
total 16
-rw-rw-r-- 1 tomas tomas   109 d’abr.   19 00:05 BCK_202104190001.log
-rw-rw-r-- 1 tomas tomas 10240 d’abr.   19 00:05 BCK_202104190001.tar
tomas@portatil:~$ 


```
MODIFIQUEM el fitxer1

```bash
tomas@portatil:~$ sudo date +"%Y%m%d%H%M" -s "202104210920"
tomas@portatil:~$ date >> origen/fitxer1
tomas@portatil:~$ v="bck_"$(date +"%Y%m%d%H%M")
tomas@portatil:~$ echo $v
bck_202104210009
tomas@portatil:~$ tar -cvf backups/${v}.tar -g backups/estat.log origen/
origen/
origen/fitxer1

```


MODIFIQUEM EL SEGON I CREEM
```bash
tomas@portatil:~$ sudo date +"%Y%m%d%H%M" -s "202104220923"

tomas@portatil:~$ date >> origen/fitxer2
tomas@portatil:~$ touch origen/fitxer4
tomas@portatil:~$ tar -cvf backups/${v}.tar -g backups/estat.log origen/
origen/
origen/fitxer2
origen/fitxer4
```


ELIMINEM 
```bash
tomas@portatil:~$ rm origen/*
tomas@portatil:~$ ls -l origen/
total 0
tomas@portatil:~$ 
```

