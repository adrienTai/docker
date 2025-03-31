31/01/2025

# Que contient ce docker-compose ?
Un serveur web Apache allégé + les services dont j'aurais besoin.
Le serveur apache2, plusieurs versions de php, mysql, phpmyadmin.


# Que contient l'image apache ?
On part d'une version alpine de httpd, très légère et simpliste.
On ajoute quelques outils de base si j'ai besoin de me connecter à mon container apache.
On ajoute les modules dont j'ai besoin, surtout fastCGI.
On ajoute notre fichier de configuration apache_conf/httpd-vhosts.conf, pour gérer les différents vhost noms de domaine.
On créé un utilisateur "Serve", pour que notre service puisse accéder aux logs.
On passe sous l'utilisateur Serve, et on lance la commande "httpd-foreground", pour démarrer le service apache.
Les logs du serveur sont centralisés sur les mêmes fichiers.
Les privilèges donnés à mon service apache sont restreint, pour plus de sécurité. On évite de donner un accès root à notre service apache, au sein de docker-compose.


# Construire l'image dockerfile :
Ouvrir un terminal au niveau du dossier qui contient ton ficher docker-compose.yml

  $docker compose up -d
  
La première execution va lancer un docker build, donc la construction de l'image.

# Ajouter un tag :
Si besoin de le renommer, un ajoute un tag

# Lancer le container :
Avec docker-compose
  $docker compose up -d
  
# Entrer dans le container httpd_serve
Avec docker-compose :
  $docker exec -it httpd_serve /bin/sh 

# Lire les logs :
  $cat /usr/local/apache2/logs/access.log 

# Lire le fichier de conf :
  $cat /usr/local/apache2/conf/httpd.conf

# Redémarrer le serveur apache :
Quitter le container
  $exit
Relancer le container httpd_serve
  $docker compose restart httpd_serve

# Vérifier dans un navigateur de la machine hôte que ça fonctionne sur le port choisi :
  http://localhost:8080/

ou depuis la console du container httpd_serve : 
  $curl http://localhost:80
  
  Enjoy ! ;)
