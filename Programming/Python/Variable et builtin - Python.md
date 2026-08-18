- Python embarque de nombreuses fonctions qui sont utilisable sans faire d'import aux global
![img](https://raw.githubusercontent.com/Asabeneh/30-Days-Of-Python/refs/heads/master/images/builtin-functions.png)
- Les variables stock des données dans la memoire de l'ordi 
- Pour les nom des variables :
	- Nom facile a retenir et a associer
	- Un nom de variable doit commencer par une lettre ou un tiret du bas
	- Un nom de variable ne peut pas commencer par un chiffre
	- Un nom de variable ne peut contenir que des caractères alphanumériques et des tiret du bas (A-z, 0-9 et _ )
	- Généralement les dev en python utilisent la convetion de nommage camel snake (first_name, person_info, etc ...)
- La fonction `print()` accepte un nombre illimiter d'arguments
- Il est possible de definir plusieurs variabble sur une seul ligne
```
first_name, last_name, country, age, is_married = 'Asabeneh', 'Yetayeh', 'Helsinki', 250, True
```
- On peux utiliser `input()` pour lire sur l'stdin
- Pour convertir (cast) une donnée dans un autre type nous devons utiliser le constructor du type `int()`, `float()`, `str()`, `list()`, etc ...