- Le formatage de block defini la representation des données compressées sans metadata
- Le formatage des blocks LZ4 a un encodage orienté octet concu pour la simplicité et la rapidité. Il est composé de serie de sequences ou chaque sequence represente une suite de literal non compresser suivi d'une reference vers l'ancienne donnée decoder qui correspond a ce que l'on veux decoder.
### Structure d'une sequence
- Chaque sequence est composé de : 
	1. Un token (1 bytes) :
		- Le token est diviser en deux champs de 4 bit chaque un. Les 4 bits les plus fort represente la taille du literal et 4 autres bits represente la taille de notre reference
	2. La longeur du literal (optionnel) :
		- Il est present si le literal est plus grand que 15 ???
		- Est encoder grace aux 4 bits les plus fort de notre token
	3. Literal : Le bytes des données en brut
	4. Offset (2 bytes en little eldian):
	5. Match length : Present si la taille de la reference est superieur a 19 ???