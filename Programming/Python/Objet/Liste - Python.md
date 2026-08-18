- Une liste est une collection de données ordonnée (qui est a un index), modifiable et qui accepte les doublons
- Pour crée une liste il y a deux possibiliter :
	- Utiliser le constructeur de l'objet list `list()`
		- `lst = list() # Nous cree une liste vide`
	- Utiliser des `[]`
		- `lst = [] # Nous crée une liste vide`
- On utilise `len()` afin de connaitre la taille de notre liste
- Les liste peuvent contenir plusieurs type d'element different
- On peux acceder aux elements qui sont stocker a l'interieur d'une liste grace a un indice (positif ou negatif)
	- `first_fruit = fruits[0] # Donne le premiere element`
	- `first_fruit = fruits[-1] # Donne le derniere`
- Il existe un fonctionnaliter qui s'appelle le depacktage, elle permet de stocker dans un autre objet l'element a cette indice et le reste des elements
	`lst = ['item1','item2','item3', 'item4', 'item5']`
	`first_item, second_item, third_item, *rest = lst # rest va contenir ['item4', 'item5']`
	- Le depaquetage peux etre possible uniquement lorsque l'on definit plusieurs variable sur la meme ligne
- Il est egalement possible de faire du slicing avec les liste
```python
# --- INDICE POSITIF ---
fruits = ['banana', 'orange', 'mango', 'lemon']
all_fruits = fruits[0:4] # renvoie tous les fruits
# donne aussi le même résultat
all_fruits = fruits[0:] # si on ne fixe pas de fin, on prend tout le reste
orange_and_mango = fruits[1:3] # n'inclut pas le premier indice
orange_mango_lemon = fruits[1:]
orange_and_lemon = fruits[::2] # on utilise un 3e argument, le pas. Prend un élément sur deux - ['banana', 'mango']

--- INDICE NEGATIF ---
fruits = ['banana', 'orange', 'mango', 'lemon']
all_fruits = fruits[-4:] # renvoie tous les fruits
orange_and_mango = fruits[-3:-1] # n'inclut pas le dernier indice, ['orange', 'mango']
orange_mango_lemon = fruits[-3:] # de -3 jusqu'à la fin, ['orange', 'mango', 'lemon']
reverse_fruits = fruits[::-1] # un pas négatif prend la liste en ordre inverse, ['lemon', 'mango', 'orange', 'banana']
```
- Il est possible de modifier un element de notre liste en fessant
	- `fruits[1] = 'pineapple'`
- On peux verifier la presence d'un element en fessant 
	- `does_exist = 'banana' in fruits # Va nous renvoyer True si 'banana' est dans la liste`
- Pour ajouter des element a notre liste nous devons utiliser la methode `.append()`
	- `fruits.append('apple')`
- Nous pouvons inserer un element a un index precis avec la methode `.insert()`
	- `lst.insert(index, item)`
- On peut retirer un element par sa valeur avec la methode `.remove()`
	- `lst.remove('banana')`
- On retire le derniere element d'une liste avec `.pop()`
	- `lst.pop()`
- On peux utilier le keyword `del` pour supprimer un element a un index (ou toute la liste)
	- `del lst[index] # un seul élément`
	- `del lst        # supprime complètement la liste`
- On peux vider une liste avec `.clear()`
- On peux faire une copie de notre liste avec `.copy()`
	- Lorsque l'on fait `lst2 = lst1`, python ne fait pas réelment une copie. Il donne juste la **reference de lst1 a lst2**. Ce qui veux dire que si un element est changer dans lst2, il va egalement l'etre dans lst1
- On peux conconcatener une liste dans une autre de deux facon :
	- Operateur + :
		- `lst3 = lst1 + lst2`
	- `.extend()`
		- `lst2.extend(lst1)`
- On utilise `.count()` pour compter l'occurence d'un element dans une liste
	- `lst.count(item)`
- On utilise `.index()` pour trouver l'index d'un element
	- `lst.index(item)`
- On utilise `.reverse()` pour inverser notre liste
- On peux utiliser `.sort()` pour trier une liste
- On peux utiliser `.sorted()` pour savoir si notre liste est trié
