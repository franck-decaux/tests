# Résumé – Utiliser plusieurs Debian avec Docker et un réseau Docker (Konsole/KDE)

## 1. Lancer plusieurs Debian avec Docker
- Il est possible de lancer plusieurs conteneurs Debian en parallèle.
- Docker permet de les connecter sur un réseau privé virtuel.

### Création du réseau
```bash
docker network create mon_reseau
```

### Lancement de chaque conteneur
```bash
docker run -it --name deb1 --network mon_reseau debian
```
```bash
docker run -it --name deb2 --network mon_reseau debian
```

## 2. Communication entre conteneurs
- Installer `iputils-ping` dans chaque conteneur :
  ```bash
  apt update && apt install -y iputils-ping
  ```
- Les conteneurs peuvent se « pinguer » par leur nom Docker :
  ```bash
  ping deb2
  ```

## 3. Avec Docker Compose
- Automatisation possible avec un `docker-compose.yml` :

```yaml
version: '3.8'

services:
  deb1:
    image: debian:latest
    container_name: deb1
    command: ["bash", "-c", "apt update && apt install -y iputils-ping && bash"]
    networks:
      - mon_reseau

  deb2:
    image: debian:latest
    container_name: deb2
    command: ["bash", "-c", "apt update && apt install -y iputils-ping && bash"]
    networks:
      - mon_reseau

networks:
  mon_reseau:
    driver: bridge
```

- Lancement :
  ```bash
  docker compose up -d
  ```

## 4. Accès à une console dans chaque conteneur
- Ouvrir une console interactive dans chaque Debian :
  ```bash
  docker exec -it deb1 bash
  docker exec -it deb2 bash
  ```
- Possible d’ouvrir plusieurs terminaux pour plusieurs consoles simultanées.

## 5. Automatiser l’ouverture de consoles (exemple Konsole, KDE)

Script bash d’exemple :
```bash
#!/bin/bash

konsole -e "docker exec -it deb1 bash" &
konsole -e "docker exec -it deb2 bash" &
```

- Rendez le script exécutable avec `chmod +x open_consoles.sh`.
- Lancez-le avec `./open_consoles.sh`.
- Variante possible pour automatiser N conteneurs :

```bash
#!/bin/bash
for i in {1..5}; do
  konsole -e "docker exec -it deb$i bash" &
done
```

## 6. Autres points
- Docker gère la résolution DNS interne entre conteneurs sur le même réseau personnalisé.
- Il est possible d’étendre ce schéma à plus de conteneurs ou à des services plus complexes.

