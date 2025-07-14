# docker_exam

## Docker – Commandes de base

Affichage de la version de Docker
```  
docker --version
```

Infos système Docker
```  
docker info
```

Aide générale
```  
docker help
```

# Docker Container

Lister les conteneurs en cours d’exécution
```  
docker ps
```

Lister tous les conteneurs (même arrêtés)
```  
docker ps -a
```

Lancer un conteneur interactif basé sur Ubuntu
```  
docker run -it ubuntu bash
```

Lancer un conteneur nginx en arrière-plan
```  
docker run -d --name mon_nginx nginx
```

Arrêter le conteneur mon_nginx
```  
docker stop mon_nginx
```

Redémarrer le conteneur mon_nginx
```  
docker restart mon_nginx
```

Afficher les logs du conteneur mon_nginx
```  
docker logs mon_nginx
```

Exécuter une commande dans le conteneur mon_nginx
```  
docker exec -it mon_nginx bash
```

Supprimer le conteneur mon_nginx
```  
docker rm mon_nginx
```

# Docker Image

Lister les images locales
```  
docker images
```

Télécharger l’image nginx
```  
docker pull nginx
```

Construire une image avec le nom monimage
```  
docker build -t monimage .
```

Taguer l’image pour un repository fictif Docker Hub
```  
docker tag monimage monutilisateur/monimage:latest
```

Pousser l’image vers un registry
```  
docker push monutilisateur/monimage:latest
```

Supprimer une image
```  
docker rmi 123456789abc
123456789abc est image_id
```

# Dockerfile

Créer le fichier index.js
```
nano index.js
```
```
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/', (req, res) => {
  res.send('Hello Docker!');
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

Créer le Dockerfile
```
nano Dockerfile
```
```
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

Construire l’image Docker
```
docker build -t hello-docker .
```

Exécuter le conteneur
```
docker run -p 3000:3000 hello-docker
```

Tester
```
curl http://localhost:3000
```

# Docker Compose

Démarrer les services
```
docker-compose up
```

Démarrer en arrière-plan
```
docker-compose up -d
```

Arrêter et supprimer les conteneurs
```
docker-compose down
```

Construire les images
```
docker-compose build
```

Afficher les logs
```
docker-compose logs
```

Affiche l'état des services
```
docker-compose ps
```

```
nano php_app/index.php
```
```
<?php
echo "<h1>Bienvenue sur mon application PHP via Docker Compose !</h1>";
echo "<p>Le serveur PHP fonctionne sur le port 3000.</p>";
?>
```
```
nano docker-compose.yml
```
```
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
```
```
nano nginx.conf
```
```
server {
    listen 80;

    location / {
        proxy_pass http://app:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

# Docker Volume

Créer un volume
```
docker volume create monvolume
```

Lister les volumes
```
docker volume ls
```

Afficher les détails du volume
```
docker volume inspect monvolume
```

Supprimer un volume
```
docker volume rm monvolume
```

Utilisation dans docker run
```
docker run -v monvolume:/data nginx
```

# Docker Swarm

Initier un Swarm
```
docker swarm init
```

Token pour ajouter un worker
```
docker swarm join-token worker
```

Lister les noeuds du Swarm
```
docker node ls
```

Créer un service
```
docker service create --replicas 3 --name web nginx
```

Lister les services
```
docker service ls
```

Tâches du service
```
docker service ps web
```

Déployer une stack
```
docker stack deploy -c docker-compose.yml mystack
```

Supprime une stack
```
docker stack rm mystack
```

# Docker Network

Lister les réseaux existants
```
docker network ls
```

Créer un réseau bridge personnalisé
```
docker network create \
  --driver bridge \
  mon_reseau_bridge
```

Vérification
```
docker network inspect mon_reseau_bridge
```

Créer deux conteneurs connectés au réseau
```
docker run -dit --name conteneur1 --network mon_reseau_bridge alpine sh
docker run -dit --name conteneur2 --network mon_reseau_bridge alpine sh
```

Tester la communication entre les deux conteneurs
```
docker exec -it conteneur1 ping conteneur2
```

Créer un réseau overlay (pour Docker Swarm)
```
docker swarm init
docker network create \
  --driver overlay \
  --attachable \
  mon_reseau_overlay
```

Connecter manuellement un conteneur à un réseau
```
docker network connect mon_reseau_bridge conteneur1
```

Pour le déconnecter
```
docker network disconnect mon_reseau_bridge conteneur1
```

Nettoyage
```
docker container rm -f conteneur1 conteneur2
docker network rm mon_reseau_bridge mon_reseau_overlay
```

# Nettoyage Docker

Supprimer les objets inutilisés
```
docker system prune
```

Supprimer les volumes inutilisés
```
docker volume prune
```

Supprimer les images non utilisées
```
docker image prune
```