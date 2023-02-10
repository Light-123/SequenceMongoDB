Voici la base de donnees qui permet d'effectuer la serie d'exercices : 

## La base créer : **EXO_DB** ##


```javascript
db.salles.insertMany([ 
   { 
       "_id": 1, 
       "nom": "AJMI Jazz Club", 
       "adresse": { 
           "numero": 4, 
           "voie": "Rue des Escaliers Sainte-Anne", 
           "codePostal": "84000", 
           "ville": "Avignon", 
           "localisation": { 
               "type": "Point", 
               "coordinates": [43.951616, 4.808657] 
           } 
       }, 
       "styles": ["jazz", "soul", "funk", "blues"], 
       "avis": [{ 
               "date": new Date('2019-11-01'), 
               "note": NumberInt(8) 
           }, 
           { 
               "date": new Date('2019-11-30'), 
               "note": NumberInt(9) 
           } 
       ], 
       "capacite": NumberInt(300), 
       "smac": true 
   }, { 
       "_id": 2, 
       "nom": "Paloma", 
       "adresse": { 
           "numero": 250, 
           "voie": "Chemin de l'Aérodrome", 
           "codePostal": "30000", 
           "ville": "Nîmes", 
           "localisation": { 
               "type": "Point", 
               "coordinates": [43.856430, 4.405415] 
           } 
       }, 
       "avis": [{ 
               "date": new Date('2019-07-06'), 
               "note": NumberInt(10) 
           } 
       ], 
       "capacite": NumberInt(4000), 
       "smac": true 
   }, 
    { 
       "_id": 3, 
       "nom": "Sonograf", 
       "adresse": { 
           "voie": "D901", 
           "codePostal": "84250", 
           "ville": "Le Thor", 
           "localisation": { 
               "type": "Point", 
               "coordinates": [43.923005, 5.020077] 
           } 
       }, 
       "capacite": NumberInt(200), 
       "styles": ["blues", "rock"] 
   } 
]) 
```

Exercice 1

Affichez l’identifiant et le nom des salles qui sont des SMAC.
```javascript 
**db.salles.find({},{"_id":1, "nom":1}) ou db.salles.find({},{"nom":1}) -->Sachant que le champ "_id" qui est la clé primaire est par défaut sélectionné**
//Sur la collection salles la méthode find() est appellé, la requête cherche les documents ayant pour projection id et nom en true afin d'obtenir comme résultat uniquement le nom et job(comme l'id est la clé primaire d'un document on se doit lui donner la valeur false pour qu'elle ne soit pas visible sinon elle sera d'office)
```
Exercice 2

Affichez le nom des salles qui possèdent une capacité d’accueil strictement supérieure à 1000 places.

```javascript 
db.salles.find({ "capacite" :{ $gt : 1000 }})
//Sur la même collection grâce à la méthode "find()" nous cherchons pour le champ "capacite" strictement supérieur à 1000 par l'intérmediaire de l'opérateur $gt***
```

Exercice 3

Affichez l’identifiant des salles pour lesquelles le champ adresse ne comporte pas de numéro.

```javascript 
db.salles.find({"adresse.numero": null }  )
//Dans la méthode "find()" la requête viens parcourir le sous-champ "numero" du champ "adresse" et chercher une valeur null
```

Exercice 4

Affichez l’identifiant puis le nom des salles qui ont exactement un avis.

```javascript 
db.salles.find({"avis":{$exists:1}},{"_id":1, "nom":1 })
//Avec la méthode find() dans la l'espace requête nous nous ciblons sur  le champ "avis" en cherchant une valeur de 1 grâce à l'opérateur "$exists". Puis à la partie projection les champs id et nom prennent la valeur 1(true) afin qu'ils en soient resultés.
```


Exercice 5

Affichez tous les styles musicaux des salles qui programment notamment du blues.

```javascript 
db.salles.find({"styles":'blues'}, {"styles":1, "_id":0}) ou db.salles.find({"styles":{$exists:'blues'}}, {"styles":1, "_id":0})
//Par la méthode find() nous cherchons pour le champ "styles" de type tableau une valeur "blues" existante ,on projete uniquement le champ "style" et non l'id de chaque document correspondant à la requête
```

Exercice 6

Affichez tous les styles musicaux des salles qui ont le style « blues » en première position dans leur tableau styles.

