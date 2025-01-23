
# Redis et NoSQL : Systèmes de Gestion de Bases de Données Modernes

## Qu'est-ce que NoSQL ?

### Définition
NoSQL (Not Only SQL) est une approche de gestion de données qui diffère des bases de données relationnelles traditionnelles. Elle permet de stocker et récupérer des données avec des mécanismes qui diffèrent des tables relationnelles classiques.

### Caractéristiques Principales
- Schéma dynamique
- Scalabilité horizontale
- Haute performance
- Traitement de données semi-structurées
- Flexibilité de stockage

Les quatre familles principales de bases de données NoSQL :

1. **Bases de données clé-valeur**
   - Stockage simple en paires clé-valeur
   - Lectures/écritures rapides
   - Exemples : Redis, DynamoDB

2. **Bases de données orientées colonnes**
   - Stockage par colonnes
   - Performantes pour analyses massives
   - Exemples : Cassandra, HBase

3. **Bases de données orientées documents**
   - Données semi-structurées
   - Schéma flexible
   - Exemples : MongoDB, CouchDB

4. **Bases de données orientées graphes**
   - Modélisation des relations complexes
   - Requêtes relationnelles optimisées
   - Exemples : Neo4j, Amazon Neptune

## Redis : Une Solution NoSQL 

### Qu'est-ce que Redis ?

Redis (Remote Dictionary Server) est un système de stockage de données open-source, en mémoire, qui fonctionne comme :
- Un magasin de structures de données
- Un cache
- Un courtier de messages
- Une base de données


## Redis : Base de Données Clé-Valeur en Mémoire

Redis (Remote Dictionary Server) se caractérise par :
- Stockage principalement en mémoire
- Persistance optionnelle
- Hautes performances
- Structures de données avancées

## Installation

```bash
# Installation Ubuntu/Debian
sudo apt-get install redis-server

# Démarrage
sudo systemctl start redis-server

# Connexion
redis-cli
```

### Structures de Données Redis

#### 1. Chaînes de caractères (Strings)
```bash
SET cle "valeur"     # Création/modification
GET cle              # Récupération
INCR cle             # Incrémentation
DEL cle              # Suppression
```
**Particularités** :
- Stockage simple de valeurs textuelles ou numériques
- Permet des opérations atomiques d'incrémentation
- Idéal pour les compteurs, flags, et données simples

#### 2. Listes
```bash
RPUSH liste "element"  # Ajout à droite
LPUSH liste "element"  # Ajout à gauche
LRANGE liste 0 -1      # Affichage complet
```
**Particularités** :
- Ordre préservé
- Opérations d'insertion/suppression aux deux extrémités
- Peut contenir des éléments dupliqués
- Utile pour files d'attente, historiques

#### 3. Ensembles
```bash
SADD ensemble "membre"   # Ajout
SMEMBERS ensemble        # Liste membres
SREM ensemble "membre"   # Suppression
SUNION ensemble1 ensemble2  # Union
```
**Particularités** :
- Éléments uniques (pas de doublons)
- Pas d'ordre spécifique
- Opérations rapides de comparaison/union
- Idéal pour liste de tags, relations

#### 4. Ensembles ordonnés
```bash
ZADD classement 10 "utilisateur1"  # Ajout avec score
ZRANGE classement 0 -1              # Affichage croissant
ZREVRANGE classement 0 -1           # Affichage décroissant
```
**Particularités** :
- Éléments uniques avec score numérique
- Tri automatique par score
- Parfait pour classements, tableaux de scores

#### 5. Hachages
```bash
HSET utilisateur:1 nom "Jean"   # Création champ
HGETALL utilisateur:1           # Récupération complète
HINCRBY utilisateur:1 age 1     # Incrémentation
```
**Particularités** :
- Stockage de champs multiples pour un objet
- Schéma dynamique
- Permet des opérations par champ
- Similaire à un objet/dictionnaire

### Fonctionnalités Avancées

#### Publication/Abonnement
```bash
SUBSCRIBE canal      # Abonnement
PUBLISH canal message # Publication
PSUBSCRIBE motif*    # Abonnement par motif
```
**Particularités** :
- Communication temps réel
- Système de messagerie distribué
- Abonnement par canal ou motif

#### Expiration des Clés
```bash
EXPIRE cle 120       # Expiration dans 120 secondes
TTL cle              # Temps restant
```
**Particularités** :
- Suppression automatique des clés
- Gestion dynamique de la durée de vie
- Utile pour caches, sessions temporaires

### Performance

- Stockage principal en RAM
- Accès mémoire ultra-rapide
- Latence : 10^-8 secondes
- Débit : ~4 Go/seconde

### Cas d'Usage

1. Cache de session
2. Tableaux de bord temps réel
3. Files d'attente
4. Recommandations
5. Compteurs

## Fonctionnalités Supplémentaires

Quatre fonctionnalités avancées :

1. **Transactions** : Exécution atomique de commandes
2. **Lua Scripting** : Scripts côté serveur
3. **Géo-indexation** : Requêtes spatiales
4. **Streaming** : Gestion de flux temps réel

