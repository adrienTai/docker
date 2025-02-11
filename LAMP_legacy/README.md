31/01/2025

# Que contient ce docker compose ?
Un serveur web LAMP. Linux Debian dernière version (bookworm), serveur apache, plusieurs versions de PHP, et MySQL.
Ce serveur est conçu pour faire tourner les anciennes versions de PHP.
Il sert à faire tourner les anciennes applications en l'état, avant d'opérer leur migration.

# Construire l'image dockerfile :
Ouvrir un terminal au niveau du dossier qui contient ton ficher docker-compose.yml

  $docker compose up
  
La première execution va lancer un docker build, donc la construction de l'image.

# Ajouter un tag :
Si besoin de le renommer, un ajoute un tag.

# Lancer le container :
Avec docker-compose
  $docker compose up -d

  -d pour le mode detached.
  
# Entrer dans le container lamp_legacy
Avec docker-compose :
  $docker exec -it lamp_legacy-php73-1 /bin/bash 

# Lancer ensuite le serveur apache, depuis ton container lamp_legacy :
  $service apache2 start
  ou 
  $service apache2 restart

# Vérifier dans un navigateur de la machine hôte que ça fonctionne sur le port choisi :
  http://localhost:8073/
  ou 
  http://localhost:8056/ pour php 5.6

ou depuis la console du container LAMP_legacy : 
  $curl http://localhost:80
  
  Enjoy ! ;)
