# Exemple de relations n,n en SQL : élèves et clubs

## Présentation du contexte

Dans une base de données scolaire, il est fréquent de devoir modéliser des situations où une entité peut être liée à plusieurs autres, et réciproquement. C'est le cas par exemple pour des élèves et des clubs :

- Un élève peut s'inscrire à plusieurs clubs.
- Un club accueille plusieurs élèves.

Ce type de relation s'appelle une **relation n,n** (ou "plusieurs à plusieurs", many-to-many en anglais).

Pour représenter cela en SQL, on utilise une **table de jointure** contenant les clés étrangères des deux entités à relier.

---

## Création des tables

```sql
CREATE TABLE eleves (
    id_eleve INT PRIMARY KEY,
    nom VARCHAR(100)
);

CREATE TABLE clubs (
    id_club INT PRIMARY KEY,
    nom VARCHAR(100)
);

CREATE TABLE eleves_clubs (
    id_eleve INT,
    id_club INT,
    PRIMARY KEY (id_eleve, id_club),
    FOREIGN KEY (id_eleve) REFERENCES eleves(id_eleve),
    FOREIGN KEY (id_club) REFERENCES clubs(id_club)
);
```

↔↔---

## Insertion des données

### Les élèves

```sql
INSERT INTO eleves (id_eleve, nom) VALUES
(1, 'Alice'),
(2, 'Bob'),
(3, 'Charlie'),
(4, 'David'),
(5, 'Eva'),
(6, 'Fatima'),
(7, 'Georges'),
(8, 'Hélène');
```

### Les clubs

```sql
INSERT INTO clubs (id_club, nom) VALUES
(1, 'Club Informatique'),
(2, 'Club Théâtre'),
(3, 'Club Échecs'),
(4, 'Club Musique');
```

### Les inscriptions (table de jointure)

```sql
INSERT INTO eleves_clubs (id_eleve, id_club) VALUES
(1, 1),  -- Alice dans Informatique
(1, 3),  -- Alice dans Échecs
(2, 1),  -- Bob dans Informatique
(2, 2),  -- Bob dans Théâtre
(3, 4),  -- Charlie dans Musique
(4, 1),  -- David dans Informatique
(4, 2),  -- David dans Théâtre
(4, 3),  -- David dans Échecs
(5, 2),  -- Eva dans Théâtre
(6, 3),  -- Fatima dans Échecs
(6, 4),  -- Fatima dans Musique
(7, 1),  -- Georges dans Informatique
(7, 4),  -- Georges dans Musique
(8, 2),  -- Hélène dans Théâtre
(8, 3);  -- Hélène dans Échecs
```

---

## Explications

- Chaque élève et chaque club a un identifiant unique (clé primaire).
- La table `eleves_clubs` permet de représenter la relation n<->n en liant les deux entités par leurs identifiants.
- La clé primaire de la table de jointure est composée des deux identifiants pour éviter les doublons.
- On peut ensuite réaliser toutes sortes de requêtes grâce à cette structure.

---

## Exemples de requêtes classiques

### 1. Afficher les clubs d’un élève donné

```sql
SELECT c.nom AS club
FROM clubs c
JOIN eleves_clubs ec ON c.id_club = ec.id_club
JOIN eleves e ON ec.id_eleve = e.id_eleve
WHERE e.nom = 'David';
```

### 2. Afficher les élèves d’un club donné

```sql
SELECT e.nom AS eleve
FROM eleves e
JOIN eleves_clubs ec ON e.id_eleve = ec.id_eleve
JOIN clubs c ON ec.id_club = c.id_club
WHERE c.nom = 'Club Informatique';
```

### 3. Nombre de clubs par élève (statistiques)

```sql
SELECT e.nom AS eleve, COUNT(ec.id_club) AS nb_clubs
FROM eleves e
LEFT JOIN eleves_clubs ec ON e.id_eleve = ec.id_eleve
GROUP BY e.nom
ORDER BY nb_clubs DESC, e.nom;
```