```javascript 

db.salles.find({"styles.0":"blues"})
//Par la méthode find() nous cherchons pour le premier élément du champ "styles" de type tableau une valeur "blues" existante ,on projete uniquement le champ "styles" et non l'id
```



Exercice 7

Affichez la ville des salles dont le code postal commence par 84 et qui ont une capacité strictement inférieure à 500 places (pensez à utiliser une expression régulière).

```javascript 
db.salles.find({"adresse.codePostal": {$regex :"84(?-i)"},"capacite": {$lt : 500}}, {"adresse.ville":1,"_id":0})
//Filtrage de document par l'élément "codePostal" de du tableau adresse commençant par 84 avec une capacité supérieur à 500 et une projection de la ville
```



Exercice 8

Affichez l’identifiant pour les salles dont l’identifiant est pair ou le champ avis est absent.

```javascript 
db.salles.find(  {  $or : [ {"_id": { $mod: [2,0] }} ,   { "avis" : null } ] }, {"_id":1})
//Filtrage de documents où l'avis est absent et l'id est pair grâce à l'opérateur "$mod" (modulo)
```

Exercice 9

Affichez le nom des salles dont au moins un des avis comporte une note comprise entre 8 et 10 (tous deux inclus).

```javascript 
db.salles.find(  { "avis" : { $elemMatch :  { "note" : { $gte:9  , $lte:10  }  }  , $exists: 1}}, {"nom":1, "_id":0}  )
//Filtrage de document où les notes sont supérieur ou égale à 8 et inférieures ou égales à 10 dont le nom des salles existent avec une projection des noms uniquement
```


Exercice 10

Affichez le nom des salles dont au moins un des avis comporte une date postérieure au 15/11/2019 (pensez à utiliser le type JavaScript Date).

```javascript 
db.salles.find(  { "avis": {  $elemMatch: {"date": {$gt: ISODate('2019-11-15') } } }}, {"nom":1, "_id":0 } )
//Filtrage de documents où la date d'un avis est postérieurs au 15/11/2019 grâce à l'opérateur strictement supérieur "$gt" avec pour projection le nom de salle
```




Exercice 11

Affichez le nom ainsi que la capacité des salles dont le produit de la valeur de l’identifiant par 100 est strictement supérieur à la capacité.

```javascript 
db.salles.find({ $expr : { $gt:[ {$multiply:["$_id", 100 ]},"$capacite"]}},{"nom":1,"id":0})
//Filtrage des documents possédant la valeur id mulitplié par 100 par l'intérmediaire de l'opérateur "$multiply" dont il doit être supérieur à sa capacité
```


Exercice 12

Affichez le nom des salles de type SMAC programmant plus de deux styles de musiques différents en utilisant l’opérateur $where qui permet de faire usage de JavaScript.




Exercice 13

Affichez les différents codes postaux présents dans les documents de la collection salles.
```javascript 
db.salles.find({},{"adresse.codePostal":1,"_id":0} )
//Projection des code postaux présents dans la collection
```

Exercice 14

Mettez à jour tous les documents de la collection salles en rajoutant 100 personnes à leur capacité actuelle.
```javascript 
db.salles.updateMany({},{ $inc : {"capacite":100}})

//Mise à jour des capacité par incrémentation de 100 par l'opérateur "$inc" 
```



Exercice 15

Ajoutez le style « jazz » à toutes les salles qui n’en programment pas.

```javascript 

db.salles.updateMany({"styles":{$nin:["jazz"]}},{ $addToSet : {"styles":"jazz"}})
//Ajout du style jazz pour un filtre des salles ne programmant pas celui-ci grâce à "$nin" par l'intermédiaire de l'opérateur "$addToSet"
```


Exercice 16

Retirez le style «funk» à toutes les salles dont l’identifiant n’est égal ni à 2, ni à 3.

```javascript 
db.salles.updateMany({$nor:[{"_id":2},{"_id":3}]},{ $pull : {"styles":"funk"}})
//Retrait du style "funk" avec l'opérateur "$pull" pour un filtre utilisant "$nor" ciblant les ids 1 et 2
```

Exercice 17

Ajoutez un tableau composé des styles «techno» et « reggae » à la salle dont l’identifiant est 3.
le nouveau tableau ajouté composé des styles s'appelle styles_2

```javascript 
db.salles.updateOne({"_id":3},{$push:{"styles_2":"techno","reggae"}})
//Ajout de tableau grâce à l'opérateur "$push"
```

