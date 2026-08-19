- Est aussi appeller __Section Headers OFFSET__
- Est egalement un offset sur **8 bytes/4 bytes** mais qui lui indique ou se trouve la `Section Headers table`
- Chaque element qui compose un fichier ELF peux se trouver n'importe ou dans le fichier, sauf deux: Le Program header qui se trouve toujours apres le header ELF et le Section Header qui se trouve toujours a la fin du fichier
- Le header ELF est comme une carte vers tous les autres element qui compose le fichier ELF
![img](https://aleeamini.com/storage/2023/08/elf-header-ph-sh.png)