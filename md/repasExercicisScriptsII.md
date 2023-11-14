# REPÀS D'ALGUNS CONCEPTES I ERRORS DELS EXERCICIS D'SCRIPTS (II)

## Exemple d'us de vectors i shift


A partir d'una solució feta amb vectors d'un alumne revisada ( Solució 1 ), fixeu-vos en l'alternativa fent ús de l'ordre *shift*. 

**Enunciat:**
L'script reb un primer paràmetre que és una cadena de text a buscar en uns fitxers que seran els paràmetres següents.
|Pàrametre|Significat|
|---|---|
|$1|cadena_a_buscar|
|$2|primer fitxer on buscar|
|$3|segon fitxer on buscar|
...|...|


Per buscar el text, usarem el *grep*. És més pràctic que obriu un terminal i feu proves per evitar editar i guardar repetidament l'script fent proves.

La solució es basarà en un algorisme que recorra des del 2on paràmetre fins el darrer (bucle) buscant el valor del primer.

El "problema" a resoldre és que cal iniciar el bucle en una posició o element que no és el primer. 


**Solució amb vectors**


Veiem la primera solució basada en arrays ( vectors).

Copia els paràmetres en un array. Si el primer paràmetre era el text a buscar, este es guardarà en el primer element del vector ( posició 0, ${fbuscado[0]}). 

Després recorre el vector ( bucle ) a partir del segon element ( posició 1, ${fbuscado[1]})

```bash
#!/bin/bash
afco=1
fbuscados=($*)
echo ${fbuscados[@]}
while [ $afco -lt $# ];do
resultado=$(grep -c $1 ${fbuscados[$afco]})
if [ $resultado -gt 0 ];then
llista="$llista ${fbuscados[$afco]}"
fi
let afco=afco+1
done
if [[ $llista = "" ]]; then
echo "No s'ha trobat el text $1"
else
echo "El text $1 s'ha trobat en els fitxers $llista"
fi
```

**Solució basada en shift**
Com la cadena a buscar és el primer argument, la guardem en una variable i la "llevem" de la llista de paràmetres amb *shift 1* El "1" indica el nombre de paràmetres que ometrem. 

A partir d'un shift 1, La variable $1 fa referència al segon paràmetre, el que era abans $2 i així successivament. $* són tots,excepte el primer.

Podem fer shift 2 o 3...


```bash
#!/bin/bash
text=$1   #guardem el valor a buscar perquè...
shift 1   
#en desplaçar-nos no podrem accedir a ell. $1 passa a valdre el segon argument
echo "Anem a buscar $text en els fitxers $*"
for fitxer in $*;do
	if [ $(grep -c $text $fitxer) -gt 0 ];then
	llista="$llista $fitxer"
fi
done
if [[ $llista = ""  ]];then
echo "No s'ha trobat el text: $text en cap fitxer"
else 
echo "S'ha trobat $text en $llista"
fi
```

En estos casos, convé guardar en variables els paràmetres que anem a llevar abans de fer el shift, per poder usar-los.

**Conclusió**

> Sovint tindrem una llista amb un nombre indefinit de paràmetres a recórrer i altres, de distinta naturalesa en un nombre fixe.

>La solució passa per passar els criteris com a paràmetres primers ($1, $2...) i la llista a recórrer en els paràmetres que venen a continuació.Així, guardem els criteris en variables i usem el shift just abans de recórrer la llista.

*Exemples:*

* Buscar d'una llista de PID (processos) els que ha executat un usuari concret.

* Sumar el tamany d'una llista de fitxers els que superen un mínim i no arriben a un màxim. En este cas cladria un *shfit 2*
