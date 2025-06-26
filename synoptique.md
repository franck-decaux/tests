```mermaid
flowchart LR
    TempCapteur["Capteur Température"] --> Microbit["Micro:bit"]
    Microbit --> Raspberry["Raspberry Pi"]
    Raspberry --> PageWeb["Page web"]
    Raspberry <--> BaseDeDonnees[("Base de données")]
    Raspberry <-->  Internet(("Internet"))
```
