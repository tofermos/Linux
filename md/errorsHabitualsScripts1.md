# REPÀS D'ALGUNS CONCEPTES I ERRORS DELS EXERCICIS D'SCRIPTS

## Aclariment bàsic
Els comandadaments per tractar fitxer de textos són fonamentals
* cat
* cut
* grep
* wc

A estes altures no podeu confondre la redirecció (ordre **>** fitxer ), el pipeline (ordre **|** ordre )  i el **echo** (sobre variables)

### Exemple senzillet
```bash
tomas@portatil:~$ grep /bash$ /etc/passwd|cut -d: -f1 > usbash.txt
tomas@portatil:~$ for i in `cat usbash.txt`;do echo usuari:$i;done
usuari root
usuari tomas
usuari rosa
```
## Sobre els bucles 
### El de tipus "for var in ..." són útils per recórrer:

* variables amb llistes d'elements ( a=`ls`)
* vectors
* línies de fitxer de text ( exemple anterior)
* els paràmetres d'un script
  
### Els infinits tipus "while true do "
* Molt útil per a menús en script interactius que ens donen l'opció de eixir polsant una tecla ( exit o break ).
 * Compte amb els recursos del sistema si estem usant-los.
*exit* si volem eixir del script ( podem afegir un codi d'error: exit -1)

*break* per eixir del bucle


### Abans d'un while ab guarda ( condició )

* Convé controlar el/s valors inicials de les variables de la guarda
* La condició s'ha de revisar dins del bucle per poder eixir
  
### El for tipus "*for (( compte=1; compte<=10; c++ ))*"
* Les variable no duen "$".
* S'usa quan sabem el nombre exacte de iteracions del bucle.

## Sobre els paràmetres d'un script ( o funció )
Hi ha dos maneres de passar valors a un script:

Interactivament amb el comandament **read**

En la mateixa crida del script/funció *nomScript.sh param1 param2*

## Paràmetres de script/funció.

|||
|---|---|
|* $0| Nom de l'script|
|* $#| Nombre total de paràmetres|
|$* o $@| Tots|
|$1..$9|paràmetres
|\$\{10},${11}... | paràmetres|


Molt útil: **shift** Teniu a solució a un exercici que es simplifica molt amb un *shift 1*
Exemple:
```bash
#!/bin/bash
echo "Has passat $# paràmetres"
echo "Son els següents: $*"
read -p "Quants vols que en lleve? " n
shift $n
echo "Ara et queden: $*"
```

Els script sovint són per a tasques de l'administrador del sistema que queden programades. No seran interactius, per això el paràmetres son fonamentals.

L'ús del *shift* ens permet separar els paràmetres de forma que si alguns els hem de recórrer dins dels script, els posem darrere. 
