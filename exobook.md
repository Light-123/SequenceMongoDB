# Créez une base de données sample nommée "sample_db" et une collection appelée "employees".


### Création de la collection employees ###
**db.createCollection("employees",{"collation":{"locale":"fr"}})la variable db appelle la methode createCollection pour créer une collection du nom de "employees" ayant pour valeur au champ "locale" faisant partie des collations  fr, cette valeur indique la langue locale définit**

**resultat : { ok: 1 } indique pour le champ ok une valeur true, on en déduit donc que la requête à bien été executé**

## Insérez les documents suivants dans la collection "employees": ##

```javascript
**db.employees.insertMany([{
   name: "John Doe",
   age: 35,
   job: "Manager",
   salary: 80000
},**

**{
   name: "Jane Doe",
   age: 32,
   job: "Developer",
   salary: 75000
},**

**{
   name: "Jim Smith",
   age: 40,
   job: "Manager",
   salary: 85000
}])**
```

***Sur la collection employees une insertion d'un tableau de trois objets est inséré grâce à la méthode "insertMany()ayant pour champs name; age; job et salary***

résultat
```javascript
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("63e11f4aa70858039cb0863b"),
    '1': ObjectId("63e11f4aa70858039cb0863c"),
    '2': ObjectId("63e11f4aa70858039cb0863d")
  }
}

{
   name: "John Doe",
   age: 35,
   job: "Manager",
   salary: 80000
}

{
   name: "Jane Doe",
   age: 32,
   job: "Developer",
   salary: 75000
}

{
   name: "Jim Smith",
   age: 40,
   job: "Manager",
   salary: 85000
} 
```
Écrivez une requête MongoDB pour trouver tous les documents dans la collection "employees".

```javascript
db.employees.find()//cette requête nous pointe sur la collection "employees" et appelle la méthode find() pour chercher tous les documents de celle-ci.
```

```javascript 
use sample_db Cette requête //initialise une base de données nommée sample_db
'switched to db sample_db'//nous nous dirigons sur cette base de données
--Remarque Sur MongoDB compass malgré un reload data la base n'est toujours pas visible sur l'arborescence**
```

Écrivez une requête pour trouver tous les documents où l'âge est supérieur à 33.
```javascript 
db.employees.find({"age":{$gt:33}})
//Sur la collection "employees" la requête appelle la méthode find() avec pour parametre le champ "age" et une projection comportant un opérateur strictement supérieur à la valeur 33
```
```javascript Resultat : {
  _id: ObjectId("63e11f4aa70858039cb0863b"),
  name: 'John Doe',
  age: 35,
  job: 'Manager',
  salary: 80000
}
{
  _id: ObjectId("63e11f4aa70858039cb0863d"),
  name: 'Jim Smith',
  age: 40,
  job: 'Manager',
  salary: 85000
}
```

Écrivez une requête pour trier les documents dans la collection "employees" par salaire décroissant.

```javascript 
db.employees.find().sort({"salary":-1})

//La requête appelle la methode find en complément de sort, celle-ci inclus comme paramètre le champ "salary" avec pour valeur -1 indiquant un tri décroissant
```


```javascript 
Résultat:{
  _id: ObjectId("63e11f4aa70858039cb0863d"),
  name: 'Jim Smith',
  age: 40,
  job: 'Manager',
  salary: 85000
}
{
  _id: ObjectId("63e11f4aa70858039cb0863b"),
  name: 'John Doe',
  age: 35,
  job: 'Manager',
  salary: 80000
}
{
  _id: ObjectId("63e11f4aa70858039cb0863c"),
  name: 'Jane Doe',
  age: 32,
  job: 'Developer',
  salary: 75000
}
```

Écrivez une requête pour sélectionner uniquement le nom et le job de chaque document.

```javascript 
db.employees.find({}, {"age":1,"job":1, "_id":0})*
//Par la méthode find() sur la collection employees, la requête cherche les documents ayant pour projection l'age et job en true et _id en false afin d'obtenir comme résultat uniquement le nom et job(comme l'id est la clé primaire d'un document on se doit lui donner la valeur false pour qu'elle ne soit pas visible sinon elle sera d'office).

Résultat:{
  age: 35,
  job: 'Manager'
}
{
  age: 32,
  job: 'Developer'
}
{
  age: 40,
  job: 'Manager'
}
```



Écrivez une requête pour compter le nombre d'employés par poste.

```javascript 
db.employees.find( { "job" : "NOM_DU_POSTE"} ).count()


//La requête cherche par la méthode  find les documents possédant la valeur saisi du champ job, vient s'ajouter la méthode count afin de compter le nombre d'occurrence de la sous requête.
Résultat:
Mangaer:2
Developper :1***
```


Écrivez une requête pour mettre à jour le salaire de tous les développeurs à 80000.

```javascript 
db.employees.updateMany({}, { $set:{"salary" : 80000 } })
//Nous appelons ici la méthode updateMany pour mettre à jour plusieurs documents, dans l'espace requête rien n'est sélectionner afin de cibler la totalité des documents. Dans la partie projection l'opérateur $set est utilisé et attribue la valeur 80000 pour le champ salary.
```