- Est l'acronyme de __Section Headers FLAGS__
- Sur 8 bytes en 64 bits ou 4 bytes en 32 bits
- Permet d'avoir des information supplementaire a propos de la section
- SHF_WRITE = Indique que l'on peux ecrire dans la section durant le runtime
- **SHF_ALLOC** : Indique que le contenu de la section va etre charger en memoire pendant le runtime
- **SHF_EXECINSTR** : Indique que le contenu de la section contient du code et doit etre charger pendant le runtime

| CODE       | NAME                 | DESCRIPTION                                                      |
| ---------- | -------------------- | ---------------------------------------------------------------- |
| 0x1        | SHF_WRITE            | Writable                                                         |
| 0x2        | SHF_ALLOC            | Occupies memory during execution                                 |
| 0x4        | SHF_EXECINSTR        | Executable                                                       |
| 0x10       | SHF_MERGE            | Might be merged                                                  |
| 0x20       | SHF_STRINGS          | Contains null-terminated strings                                 |
| 0x40       | SHF_INFO_LINK        | ‘sh_info’ contains SHT index                                     |
| 0x80       | SHF_LINK_ORDER       | Preserve order after combining                                   |
| 0x100      | SHF_OS_NONCONFORMING | The section is member of a group                                 |
| 0x200      | SHF_GROUP            | The section is excluded unless referenced or allocated (Solaris) |
| 0x400      | SHF_TLS              | Section hold thread-local data                                   |
| 0x0FF00000 | SHF_MASKOS           | OS-specific                                                      |
| 0xF0000000 | SHF_MASKPROC         | Processor-specific                                               |
| 0x4000000  | SHF_ORDERED          | Special ordering requirement (Solaris)                           |
| 0x8000000  | SHF_EXCLUDE          | Section is excluded unless referenced or allocated (Solaris)     |