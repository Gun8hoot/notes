#compression
- LZ4 est un **algorithme de compression lossless** (ne fait pas de perte de donnée) qui a un bon equilibre entre le taux de compression et la vitesse tant de compression/decompression
- C'est un algorithme qui utilise de la compression par dictionnaire
- Il est supporter nativement par [Linux](), [ZFS](https://en.wikipedia.org/wiki/ZFS), [Redis]() ou bien encore [PostgressSQL]()
### Comment LZ4 marche : 
![img|700](lz4_file_structure.png)
1.  En premier nous devons identifier la sequence qui a été compresser en LZ4 grace a son **magic number** (*`0x184D2204`*)
	- Le magic number est sur 4 bytes en little eldian
2. Secondement, nous allons regarder le **frame descriptor**.
	- Il va nous donner toutes les métadonnées sur les blocks de données
	- Il est composé de cinq section :
		- FLG Bytes : Bytes qui va contenir des options
			- Version (bits 7-6) : Doit toujours etre mis a 1
			- Block independence (bit 5) : Si ce bit est mis a 1, lles block sont independent les un les autres.
			- Block checksum (bit 4) : Est mis a 1 si chaque block a une [checksum](https://en.wikipedia.org/wiki/Checksum) de 4 bytes
			- Content size (bit 3) : Si est a 1 c'est que la prochaine section va contenir la taille decompresser 
			- Content checksum (2 bit) : Si mis a 1, une checksum est presente a la fin du contenu
			- Reserver (1 bit) : N'est pas utiliser, doit etre toujours mis a 0
			- Dictionary ID (0 bit) : Si ce bit est a 1 c'est que l'id d'un dictionaire est present dans le header

		- BD (Block Description) Byte : Bytes qui va contenir des metadonnée sur comment sont regler les blocks
			- Reserver (7 bit) : Doit etre mis a 0, ne sert a rien
			- Block MaxSize (6-5-4) : Nous precise la taille maximal d'un block
				- 4 = 64kb (64000 bytes)
				- 5 = 256kb (256000 bytes)
				- 6 = 1mb (1000000 bytes)
				- 7 = 4mb (4000000 bytes)
				- La taille maximum d'un block est de 16mb (16000000 bytes)

		- Content Size (OPTIONEL) : 
			- Taille du contenu non compresser
			- Est present uniquement si le 3e bit de FLG bytes est mis a 1
			- Le contenu est stocker sur 8 bytes
			- L'eldianness est en little eldian
			- Cette valeur existe pour etre display ou pour les allocation dymatiques

		- Dictionary ID (OPTIONEL) :
			- Un dictionnaire est utile lorsque l'on veux compresser des petite sequence pour pouvoir les + compacter les données
			- Le compresseur et le decompresseur doivent utiliser le meme dictionnaire
			- Section presente uniquement si le bit 0 de FLG bytes est mis a 1
			- Il est stocker sur un unsigned 32bit (uint32_t)
		
		- Header checksum :
			- Est un checksum de tous les champs mis en haut
			- Il est realiser sur 1 bytes avec l'[[xxHash]] algorithm. l'algo est sur 32bit mais on prend le second byte du resultat pour servir de checksum *`(xxh32()>>8) & 0xFF`*
			- La seed de xxhash32 doit etre mis a 0
#### Frame descriptor
- Il contient des metadonnées sur comment les données ont été compresser et le format de notre frame
- Il est composé de 5 section : 
	- FLG Bytes : Bytes qui va contenir des options
		- Version (bits 7-6) : Doit toujours etre mis a 1
		- Block independence (bit 5) : Si ce bit est mis a 1, lles block sont independent les un les autres.
		- Block checksum (bit 4) : Est mis a 1 si chaque block a une [checksum](https://en.wikipedia.org/wiki/Checksum) de 4 bytes
		- Content size (bit 3) : Si est a 1 c'est que la prochaine section va contenir la taille decompresser 
		- Content checksum (2 bit) : Si mis a 1, une checksum est presente a la fin du contenu
		- Reserver (1 bit) : N'est pas utiliser, doit etre toujours mis a 0
		- Dictionary ID (0 bit) : Si ce bit est a 1 c'est que l'id d'un dictionaire est present dans le header
	- BD (Block Description) Byte : Bytes qui va contenir des metadonnée sur comment sont regler les blocks
		- Reserver (7 bit) : Doit etre mis a 0, ne sert a rien
		- Block MaxSize (6-5-4) : Nous precise la taille maximal d'un block
			- 4 = 64kb (64000 bytes)
			- 5 = 256kb (256000 bytes)
			- 6 = 1mb (1000000 bytes)
			- 7 = 4mb (4000000 bytes)
			- Vue que Block MaxSize est sur 3 bits, la taille maximal d'un block est de 16mb (16000000)
#### Data Blocks
- Les blocks sont separer en 3 section : 
	- Block size : 4 bytes avec les valeur en little eldian qui indique la taille de notre block actuel :
		- Si le highest bit (`0x80000000`) est mis c'est que le block contien des donnée non compressée
		- Si la block size est de `0x00000000`, c'est que nous somme arriver au EndMark et qu'il n'existe plus aucun block
		- Tous autre valeur est la taille totale du block
		- La taille d'un block ne doit JAMAIS etre plus grand que __Block MaxSize__
	- Data : Est les données compresser (ou non)
[[Format des Blocks LZ4]]
## SOURCES
- [Wikipedia ZFS](https://en.wikipedia.org/wiki/ZFS)
- [Deepwiki LZ4](https://deepwiki.com/lz4/lz4/6.2-lz4-frame-format)
- [Deepwiki Block Format LZ4](https://deepwiki.com/lz4/lz4/6.1-lz4-block-format)