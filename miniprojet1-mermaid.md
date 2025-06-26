# Mesure de température

## Synoptique

```mermaid
flowchart LR
    TempCapteur["Capteur Température"] --> Microbit["Micro:bit"]
    Microbit --> Raspberry["Raspberry Pi"]
    Raspberry --> PageWeb["Page web"]
    Raspberry <--> BaseDeDonnees[("Base de données")]
    Raspberry <-->  Internet(("Internet"))
```

### Ce document présente, à l'aide de diagrammes UML Mermaid, l'architecture d'un projet :

- Mesure de température par capteur analogique
- Transmission des données à une micro:bit
- Transmission série vers une Raspberry Pi
- Stockage dans une base SQLite avec Python
- Génération d'une page web à partir des données

---

## Diagramme de classes <a id="classes"></a>

```mermaid
classDiagram
    class CapteurAnalogique {
        +float lireValeur()
    }
    class Microbit {
        +float convertirAnalogique(float valeur)
        +void transmettre(float valeur)
    }
    class RaspberryPi {
        +void recevoirSerie()
        +void insererBDD(float temperature)
    }
    class BaseDonnees {
        +void inserer(float temperature, datetime timestamp)
        +float[] lireDonnees()
    }
    class ScriptPython {
        +void lireSerie()
        +void stockerBDD(float temperature)
    }
    class ScriptWeb {
        +void genererHTML(float[] temperatures)
    }

    CapteurAnalogique -- Microbit : mesure
    Microbit -- RaspberryPi : transmission série
    RaspberryPi -- BaseDonnees : écrit/insère
    ScriptPython .. RaspberryPi : s'exécute sur
    ScriptWeb -- BaseDonnees : lit
```

---

## Diagramme de séquence <a id="sequence"></a>

```mermaid
sequenceDiagram
    participant Capteur as CapteurAnalogique
    participant MB as Microbit
    participant RPi as RaspberryPi
    participant BDD as BaseDonnees
    participant ScriptWeb

    Capteur->>MB: lireValeur()
    MB->>MB: convertirAnalogique(valeur)
    MB->>RPi: transmettre(valeur)
    RPi->>BDD: insererBDD(valeur, timestamp)

    Note over RPi,BDD: Stockage périodique dans la BDD

    ScriptWeb->>BDD: lireDonnees()
    ScriptWeb->>ScriptWeb: genererHTML(données)
```
---

## Diagramme d'activités <a id="activites"></a>

```mermaid
flowchart TD
    A[Début] --> B[Lire température depuis capteur]
    B --> C[Convertir valeur analogique]
    C --> D[Transmettre valeur à la Raspberry Pi]
    D --> E[Insérer dans la base de données]
    E --> F[Attendre prochain relevé]
    F --> B
    E --> G[Générer page web]
    G --> H[Afficher au visiteur]
```


