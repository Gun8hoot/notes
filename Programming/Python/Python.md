- Python est un language de programation haut niveau qui a été concu pour un usage general
- Il est un language interpreter ce qui veux dire que le code ecrit en python n'est pas compiler mais juste lu puis executer durant l'execution
- La synthax est assez proche des langue humaine ce qui le rend assez simple a apprendre
- Les fichier en python ont une extension en `.py`

| [[Operateur - Python]] | **DESCRIPTION**                                   |
| ---------------------- | ------------------------------------------------- |
| 3 + 2 = 5              | Addition                                          |
| 3 - 2 = 1              | Soustraction                                      |
| 3 / 2 = 1.5            | Division                                          |
| 3 * 2 = 6              | Multiplication                                    |
| 3 ** 2 = 9             | Puissance                                         |
| 3 % 2                  | Modulos, permet d'obtenir le reste d'une division |
| 3 // 2                 | Retire le reste de la division                    |
- Les commentaire s'ecrivent en mettant un `#
```py
# This is the first comment
# This is the second comment
# Python is eating the world
```
- Les commentaire sur plusieurs lignes s'ecrivent en mettant trois `"""` 
```py
"""This is multiline comment
multiline comment takes multiple lines.
python is eating the world
"""
```

- En python il existe plein de type de données differentes : 
	- Nombre : 
		- Int = Nombre rond comme 3, 6 ou 12
		- Float = Nombre a virgule comme 3.14, 999.9 ou 0.3333333
		- Nombre complexe = On y reviendra dessus plus tard
	- Chaine de charactere ([[Strings - Python]]) :
		- Collection d'un ou plusieurs charactere
		- On les definit en mettant des simple quote ou avec des doubles quotes
	- Boolean : 
		- Type qui peux uniquement etre mis a `True` ou `False`
	- [[Liste - Python]] :
		- Nous permet de stocker des données de different type
		- Est similaire aux array dans les autres language
		- `list = ["str1", 28, True]`
	- Dictionnaire :
		- Nous permet de stocker des données dans un format pair de key:value
		- `arr = {'age':99, 'skills':['C', 'C++']}`
	- [[Tuple - Python]] : 
		- Comme une list mais on le peux pas modifier les données
		- `('Earth', 'Jupiter', 'Neptune', 'Mars', 'Venus', 'Saturn', 'Uranus', 'Mercury')
	- [[Sets - Python]] : 
		- Est comme les list et set mais le set stock que un seul type de donnée
		- `{2, 4, 3, 5}`

[[Variable et builtin - Python]]