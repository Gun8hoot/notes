- Pendant cette phase, le linker va lier tous les fichier objet qui lui est donné afin d'avoir un executable
- Sur Linux, le linker par defaut est nommer **[ld]()** ("loader", l'ancien nom des linker)
- Le linker, lui, a toutes les objet disponible ce qui lui permet de remplacer toutes les "dummy value" mis durant l'[[Assembling]] par leurs vrai addresses
```sh
$ file main.out
main.out: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=93ac661cd1d1ca30d34a834b2295dbc54acc29af, for GNU/Linux 3.2.0, not stripped
```

- Dynamicly linked : Le fichier utilise des symboles provenant d'objet partager (qui sont generalement en .so) qui seront resolu au runtime
- Interpreter : Donne le nom et le path du "Dynamic linker" qui va permettre de linker les symbole venant des librarie partager (qui est en so)
- Relocation : (Voir le point  sur la relocation qui est plus bas)

- Si on devrai resumer la compilation en image sa devrait ressembler a ça : 
![img](https://aleeamini.com/storage/2023/08/linking-3-1536x857.png)

### Relocation :
- Les fichier ELF ont plusieurs tables differentes qui ont tous leurs propres données avec leurs propres utiliter, l'une d'elle est la table appeller **Relocation** ou `.reloc`
- Elle va contenir des information de relocalisation qui va aider le linker a trouver les differentes addresses.
```
$ objdump -s -M intel main.o
[...]
  69:	89 c7                	mov    edi,eax
>  6b:	e8 00 00 00 00       	call   70 <main+0x5c>
  70:	89 45 fc             	mov    DWORD PTR [rbp-0x4],eax
[...]
```
- Le code ci dessus provien d'un deassembleur appeller `objdump`. Il nous permet de convertir notre code machine en asm.
	- Sur la ligne mis en evidence, nous pouvons voir que notre programme utilise [l'instruction call](https://asm-docs.microagi.org/x86/call.html) qui permet d'appeller un fonction a l'addresse renseigner.
	- Si nous regardons les bytes en hex, nous pouvons y lire `e8` qui est [l'opcode de notre instruction](https://en.wikipedia.org/wiki/List_of_x86_instructions) et 4 bytes a `00`. Normalement il y a l'addresse de notre fonction a la place des 0 mais vue  que l'assembleur ne connaissait pas l'addresse de la fonction il a mis des 0x00.
	- Imaginons que notre addresse est `0xff775522`, nous etions sensé avoir `E8 22 55 77 ff` (les bytes sont inverser parce que nous somme en LSB)
```
$ readelf --relocs main.o
Relocation section '.rela.text' at offset 0x350 contains 11 entries:
  Offset          Info           Type           Sym. Value    Sym. Name + Addend
00000000003b  000500000002 R_X86_64_PC32     0000000000000000 .rodata - 4
000000000045  000c00000004 R_X86_64_PLT32    0000000000000000 __isoc99_scanf - 4
000000000053  000500000002 R_X86_64_PC32     0000000000000000 .rodata - 4
00000000005d  000c00000004 R_X86_64_PLT32    0000000000000000 __isoc99_scanf - 4
>> 00000000006c  000900000004 R_X86_64_PLT32    0000000000000000 func_add - 4

000000000076  000500000002 R_X86_64_PC32     0000000000000000 .rodata + 24
000000000082  000500000002 R_X86_64_PC32     0000000000000000 .rodata - 1
00000000008c  000d00000004 R_X86_64_PLT32    0000000000000000 printf - 4
00000000009d  000500000002 R_X86_64_PC32     0000000000000000 .rodata + 24
0000000000b2  000500000002 R_X86_64_PC32     0000000000000000 .rodata + d
0000000000bc  000d00000004 R_X86_64_PLT32    0000000000000000 printf - 4
```
- `readelf` est un programme qui permet de parser et dumper les information d'un fichier ELF
- Les deux colone les plus importantes sont la colone **offset** et **Sym. Name + Addend**
	- offset : Est l'emplacement en bytes de notre relocation dans la section.
	- Sym. Name + Addend : Ce qu'il y a avant l'operateur est un symbole etant referencer par un nom. Addend est un nombre constant qui sera mis durant le relocation
- Avec ces information, le linker a juste besoin d'aller lire la table de relocation et de chercher **reference du symbole** et avec le nom de la fonction ou l'on cherche son addresse.  
- Pour revenir a comment le linker arrive a bien placer les addresse aux bonne endroit. Objdump nous dit que notre `call` est a un offset 6b dans notre section `.text` et notre relocation table nous dit que la fonction func_add a un offset de `6c` qui est egal a l'offset de notre instruction + 1. 