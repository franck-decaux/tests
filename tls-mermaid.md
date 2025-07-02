# Schéma de fonctionnement de SSL/TLS

Ce diagramme montre comment s’établit une connexion sécurisée entre un navigateur et un serveur à l’aide de SSL/TLS :

```mermaid
sequenceDiagram
    participant Navigateur
    participant Serveur

    Navigateur->>Serveur: 1. Client Hello\n(Négociation des options)
    Serveur-->>Navigateur: 2. Server Hello + Certificat\n(Authentification du serveur)
    Navigateur->>Serveur: 3. Envoi clé de session chiffrée\n(Génération clé symétrique)
    Serveur-->>Navigateur: 4. Activation du chiffrement
    Navigateur<-->Serveur: 5. Échange sécurisé\n(Données chiffrées)