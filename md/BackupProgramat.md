# PROGRAMACIÓ DE TASQUES
## BACKUP

Crea una estructura de dades similar a la figura:
```
$ tree diracop/
diracop/
├── subdir1
│   ├── dos.txt
│   └── tres.txt
├── subdir2
└── uno.txt

2 directories, 3 files
```

Amb l'orde tar fes una còpia del directori en un fixer comprimit.
```
$ tar -czf copia.tar.gz diracop
```

Esborra "accidentalment" el directori:
```

$ rm -r diracop
```

Recupera de la còpia feta les dades:
```
$tar -xf copia.tar.gz
```


## PROGRAMAR AMB EL cron

L'ordre de backup de l'apartat anterior, incorpora-la a un script i programa el sistema per a que s'execute a les 24:00 del dijous de cada setmana.

```
crontab -e
```

