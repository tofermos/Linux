
# Tasques programades amb gnu/linux
Com ja hem explicat a classe, l'administració d'un sistema requeix planificar determinades tasques per a que s'executen de forma programada fora de l'horari habitual de treball dels usuaris finals.

## Cron
Cron és un servici ( programa tipus __daemons_  ) dels sistemes gnu/linux que permet executar ordres de forma automàtica a una data i hora concretat. És, per tant, usat pels administradors de sistemes com a eina per automatitzar tasques d’administració.

**Per iniciar el servici**

```
sudo /etc/init.d/cron start
```

## Crontab
Amb el comandament crontab podem gestionar les tasques programades al
nostre sistema. 
Per poder veure quines són les tasques actualment programades:
```
$ crontab -l
```
Per editar les tasques programades utilitzarem: 
```
$ crontab -e
```
Ens permetrà seleccionar l’editor amb el qual volem editar-les. Recomenem
editar amb l’editor nano que hem fet servir fins ara.
```
Select an editor. To change later, run 'select-editor'.
1. /bin/nano    <---- easiest
2. /usr/bin/vim.tiny
3. /usr/bin/code
Choose 1-3 [1]:

```
Programació de tasques amb cron.
La sintaxi de les tasques és la següent:
\* \* \* \* \* [path al binari o script a executar]

Els 5 asteriscs. D’esquerra a dreta, els asteriscs representen:
1.  Minuts: de 0 a 59.
2.  Hores: de 0 a 23.
3.  Dia del mes: d’1 a 31.
4.  Mes: d’1 a 12.
5.  Dia de la setmana: de 0 a 6, sent 0 el diumenge.

Si es deixa un asterisc, vol dir “cada” minut, hora, dia de mes, mes o dia de la setmana.

Per exemple:
45 1 \* \* 2 /root/backup.sh
Executar este script:
1.  Al minut 45
2.  De les 1 de la nit
3.  De cada dia del mes
4.  De cada mes
5.  Només si és dimarts

O siga, cada dimarts 1:45 hores s’executarà l’script



