# Guide détaillé sur MongoDB

## Introduction
Ce guide fournit une série de requêtes MongoDB permettant d'explorer et d'interroger la base de données `lesfilms`. Il vise à offrir une approche méthodique et structurée pour analyser le contenu de cette base.

## 1. Vérification de l'importation des données
### Objectif : Vérifier que les données ont été correctement importées en comptant le nombre total de documents.
```javascript
db.lesfilms.count()
```

## 2. Exploration de la structure des documents
### Objectif : Examiner un document de la collection `lesfilms` pour comprendre sa structure et les champs disponibles.
```javascript
db.lesfilms.findOne()
```

## 3. Récupération des films d'action
### Objectif : Filtrer et afficher les films appartenant au genre "Action".
```javascript
db.lesfilms.find({ genre: "Action" })
```

## 4. Comptage des films d'action
### Objectif : Déterminer le nombre total de films d'action.
```javascript
db.lesfilms.count({ genre: "Action" })
```

## 5. Films d'action produits en France
### Objectif : Afficher les films d'action produits en France.
```javascript
db.lesfilms.find({ genre: "Action", pays: "France" })
```

## 6. Films d'action produits en France en 1963
### Objectif : Filtrer les films d'action produits en France en 1963.
```javascript
db.lesfilms.find({ genre: "Action", pays: "France", annee: 1963 })
```

## 7. Films d'action en France sans les grades
### Objectif : Exclure le champ "grades" des résultats.
```javascript
db.lesfilms.find({ genre: "Action", pays: "France" }, { grades: 0 })
```

## 8. Films d'action en France sans les identifiants
### Objectif : Supprimer l'affichage des identifiants (_id).
```javascript
db.lesfilms.find({ genre: "Action", pays: "France" }, { _id: 0 })
```

## 9. Affichage des titres et grades sans identifiants
### Objectif : Montrer uniquement les titres et grades.
```javascript
db.lesfilms.find({}, { _id: 0, titre: 1, grades: 1 })
```

## 10. Films ayant au moins une note supérieure à 10
### Objectif : Afficher les films dont une des notes est supérieure à 10.
```javascript
db.lesfilms.find({ "grades.note": { $gt: 10 } })
```

## 11. Films dont toutes les notes sont supérieures à 10
### Objectif : Filtrer les films ayant exclusivement des notes > 10.
```javascript
db.lesfilms.find({ grades: { $not: { $elemMatch: { note: { $lte: 10 } } } } })
```

## 12. Liste des genres distincts
### Objectif : Extraire tous les genres distincts présents dans la base.
```javascript
db.lesfilms.distinct("genre")
```

## 13. Différents grades attribués
### Objectif : Obtenir la liste des grades existants.
```javascript
db.lesfilms.distinct("grades")
```

## 14. Films sans résumé
### Objectif : Identifier les films dont le champ "résumé" est absent ou vide.
```javascript
db.lesfilms.find({ resume: { $exists: false } })
```

## 15. Films avec Leonardo DiCaprio en 1997
### Objectif : Rechercher les films de Leonardo DiCaprio sortis en 1997.
```javascript
db.lesfilms.find({ acteurs: "Leonardo DiCaprio", annee: 1997 })
```

## 16. Films avec Leonardo DiCaprio ou produits en 1997
### Objectif : Afficher les films où Leonardo DiCaprio figure ou qui ont été produits en 1997.
```javascript
db.lesfilms.find({ $or: [ { acteurs: "Leonardo DiCaprio" }, { annee: 1997 } ] })
```

## Conclusion
Ce guide fournit une vue d'ensemble des principales requêtes MongoDB permettant d'explorer une base de données cinématographique. Grâce à ces commandes, vous pourrez filtrer et analyser efficacement les films selon divers critères, offrant ainsi une meilleure compréhension des données stockées.

