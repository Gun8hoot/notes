- 2 bytes qui permettent de specifier le type de fichier ELF (si c'est un executable, librarie partager, core dump, etc...)

| Value  | Type      | Meaning                                              |
| ------ | --------- | ---------------------------------------------------- |
| 0x00   | ET_NONE   | Unknown.                                             |
| 0x01   | ET_REL    | Relocatable file.                                    |
| 0x02   | ET_EXEC   | Executable file.                                     |
| 0x03   | ET_DYN    | Shared object.                                       |
| 0x04   | ET_CORE   | Core file. (Dump files)                              |
| 0xFE00 | ET_LOOS   | Reserved inclusive range. Operating system specific. |
| 0xFEFF | ET_HIOS   |                                                      |
| 0xFF00 | ET_LOPROC | Reserved inclusive range. Processor specific.        |
| 0xFFFF | ET_HIPROC |                                                      |
![img](https://aleeamini.com/storage/2023/08/elf-header-part2-1.png)