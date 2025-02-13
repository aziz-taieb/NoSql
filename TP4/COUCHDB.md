# Présentation de CouchDB

## Introduction
Apache CouchDB est une base de données NoSQL orientée document, développée et maintenue par la Fondation Apache. Elle repose sur un modèle de stockage JSON, utilise le protocole HTTP pour l'accès aux données via une API RESTful, et intègre une architecture distribuée avec réplication automatique. CouchDB se distingue par sa robustesse, sa simplicité d’installation, et sa grande flexibilité en termes de structuration des données.

## Modes d’installation

### Installation locale
Il est possible d’installer CouchDB directement sur un système d’exploitation (Windows, macOS, Linux) en téléchargeant et en installant les packages officiels fournis par Apache. Cette méthode est idéale pour les tests en local et les petites applications.

### Installation via Docker
L'utilisation de Docker permet de déployer rapidement CouchDB sans configuration complexe. Avantages de cette approche :

- **Déploiement rapide** : Un simple `docker run` permet de lancer un conteneur CouchDB opérationnel.
- **Facilité de mise à jour** : Mise à niveau simple en changeant l’image utilisée.
- **Gestion des données** : Les volumes Docker permettent de persister les données même après la suppression du conteneur.

Exemple de commande pour lancer CouchDB avec Docker :
```bash
docker run -d --name couchdb -p 5984:5984 -v /mydata:/opt/couchdb/data couchdb
```

## Fonctionnalités principales

### API REST
CouchDB repose sur une API RESTful qui facilite les interactions avec la base de données via des requêtes HTTP. Les principales opérations incluent :

- **GET** : Récupérer des documents ou des informations sur la base.
- **PUT** : Créer ou mettre à jour un document.
- **POST** : Ajouter un document sans fournir d’identifiant explicite.
- **DELETE** : Supprimer un document.

Exemple d’insertion d’un document via `curl` :
```bash
curl -X PUT http://localhost:5984/ma_base/mon_doc -d '{"nom":"Alice", "age":25}' -H "Content-Type: application/json"
```

### Interface graphique intégrée
CouchDB propose une interface web, **Fauxton**, accessible via un navigateur à l'adresse `http://localhost:5984/_utils/`. Cette interface permet :

- La gestion des bases de données et des documents.
- L’exécution de requêtes MapReduce.
- La configuration des paramètres de réplication et de sécurité.

### Flexibilité des documents JSON
Contrairement aux bases relationnelles, CouchDB ne nécessite pas de schéma prédéfini. Chaque document est un objet JSON autonome qui peut évoluer sans contrainte de structure.

## Avantages de CouchDB

- **Versioning natif** : Chaque document inclut un identifiant de révision (`_rev`) qui permet de gérer l'historique des modifications.
- **Insertion en batch** : Possibilité d'insérer plusieurs documents en une seule requête pour optimiser les performances.
- **Réplication et haute disponibilité** : Synchronisation automatique des bases de données entre plusieurs serveurs, idéale pour les applications distribuées.
- **Facilité d’utilisation** : Adapté aussi bien aux débutants qu’aux développeurs expérimentés.

## Fonctionnement de MapReduce avec CouchDB

CouchDB exploite **MapReduce**, une technique permettant de structurer et d’agréger les données efficacement.

### Introduction à MapReduce
Initialement conçu pour le traitement de données massives, MapReduce dans CouchDB est utilisé pour :

- **Créer des vues indexées** qui facilitent les recherches.
- **Effectuer des calculs distribués** sur des ensembles de documents volumineux.

### Fonction Map
La fonction **Map** est exécutée sur chaque document et génère des paires clé-valeur.

#### Exemple : Extraction des films par année
```javascript
function (doc) {
  if (doc.type === "film") {
    emit(doc.year, doc.title);
  }
}
```

**Explication :**

- Chaque document de type "film" génère une entrée avec l’année comme clé et le titre comme valeur.

### Fonction Reduce
La fonction **Reduce** agrège les valeurs regroupées par clé après la phase de tri.

#### Exemple : Comptage du nombre de films par année
```javascript
function (keys, values, rereduce) {
  return values.length;
}
```

**Explication :**

- Cette fonction compte le nombre d’entrées associées à chaque clé (année de sortie des films).

### Génération de vues
CouchDB utilise deux types de vues :

- **Vues temporaires** : Créées dynamiquement pour des requêtes ponctuelles.
- **Vues permanentes** : Stockées et réutilisées pour optimiser les performances des requêtes fréquentes.

## Exercice n°1 : Modélisation et Calcul avec MapReduce

### Modélisation de la matrice M sous forme de documents
Chaque ligne de la matrice peut être représentée par un document JSON contenant les poids des liens sortants d'une page web donnée.

```json
{
  "page": "P1",
  "liens": [
    {"vers": "P2", "poids": 0.3},
    {"vers": "P3", "poids": 0.7}
  ]
}
```

### Calcul de la norme des vecteurs avec MapReduce

#### Fonction Map
```javascript
function (doc) {
  var somme = 0;
  doc.liens.forEach(function(lien) {
    somme += Math.pow(lien.poids, 2);
  });
  emit(doc.page, Math.sqrt(somme));
}
```

#### Fonction Reduce
```javascript
function (keys, values, rereduce) {
  return values;
}
```

### Produit Matrice-Vecteur avec MapReduce

#### Fonction Map
```javascript
function (doc) {
  doc.liens.forEach(function(lien) {
    emit(lien.vers, lien.poids * W[lien.vers]);
  });
}
```

#### Fonction Reduce
```javascript
function (keys, values, rereduce) {
  return values.reduce(function(a, b) { return a + b; }, 0);
}
```

## Conclusion
Apache CouchDB est une solution robuste et évolutive pour la gestion des bases de données documentaires. Son API REST, son modèle flexible sans schéma et son architecture distribuée en font un choix idéal pour les applications nécessitant un stockage efficace et une synchronisation fiable des données. Grâce à MapReduce, il offre également des capacités puissantes d’indexation et d’agrégation, adaptées aux besoins des applications modernes.

