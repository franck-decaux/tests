# Exemple de base de données relationnelle : Livres, Auteurs, Styles

## Présentation

Cet exemple présente une base de données relationnelle simple sur le thème des livres, des auteurs et des styles littéraires. On y trouve trois tables principales :

* `auteurs` : répertorie les auteurs,
* `styles` : répertorie les styles littéraires,
* `livres` : contient les livres, chacun relié à un auteur et à un style.

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

| **🔑 id\_auteur** | nom   | prenom |
| ----------------- | ----- | ------ |
| 1                 | Zola  | Émile  |
| 2                 | Hugo  | Victor |
| 3                 | Camus | Albert |

---

### Table `styles`

| **🔑 id\_style** | libelle |
| ---------------- | ------- |
| 1                | Roman   |
| 2                | Poésie  |
| 3                | Théâtre |

---

### Table `livres`

| **🔑 id\_livre** | titre                         | annee | 🗝️ id\_auteur | 🗝️ id\_style |
| ---------------- | ----------------------------- | ----- | -------------- | ------------- |
| 1                | Germinal                      | 1885  | 1              | 1             |
| 2                | L’Assommoir                   | 1877  | 1              | 1             |
| 3                | Les Misérables                | 1862  | 2              | 1             |
| 4                | Notre-Dame de Paris           | 1831  | 2              | 1             |
| 5                | Les Contemplations            | 1856  | 2              | 2             |
| 6                | Le Dernier Jour d’un Condamné | 1829  | 2              | 1             |
| 7                | L’Étranger                    | 1942  | 3              | 1             |
| 8                | La Peste                      | 1947  | 3              | 1             |
| 9                | Caligula                      | 1944  | 3              | 3             |
| 10               | Le Mythe de Sisyphe           | 1942  | 3              | 2             |

---

### Commentaire sur les deux clés étrangères

La table `livres` comporte **deux clés étrangères** :

* `id_auteur` : 🗝️ référence l’identifiant d’un auteur dans la table `auteurs`.
  Cela permet de savoir quel auteur a écrit chaque livre.
* `id_style` : 🗝️ référence l’identifiant d’un style dans la table `styles`.
  Cela permet de préciser le style littéraire associé à chaque livre.

**Grâce à ces deux clés étrangères, chaque livre est relié à la fois à son auteur et à son style, assurant la cohérence et la structuration de la base de données.**

---

## SQL pour afficher les livres avec leur auteur et leur style

```sql
SELECT
    livres.titre,
    livres.annee,
    auteurs.nom AS auteur_nom,
    auteurs.prenom AS auteur_prenom,
    styles.libelle AS style
FROM
    livres
JOIN auteurs ON livres.id_auteur = auteurs.id_auteur
JOIN styles ON livres.id_style = styles.id_style
ORDER BY
    livres.titre;
```

Cette requête affiche pour chaque livre son titre, son année, le nom et le prénom de l’auteur, ainsi que son style littéraire.

### Explication de la double jointure

La requête utilise **deux jointures (JOIN)** pour rassembler les informations provenant des trois tables :

* `JOIN auteurs ON livres.id_auteur = auteurs.id_auteur` relie chaque livre à son auteur grâce à la clé étrangère `id_auteur` dans la table `livres`.
* `JOIN styles ON livres.id_style = styles.id_style` relie chaque livre à son style littéraire grâce à la clé étrangère `id_style` dans la table `livres`.

Ainsi, chaque ligne du résultat regroupe les informations du livre, de son auteur et de son style, permettant d’afficher dans un seul tableau tous les renseignements souhaités.

---

## Résultat obtenu

| titre                         | annee | auteur\_nom | auteur\_prenom | style   |
| ----------------------------- | ----- | ----------- | -------------- | ------- |
| Caligula                      | 1944  | Camus       | Albert         | Théâtre |
| Germinal                      | 1885  | Zola        | Émile          | Roman   |
| L’Assommoir                   | 1877  | Zola        | Émile          | Roman   |
| La Peste                      | 1947  | Camus       | Albert         | Roman   |
| Le Dernier Jour d’un Condamné | 1829  | Hugo        | Victor         | Roman   |
| Le Mythe de Sisyphe           | 1942  | Camus       | Albert         | Poésie  |
| Les Contemplations            | 1856  | Hugo        | Victor         | Poésie  |
| Les Misérables                | 1862  | Hugo        | Victor         | Roman   |
| L’Étranger                    | 1942  | Camus       | Albert         | Roman   |
| Notre-Dame de Paris           | 1831  | Hugo        | Victor         | Roman   |
