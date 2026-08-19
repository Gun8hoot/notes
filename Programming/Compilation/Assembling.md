- Dans cette phase le code en assembleur est assembler pour pouvoir etre transformer en code machine (binaire)
- A la fin de cette etape, l'assembleur nous donne un fichier dit **objet**. Ce type de fichier (qui est binaire) contient tous le code machine de code qui est passer dans le [[Pre-processing]], [[Compiling]] et [[Assembling]]
- Pour les compilateur de C/C++ comme gcc/g++ ou clang/clang++, nous devons mettre le flag `-c` pour dire au compilateur de s'arreter apres avoir fini cette etape.
- En utilisant la commande [`file`](https://www.man7.org/linux/man-pages/man1/file.1.html) sur un fichier objet nous remarquons que notre fichier possede des charactere bien precis :
```sh
$ file main.o
main.o: ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped
```

1. ELF 64-bit : Nous indique que notre fichier est un fichier **ELF compiler en 64 bits**
2. LSB : Nous permet de savoir [l'eldianess de notre fichier](https://dev.to/amanprasad/understanding-endianness-little-endian-vs-big-endian-31e). Ici notre fichier est en LSB ([**Least Significant Byte**](https://www.computerhope.com/jargon/l/leastsb.htm)) mais il est possible d'avoir une autre variate qui s'appelle MSB (Most Significant Byte).
3. Relocatable : Keyword qui nous dit que le fichier est un fichier objet et non executable car les addresse des fonction et des variables globales ne sont pas definites. Pour ce faire le compilateur met des "dummy address" afin de mettre la bonne addresse durant la prochaine phase 
4. Not stripped : Keyword qui nous precise que notre fichier n'est pas "stripped" ce qui veux dire que le fichier a une string table et des info de debug (on va en parler plus en detail plus tard askip)