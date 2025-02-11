#30/01/2025

# Que contient cette image ?
Un serveur web LAMP. Linux Debian dernière version (bookworm), serveur apache2, plusieurs versions de PHP, et MySQL.
Ce serveur est conçu pour faire tourner les anciennes versions de PHP.
Il sert à faire tourner les anciennes applications, afin d'opérer des migrations.

Il permet aussi d'utiliser de manière très claire une image debian, à laquelle on ajoute nos différents services.


# Lancer la construction de l'image dockerfile :
Si l'image n'est pas construire, il faut le faire.
Ouvrir un terminal au niveau du dossier qui contient ton ficher dockerfile.yml
  $docker build -t debian_php:7.3.33
  
On a ajouté un tag pour nommer notre container avec notre version de php.

# Lancer et entrer dans le container :
En une cmde :
  $docker run -it --rm -p 8073:80 debian_php:7.3.33
  
Ou en plusieurs commandes, comme suit:

## Lancer le container :
  $docker run -it -p 8073:80 debian_php:7.3.33

Note : Je n'arrive pas à le lancer en detached.
  
## Entrer dans le container debian_PHP
  $docker exec -it debian_php:7.3.33 /bin/bash

# Lancer ensuite le serveur apache, depuis ton container debian_PHP :
  $service apache2 start

# Vérifier dans un navigateur de la machine hôte que ça fonction sur le port choisi :
  http://localhost:8073/

ou depuis la console du container debian_PHP : 
  $curl http://localhost:80
  

## NOTES sur les bonnes pratiques docker
J'ai décidé d'appliquer les bonnes pratiques de sécurité et d'optimisation de mes images.
J'ai donc réduis les images de bases, fixé les numéros de version,... et je pourrais continuer à améliorer mes images en ce sens.
Les bonnes pratiques sont expliquées dans la documentation docker : 
https://www.docker.com/blog/intro-guide-to-dockerfile-best-practices/
ou dans ce cours d'openClassRooms :
https://openclassrooms.com/fr/courses/8431896-optimisez-votre-deploiement-en-creant-des-conteneurs-avec-docker/8484673-securisez-votre-deploiement
