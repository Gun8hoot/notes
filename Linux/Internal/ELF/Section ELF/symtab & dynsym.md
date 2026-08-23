- Ces sections sont des tables qui contienne des information sur des symbols[^1]
- La section `.symtab` (SHT_SYMTAB):
	- Contient des symboles decrivant le fichier.
	- Elle est utilisé par le linker ou par des outils de debug afin d'avoir plus d'information sur le binaire
	- Cette section n'est pas charger dans la RAM durant le runtime (on le vois car elle n'a pas d'addresse ou de flag)
	- La section est de type *`SYMTAB`*
- La table `.dynsym` (SHT_DYNSYM):
	- Contient des references vers des symboles[^1]
	- Est utiliser par dynamic linker ou pendant le runtime 
	- Cette section est charger dans la RAM durant le runtime
	- La section est de type *`DYNSYM`*
- Les deux section on une structure commune*`ELF64_Sym`* presente dans *`/usr/include/elf.h`*
```c
typedef struct
{
  Elf64_Word    st_name;  (4-Byte)               /* Symbol name (string tbl index) */
  unsigned char st_info;  (1-Byte)               /* Symbol type and binding */
  unsigned char st_other; (1-Byte)               /* Symbol visibility */
  Elf64_Section st_shndx; (2-Byte)               /* Section index */
  Elf64_Addr    st_value; (8-Byte)              /* Symbol value */
  Elf64_Xword   st_size;  (8-Byte)               /* Symbol size */
} Elf64_Sym;
// --- 24 BYTES SYMTAB/DYNSYM STRUCTURE ---
```
1. `st_name` : Contient le nom de la section grace a un pointer vers `.strtab`.
2. `st_info` : Flag qui nous donne le type de symbol[^1] et son binding. Les quatres bits sur la gauche definissent le type de symbole et les quatres autres bits definissent son binding

| Type Name                                                            | Value |
| -------------------------------------------------------------------- | ----- |
| `STT_NOTYPE`                                                         | `0`   |
| `STT_OBJECT` (variable, an array, and so on.)                        | `1`   |
| `STT_FUNC` (function in the object file)                             | `2`   |
| `STT_SECTION` (ponts to a section of ELF file)                       | `3`   |
| `STT_FILE` (name of the source file associated with the object file) | `4`   |
| `STT_COMMON`                                                         | `5`   |
| `STT_TLS`                                                            | `6`   |
| `STT_LOOS` (reserved for operating system)                           | `10`  |
| `STT_HIOS` (reserved for operating system)                           | `12`  |
| `STT_LOPROC` (reserved for processor-specific)                       | `13`  |
| `STT_HIPROC` (reserved for processor-specific)                       | `15`  |
<div align="center">
	<p>Symbol type Table</p>
</div>

| Type Name    | Value |
| ------------ | ----- |
| `STB_LOCAL`  | `0`   |
| `STB_GLOBAL` | `1`   |
| `STB_WEAK`   | `2`   |
| `STB_LOOS`   | `10`  |
| `STB_HIOS`   | `12`  |
| `STB_LOPROC` | `13`  |
| `STB_HIPROC` | `15`  |

[^1]: Symbols: Representation d'une variable ou une fonction dans un fichier ELF.
