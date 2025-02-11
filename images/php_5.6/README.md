#30/01/2025

# Que contient cette image ?
Un serveur web LAMP avec une très vieille version de PHP. La 5.6.40. 
Ce serveur est conçu pour faire tourner les anciennes versions de PHP.

# Lancer la construction de l'image dockerfile :
Si l'image n'est pas construire, il faut le faire.
Ouvrir un terminal au niveau du dossier qui contient ton ficher dockerfile.yml
  $docker build -t debian_php:5.6
  
On a ajouté un tag pour nommer notre container avec notre version de php.

# Lancer et entrer dans le container :
En une cmde :
  $docker run -it --rm -p 8056:80 debian_php:5.6
  
Ou en plusieurs commandes, comme suit:

## Lancer le container :
  $docker run -it -p 8056:80 debian_php:5.6

Note : Je n'arrive pas à le lancer en detached.
  
## Entrer dans le container debian_PHP
  $docker exec -it debian_php:5.6 /bin/bash

# Lancer ensuite le serveur apache, depuis ton container debian_PHP :
  $service apache2 start

# Vérifier dans un navigateur de la machine hôte que ça fonction sur le port choisi :
  http://localhost:8056/

ou depuis la console du container debian_PHP : 
  $curl http://localhost:80
  

## NOTES sur les bonnes pratiques docker
J'ai décidé d'appliquer les bonnes pratiques de sécurité et d'optimisation de mes images.
J'ai donc réduis les images de bases, fixé les numéros de version,... et je pourrais continuer à améliorer mes images en ce sens.
Les bonnes pratiques sont expliquées dans la documentation docker : 
https://www.docker.com/blog/intro-guide-to-dockerfile-best-practices/
ou dans ce cours d'openClassRooms :
https://openclassrooms.com/fr/courses/8431896-optimisez-votre-deploiement-en-creant-des-conteneurs-avec-docker/8484673-securisez-votre-deploiement
