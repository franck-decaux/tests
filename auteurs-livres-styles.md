# Exemple de base de données relationnelle : Livres, Auteurs, Styles

## Présentation

Cet exemple présente une base de données relationnelle simple sur le thème des livres, des auteurs et des styles littéraires. On y trouve trois tables principales :

- `auteurs` : répertorie les auteurs,

- `styles` : répertorie les styles littéraires,

- `livres` : contient les livres, chacun relié à un auteur et à un style.

La table `livres` possède deux clés étrangères permettant de relier chaque livre à son auteur et à son style.

---

## SQL de création des tables

```sql
CREATE TABLE auteurs (
    id_auteur INTEGER PRIMARY KEY AUTOINCREMENT,
    nom TEXT NOT NULL,
    prenom TEXT
);

CREATE TABLE styles (
    id_style INTEGER PRIMARY KEY AUTOINCREMENT,
    libelle TEXT NOT NULL
);

CREATE TABLE livres (
    id_livre INTEGER PRIMARY KEY AUTOINCREMENT,
    titre TEXT NOT NULL,
    annee INTEGER,
    id_auteur INTEGER NOT NULL,
    id_style INTEGER NOT NULL,
    FOREIGN KEY (id_auteur) REFERENCES auteurs(id_auteur),
    FOREIGN KEY (id_style) REFERENCES styles(id_style)
);
```

---

## SQL d'insertion des données

```sql
-- Insertion des auteurs
INSERT INTO auteurs (nom, prenom) VALUES ('Zola', 'Émile');
INSERT INTO auteurs (nom, prenom) VALUES ('Hugo', 'Victor');
INSERT INTO auteurs (nom, prenom) VALUES ('Camus', 'Albert');

-- Insertion des styles
INSERT INTO styles (libelle) VALUES ('Roman');
INSERT INTO styles (libelle) VALUES ('Poésie');
INSERT INTO styles (libelle) VALUES ('Théâtre');

-- Insertion des livres
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('Germinal', 1885, 1, 1);
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('L’Assommoir', 1877, 1, 1);
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('Les Misérables', 1862, 2, 1);
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('Notre-Dame de Paris', 1831, 2, 1);
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('Les Contemplations', 1856, 2, 2);
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('Le Dernier Jour d’un Condamné', 1829, 2, 1);
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('L’Étranger', 1942, 3, 1);
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('La Peste', 1947, 3, 1);
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('Caligula', 1944, 3, 3);
INSERT INTO livres (titre, annee, id_auteur, id_style) VALUES ('Le Mythe de Sisyphe', 1942, 3, 2);
```

---

### Table `auteurs`

| **🔑 id_auteur** | nom   | prenom |
| ---------------- | ----- | ------ |
| 1                | Zola  | Émile  |
| 2                | Hugo  | Victor |
| 3                | Camus | Albert |

---

### Table `styles`

| **🔑 id_style** | libelle |
| --------------- | ------- |
| 1               | Roman   |
| 2               | Poésie  |
| 3               | Théâtre |

---

### Table `livres`

| **🔑 id_livre** | titre                         | annee | 🗝️ id_auteur | 🗝️ id_style |
| --------------- | ----------------------------- | ----- | ------------- | ------------ |
| 1               | Germinal                      | 1885  | 1             | 1            |
| 2               | L’Assommoir                   | 1877  | 1             | 1            |
| 3               | Les Misérables                | 1862  | 2             | 1            |
| 4               | Notre-Dame de Paris           | 1831  | 2             | 1            |
| 5               | Les Contemplations            | 1856  | 2             | 2            |
| 6               | Le Dernier Jour d’un Condamné | 1829  | 2             | 1            |
| 7               | L’Étranger                    | 1942  | 3             | 1            |
| 8               | La Peste                      | 1947  | 3             | 1            |
| 9               | Caligula                      | 1944  | 3             | 3            |
| 10              | Le Mythe de Sisyphe           | 1942  | 3             | 2            |

---

### Commentaire sur les deux clés étrangères

La table `livres` comporte **deux clés étrangères** :

- `id_auteur` : 🗝️ référence l’identifiant d’un auteur dans la table `auteurs`.  
  Cela permet de savoir quel auteur a écrit chaque livre.

- `id_style` : 🗝️ référence l’identifiant d’un style dans la table `styles`.  
  Cela permet de préciser le style littéraire associé à chaque livre.

**Grâce à ces deux clés étrangères, chaque livre est relié à la fois à son auteur et à son style, assurant la cohérence et la structuration de la base de données.**
