# Exemples de diagrammes UML avec Mermaid

Ce fichier montre plusieurs types de diagrammes UML créés avec le langage **Mermaid**, entièrement compatible avec GitHub.

---

## 🧱 Diagramme de classes UML

```mermaid
classDiagram
    class Personne {
        -String nom
        -int age
        +parler()
        +marcher()
    }

    class Professeur {
        +enseigner()
    }

    class Eleve {
        +etudier()
    }

    Personne <|-- Professeur
    Personne <|-- Eleve
```

---

## 📜 Diagramme de séquence UML

```mermaid
sequenceDiagram
    participant Client
    participant Serveur
    participant BDD

    Client->>Serveur: envoyerRequete()
    Serveur->>BDD: select *
    BDD-->>Serveur: résultats
    Serveur-->>Client: afficherRésultats()
```

---

## 🔁 Diagramme d’états UML

```mermaid
stateDiagram-v2
    [*] --> EnCours
    EnCours --> Pause : clicPause()
    Pause --> EnCours : clicReprendre()
    EnCours --> Terminé : clicTerminer()
```

---

## 📦 Diagramme de composants UML (simple)

```mermaid
graph LR
    UI[Interface utilisateur] -->|Requêtes| API
    API -->|Lecture/écriture| BDD[(Base de données)]
    API -->|Stockage| FS[(Système de fichiers)]
```

---

## 🗂️ Diagramme de cas d’utilisation (use case)

```mermaid
%% Pas officiellement UML mais utile et clair
graph TD
    Actor((Utilisateur))
    Actor -->|Interagir| UC1[Se connecter]
    Actor --> UC2[Consulter son profil]
    UC2 --> UC3[Modifier profil]
```

---

💡 **Astuce GitHub** : copiez simplement ces blocs dans un `README.md` de dépôt GitHub — les diagrammes seront automatiquement affichés.

👉 Pour plus d'infos : [https://mermaid.js.org/](https://mermaid.js.org/)
