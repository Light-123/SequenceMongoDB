## TP NOTE RESTAURANT

La base se nomme : TP_Note_Restaurant

La collection : Liste_Restaurant

Le jeu de données a été modifié

voici ci-dessous les différentes modification:

**Un tableau d'objet avis avec comme élément la note définit par défaut en entier**

```
db.Liste_Restaurant.updateMany({},{$push:{"avis":{"note":1}}})
```
**Attribution de note comprise entre 0 et 5**

```javascript
db.Liste_Restaurant.updateMany(
   {},
   [
      { $set:
         { "avis":
		{"note":
            { $floor:
               { $multiply: [ { $rand: {} }, 5 ] }
            }
		}
        }
      }
    ]
)
```

**Attribution d'heure comprise entre 0 et 23**

```javascript
db.Liste_Restaurant.updateMany(
   {},
   [
      { $set:
         { "heure_ouverture":
            { $floor:
               { $multiply: [ { $rand: {} }, 23 ] }
            }
         }
      }
    ]
)
```
 **Incrémentation d'heure pour des valeur inférieures à 8(ceci était fait manuellement en changeant les valeurs au fur et à mesures pour la donnée d'entrée et de sortie)**

```javascript
db.Liste_Restaurant.updateMany(
    {"heure_ouverture":1},
    
       { $inc:
 
         {"heure_ouverture":7}
       }
     
 )
 ```

**Conversion du champ heure en string"**




 ```javascript
db.Liste_Restaurant.updateMany(
  {  },
  [{ $set: { heure_ouverture: { $toString: "$heure_ouverture" } } }],
  { multi: true }
)
 ```
 **Concaténation des valeurs des champs "heure_ouverture"**

 ```javascript
 db.Liste_Restaurant.updateMany(
  {  },
  [{ $set: { heure_ouverture: { $concat: [ "$heure_ouverture", "H" ] } } }],
  { multi: true }
)
 ```

### Requêtes MongoDB:

**a. Recherchez les restaurants qui sont ouverts à partir de 18h00. Utilisez la méthode find () et les opérateurs de comparaison pour trouver les documents qui correspondent à vos critères.**
 ```javascript

 //<filtre> Champ :"heure_ouverture" : valeur "18H"
db.Liste_Restaurant.find({"heure_ouverture":"18H"})
 ```



**b. Triez les restaurants par note, du plus haut au plus bas. Utilisez la méthode sort () pour trier les résultats.**
 ```javascript
//<filtre> Champ :"note"(élément de l'objet "avis") trier dans l'ordre décroissant
db.Liste_Restaurant.find().sort({"avis.note":-1})
 ```







### Indexation avec MongoDB:

**a. Créez un index sur le champ d'ouverture des restaurants pour améliorer les performances de la recherche. Utilisez la méthode createIndex ().**
 ```javascript
 //Création d'index pour le champ "heure_ouverture et une collation avec le paramètre locale définit en français
db.Liste_Restaurant.createIndex( { "heure_ouverture": 1 }, { collation: { locale: "fr" } } )

 ```




**b. Vérifiez que l'index a été créé en utilisant la méthode listIndexes ().**


 ```javascript
 //Méthode de retour des indexes de la collection
db.Liste_Restaurant.GetIndexes()

 ```


### Requêtes géospatiales:

**a. Recherchez les restaurants qui se trouvent à moins de 2 km d'une certaine localisation. Utilisez la méthode find () avec un opérateur géospatial pour trouver les restaurants à l'intérieur d'un cercle.**

 ```javascript

 //création d'index 2dsphere pour permetre l'utilisation d'opérateur géo-spatiale
db.Liste_Restaurant.createIndex({  "geometry":"2dsphere" })
	
//Déclaration de variable de type point à rentrer en paramètre à la méthode suivante    
var certaine_localisation = {type:"Point", coordinates:[0.6249332400000001,44.17494039999998
]}
//Filtrage des restaurants ayant pour une distance à moins de 2km(2000 mètre) du point de repère "certaine_localisation"
db.Liste_Restaurant.find(

	{"geometry":{
		$nearSphere:{
			$geometry:certaine_localisation,$maxDistance:2000,
			}	
		}
	}
	,{"_id":1,"fields.decription":1})
    //retour de la description incluant le nom du restaurant
    //(il n'y a pas de champ nom et l'élément du champ "field" se nomme decription)
    
 ```
  

**b. Recherchez les restaurants qui se trouvent dans un certain rayon autour d'un point de localisation spécifique. Utilisez la méthode find () avec un opérateur géospatial pour trouver les restaurants à l'intérieur d'un cercle.**

 ```javascript
 //Recherche de restaurants situé dans le rayon circulaire du point définit en paramètre
 //[ 0.6249332400000001, 44.17494039999998] sur un radian de 10 miles divisé par le radian équatorial de la Terre (3963.2 miles)
db.Liste_Restaurant.find( {
  "geometry": { $geoWithin: { $centerSphere: [ [ 0.6249332400000001, 44.17494039999998
 ], 10/3963.2 ] } }
} )  

 ```
  


### Framework d'agrégation:

**a. Calculez la moyenne des notes des restaurants. Utilisez le framework d'agrégation de MongoDB pour effectuer des calculs sur les données.**


 ```javascript

 //Projecte un groupe de données composé de l'id des objets avis et la moyenne de leurs notes respectives
a-db.Liste_Restaurant.aggregate(
   [{ $unwind: "$avis" },
     {
       $group:
         {
           _id: "$_id",
           avgQuantity:  { $avg : "$avis.note"} 
         }
     }
   ]
)
 ```


**b. Trouvez les restaurants les plus populaires en fonction du nombre de commentaires. Utilisez le framework d'agrégation de MongoDB pour groupes les données et effectuer des calculs.**





