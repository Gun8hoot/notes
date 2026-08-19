- L'identifier est composer de 16 bytes ![img](https://aleeamini.com/storage/2023/08/elf-header-identifier.png)
- 1 - 4 : magic number d'un fichier ELF qui permet d'identifier facilement le type du fichier (ici il ecrit "ELF" pour savoir que c'est un ELF)
- 5 : **EI_CLASS byte** permet de dire le binaire a été compiler pour du 32 ou 64 bit (1 = 32 bits ; 2 = 64 bits)
- 6 : **EI_DATA bytes** permet de donnée l'ordre des bits (1 = LSB ; 2 = MSB)
- 7 : **EI_VERSION** est la version de ELF. Cepandant jusqu'a maintenant, il existe que une seul version disponible. Ce bytes est donc toujours mis a 1.
- 8 : **EI_OSABI** specifie la platforme (Application Binary Interface) de notre fichier.

| Value    | ABI                                          |
| -------- | -------------------------------------------- |
| **0x00** | **System V**                                 |
| 0x01     | HP-[UX](https://en.wikipedia.org/wiki/HP-UX) |
| 0x02     | NetBSD                                       |
| 0x03     | Linux                                        |
| 0x04     | GNU Hurd                                     |
| 0x06     | Solaris                                      |
| 0x07     | AIX (Monterey)                               |
| 0x08     | IRIX                                         |
| 0x09     | FreeBSD                                      |
| 0x0A     | [Tru64](https://en.wikipedia.org/wiki/Tru64) |
| 0x0B     | Novell Modesto                               |
| 0x0C     | OpenBSD                                      |
| 0x0D     | OpenVMS                                      |
| 0x0E     | NonStop Kernel                               |
| 0x0F     | AROS                                         |
| 0x10     | FenixOS                                      |
| 0x11     | Nuxi CloudABI                                |
| 0x12     | Stratus Technologies OpenVOS                 |

- 9 : **EI_ABIVERSION** specifie la versioné de EI_OSABI mais ce bytes n'est pas utile est toujours laisser a 0
- 10 - 16 : Bytes reserver au cas ou un jour une nouvelle option doit etre ajouter et sert egalement de padding pour arriver a 16 bytes
![img](https://aleeamini.com/storage/2023/08/elf-header-details-2.png)