### 4. Nombre d’élèves par club (statistiques)

```sql
SELECT c.nom AS club, COUNT(ec.id_eleve) AS nb_eleves
FROM clubs c
LEFT JOIN eleves_clubs ec ON c.id_club = ec.id_club
GROUP BY c.nom
ORDER BY nb_eleves DESC, c.nom;
```

---

# Série de 10 requêtes SQL (questions)

Voici 10 requêtes dont il faudra **trouver le résultat** à partir des données précédentes. Pour chaque requête, demandez à vos élèves de donner le résultat (tableau attendu).

---

## 1. Tous les élèves inscrits dans au moins un club

```sql
SELECT DISTINCT e.nom
FROM eleves e
JOIN eleves_clubs ec ON e.id_eleve = ec.id_eleve;
```

## 2. Les clubs où est inscrite "Alice"

```sql
SELECT c.nom
FROM clubs c
JOIN eleves_clubs ec ON c.id_club = ec.id_club
JOIN eleves e ON ec.id_eleve = e.id_eleve
WHERE e.nom = 'Alice';
```

## 3. Les élèves inscrits à "Club Échecs"

```sql
SELECT e.nom
FROM eleves e
JOIN eleves_clubs ec ON e.id_eleve = ec.id_eleve
JOIN clubs c ON ec.id_club = c.id_club
WHERE c.nom = 'Club Échecs';
```

## 4. Nombre de clubs pour chaque élève (par ordre décroissant)

```sql
SELECT e.nom, COUNT(ec.id_club) AS nb_clubs
FROM eleves e
LEFT JOIN eleves_clubs ec ON e.id_eleve = ec.id_eleve
GROUP BY e.nom
ORDER BY nb_clubs DESC, e.nom;
```

## 5. Nombre d’élèves par club (par ordre décroissant)

```sql
SELECT c.nom, COUNT(ec.id_eleve) AS nb_eleves
FROM clubs c
LEFT JOIN eleves_clubs ec ON c.id_club = ec.id_club
GROUP BY c.nom
ORDER BY nb_eleves DESC, c.nom;
```

## 6. Les élèves inscrits à au moins deux clubs

```sql
SELECT e.nom
FROM eleves e
JOIN eleves_clubs ec ON e.id_eleve = ec.id_eleve
GROUP BY e.nom
HAVING COUNT(ec.id_club) >= 2;
```

## 7. Les clubs auxquels aucun élève n’est inscrit

```sql
SELECT c.nom
FROM clubs c
LEFT JOIN eleves_clubs ec ON c.id_club = ec.id_club
WHERE ec.id_eleve IS NULL;
```

## 8. Les élèves qui ne sont inscrits dans aucun club

```sql
SELECT e.nom
FROM eleves e
LEFT JOIN eleves_clubs ec ON e.id_eleve = ec.id_eleve
WHERE ec.id_club IS NULL;
```

## 9. Pour chaque club, la liste des élèves inscrits (triés par club puis par nom d’élève)

```sql
SELECT c.nom AS club, e.nom AS eleve
FROM clubs c
JOIN eleves_clubs ec ON c.id_club = ec.id_club
JOIN eleves e ON ec.id_eleve = e.id_eleve
ORDER BY c.nom, e.nom;
```

## 10. Les élèves inscrits dans les mêmes clubs que "David" (hors David lui-même, résultat sans doublons)

```sql
SELECT DISTINCT e2.nom
FROM eleves_clubs ec1
JOIN eleves_clubs ec2 ON ec1.id_club = ec2.id_club
JOIN eleves e1 ON ec1.id_eleve = e1.id_eleve
JOIN eleves e2 ON ec2.id_eleve = e2.id_eleve
WHERE e1.nom = 'David' AND e2.nom <> 'David';
```