Exercice 18

Pour les salles dont le nom commence par la lettre P (majuscule ou minuscule), augmentez la capacité de 150 places et rajoutez un champ de type tableau nommé contact dans lequel se trouvera un document comportant un champ nommé telephone dont la valeur sera « 04 11 94 00 10 ».


```javascript 
db.salles.updateMany({"nom":{$regex :"(?i)p(?-i)"}},{ $inc : {"capacite":150} , $push : {"telephone" : "04 11 94 00 10" }} )
//Mise à jour des salles avec un nom commençant par "p" peu importe la casse ((?-i):majuscule et (?i):minuscule) grâce au paramètre entourant le caractère pour lequel un champ "telephone" es ajouté
```
Exercice 19

Pour les salles dont le nom commence par une voyelle (peu importe la casse, là aussi), rajoutez dans le tableau avis un document composé du champ date valant la date courante et du champ note valant 10 (double ou entier). L’expression régulière pour chercher une chaîne de caractères débutant par une voyelle suivie de n’importe quoi d’autre est [^aeiou]+$.

```javascript 
db.salles.updateMany({ "nom": { $regex : /^[aeiou]/ , $options:"i"}},{$addToSet:{"avis":{"date":Date(), "note":NumberInt(10)}}})
//Ajout des avis avec une date (methode Date() utilisé retournant la date courante)  des salles commençant par une voyelle aidé par "$regex"
```

Exercice 20

En mode upsert, vous mettrez à jour tous les documents dont le nom commence par un z ou un Z en leur affectant comme nom « Pub Z », comme valeur du champ capacite 50 personnes (type entier et non décimal) et en positionnant le champ booléen smac à la valeur « false ».

```javascript 
db.salles.updateMany({ "nom": { $regex : /^z/ , $options:"i"}},{$set: {"nom" : "Pub Z", "capacite":NumberInt(50),"smac":true}},{"upsert":true})
//Les salles commençant par z peu importe la casse sont renommé par "Pub Z" et possède une capacité de type integer d'une valeur de 50 avec en possession une smac
```


Exercice 21

Affichez le décompte des documents pour lesquels le champ _id est de type « objectId ».

```javascript 
db.salles.find({"_id": {$type:"objectId"}}).count()
//"objectId":alias
//Filtrage des id de type objectId(avec l'opérateur "$type") avec une méthode count pour retourer le nombre d'occurrence/documents
``` 

Exercice 22

Pour les documents dont le champ _id n’est pas de type « objectId », affichez le nom de la salle ayant la plus grande capacité. Pour y parvenir, vous effectuerez un tri dans l’ordre qui convient tout en limitant le nombre de documents affichés pour ne retourner que celui qui comporte la capacité maximale.
```javascript 
db.salles.find({"_id": { $not:{$type:"objectId"}}},{"nom":1,"_id":0}).sort({"capacite":-1}).limit(1)
//Recherche de salle possédant une id qui n'est pas du type objectId grâce à l'opérateur $type
```

Exercice 23

Remplacez, sur la base de la valeur de son champ _id, le document créé à l’exercice 20 par un document contenant seulement le nom préexistant et la capacité, que vous monterez à 60 personnes.
```javascript 
db.salles.replaceOne({"_id":ObjectId("63e42b6db8bb54a45e6a4656")},{"nom":"Pub Z", "capacite":NumberInt(60)})
//Requête unique avec la méthode replaceOne() de la salle "Pub Z" en changeant uniquement sa capacité
```



Exercice 24

Effectuez la suppression d’un seul document avec les critères suivants : le champ _id est de type « objectId » et la capacité de la salle est inférieure ou égale à 60 personnes.

```javascript 
db.salles.deleteOne({"_id": {$type:"objectId"}, "capacite": { $lte:60}})
//Requête unique de supression d'un document possédant une capacité inférieur ou égale à 60("$lte") de type objectId
```

Exercice 25

À l’aide de la méthode permettant de trouver un seul document et de le mettre à jour en même temps, réduisez de 15 personnes la capacité de la salle située à Nîmes.
```javascript 
db.salles.findOneAndUpdate({"adresse.ville":"Nîmes"},{$inc:{"capacite": -15}})
//Rercherche et mise à jour de la ville pour laquelle la capacité est décrémenté de 15 avec l'opérateur "$inc" On remarque que qu'on obtient le document original avant la mise à jour
```