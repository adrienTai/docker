31/01/2025

# Que contient cette image ?
Un serveur web Apache allégé + les services dont j'aurais besoin.
On part d'une image php-fpm alpine, plusieurs versions de php, apache, mysql.


# Construire l'image dockerfile :
Ouvrir un terminal au niveau du dossier qui contient ton ficher docker-compose.yml

  $docker compose up -d
  
La première execution va lancer un docker build, donc la construction de l'image.

# Ajouter un tag :
Si besoin de le renommer, un ajoute un tag

# Lancer le container :
Avec docker-compose
  $docker compose up -d
  
# Entrer dans le container debian_PHP
Avec docker-compose :
  $docker exec -it docker-debian_php-1 /bin/bash 

# Lancer ensuite le serveur apache, depuis ton container debian_PHP :
  $service apache2 start

# Vérifier dans un navigateur de la machine hôte que ça fonctionne sur le port choisi :
  http://localhost:8080/

ou depuis la console du container debian_PHP : 
  $curl http://localhost:80
  
  Enjoy ! ;)
