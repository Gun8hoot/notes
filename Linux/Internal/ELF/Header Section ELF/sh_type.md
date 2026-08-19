- Est l'acronyme de __Section Headers TYPE__
- Valeur sur 4 bytes qui nous donne le type de la section
- Cette valeur est egalement utile pendant le linking lorsque des section servent pour la relocation

|Value|Name|Meaning|
|---|---|---|
|0x0|SHT_NULL|Section header table entry unused|
|0x1|SHT_PROGBITS|Program data|
|0x2|SHT_SYMTAB|Symbol table|
|0x3|SHT_STRTAB|String table|
|0x4|SHT_RELA|Relocation entries with addends|
|0x5|SHT_HASH|Symbol hash table|
|0x6|SHT_DYNAMIC|Dynamic linking information|
|0x7|SHT_NOTE|Notes (Some additional information about the binary)|
|0x8|SHT_NOBITS|Program space with no data (bss)|
|0x9|SHT_REL|Relocation entries, no addends|
|0x0A|SHT_SHLIB|Reserved|
|0x0B|SHT_DYNSYM|Dynamic linker symbol table|
|0x0E|SHT_INIT_ARRAY|Array of constructors|
|0x0F|SHT_FINI_ARRAY|Array of destructors|
|0x10|SHT_PREINIT_ARRAY|Array of pre-constructors|
|0x11|SHT_GROUP|Section group|
|0x12|SHT_SYMTAB_SHNDX|Extended section indices|
|0x13|SHT_NUM|Number of defined types.|
|0x60000000|SHT_LOOS|Start OS-specific.|
#### Les plus important
- SHT_NULL : Indique que la section est NULL et ne contien donc pas de donnée
- SHT_PROGBITS : Indique que la section contient des instruction machine ou des constantes
- SHT_SYMTAB : Indique que cette section est un **static symbole** qui va stocker le symbole de l'executable lui meme
	- Un symbole est un nom et type symbolique pour une addresse particuliere ou un offset. Par example le nom des variable et des fonction sont sauvegarder en symbole dans le fichier ELF
	- Le linker utilise cette section pour localiser l'addresse des variables et fonctions
- SHT_DYNSYM : Indique que cette section est une table de symbole dynamique qui stock les symboles qui sont utile aux runtime de l'executable
	- Le linker utilise egalement ce type de section pour localiser toutes les fonctions externes 
- SHT_STRTAB : Indique que cette section est une string table
- SHT_RELA / SHT_REL : Indique des informations a propos de la relocation qui est effectuer par le linker pendant la linking phase.
- SHT_DYNAMIC : Contient toutes les informations necessaire durant le dynamic linking
- SHT_INIT_ARRAY : Indique que cette section contient un tableau d'addresse de constructeur de fonction
	- Un constructeur de fonction est une fonction qui est appeller avant que la fonction principale soit appeller.
- SHT_FINI_ARRAY : Indique que cette section contient un tableau d'addresse des destructeur de fonction
	- Un destructeur de fonction est une fonction qui est appeller apres que la fonction principale soit appeller.
