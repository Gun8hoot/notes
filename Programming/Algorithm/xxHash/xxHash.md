- `xxhash` est un algorithme de hashage qui nous donne un hash avec une taille de 32bit ou 64 bit en tant que "fingerprint" ou digest
- Il a été pense pour la vitesse et eviter que deux message different ont le meme hash
- Listes des operateur utiliser par l'algo
	- Addition modulaire : 
		- Un modulos va etre mis apres chaque addition
	$$addition\ modulaire=255+5\ mod\ 32$$
	- Soustraction modulaire :
		- Un modulos va etre mis apres chaque soustraction
	$$soustraction\ modulaire=255-5\ mod\ 32$$
	- Multiplication modulaire : 
		- Un modulos va etre mis apres chaque multiplication
	$$multiplication\ modulaire=255\times5\ mod\ 32$$
	- Shift circulaire vers la gauche : 
		- A le meme role que un shift vers la gauche, sauf que 
	- Shift vers la droite : 
		- Operation bitwise `>>` clasique 
	- Shift vers la gauche : 
		- Operation bitwise `<<` clasique 
	- XOR :
		- Operation bitwise `^` classique
	- Negation bitwise :
		- Operation bitwise `~` classique

- L'algorithme utilise des nombres premiers:

| NOM       | Valeur      |
| --------- | ----------- |
| PRIME32_1 | 0x9E3779B1U |
| PRIME32_2 | 0x85EBCA77U |
| PRIME32_3 | 0xC2B2AE3DU |
| PRIME32_4 | 0x27D4EB2FU |
| PRIME32_5 | 0x165667B1U |
- Etapes faites par l'algoritme :
	[[1. Initialisation des accumulateurs]]
- Les etapes de hashages sont differents si la donnée a hasher est < 16 bytes
	[[X. Cas special]]