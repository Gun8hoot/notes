#todo
- L'eldianness est pour l'ordinateur, la facon dont les bits sont stocker.
- Il en existe deux type  : 
	- [[LSB]] (Least Significant Bit)
	- [[MSB]] (Most Significant Bit)


- L'edianness en architecture systeme est comment les bits sont stoquer en memoire
- Il existe deux type de d'eldianness
	- MSB (Most Significant Byte || Big Eldian) : Va stocker les bytes du plus grand aux plus petit (similaire a comment on lit nous les nombre ou le plus petit est sur la droite et le plus grand sur la gauche)
	- LSB (Least Significant Byte || Big Eldian) : Va stocker les bytes du plus petit aux plus grand
	- Donc si ont prend l'example du int **44497** (0xADD1 en hexa) qui est sur deux bytes:
		- En MSB ca c'est stocker en 0xADD1 (on calcul l'hexa en MSB)
		- En LSB ca c'est stocker en 0xD1AD
	- Pour un int comme **2147483647** (0x7FFFFFFF en hexa) qui est sur 4 bytes			
		- En MSB ca c'est stocker en 0x7FFFFFFF (on calcul l'hexa en MSB)
		- En LSB ca c'est stocker en 0xFFFFFF7F
- Vue qu'il existe deux type d'eldianness different et que l'on ne peux pas prevoir comment comment l'autre machine traite les données. Il a ete decider que l'ordre des octets reseaux est toujours en MSB. Et apres l'host va convertir les donnee en LSB si besoin