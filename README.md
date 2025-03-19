30/11/2024 - maj 11/02/2025

# Environnements de développement WEB #

## Que contient ce dépôt ? ##
Des images docker, des fichiers docker-compose, et une doc pour l'utilisation de ces images. 
Des serveurs web Apache, plusieurs versions de php, des services de cache, phpmyadmin, base de données,...
Essentiellement des projets PHP et Symfony.

## Quel est le but de ce dépôt ? ##
Disposer de mes images docker, pour travailler à distance. 
Sécuriser mes travaux, au cas où ma machine part au paradis des ordinateurs.
Apprendre à créer des environnements docker :
  * légers
  * sécurisés 
  * normalisés 
  * optimisés en rapidité
  * qui suivent les bonnes pratiques
  * et simples d'utilisation.

Pour finir, partager ces environnements, pour quiconque en aura besoin (licence GNU).


## Quel est le but de ces images ? ##
M'entrainer à utiliser docker.
Travailler sur des projets PHP, symfony.
Faire tourner mes anciens sites internet.
Permettre une migration d'un projet PHP, pas à pas.

## Qu'est-ce qui viendra ensuite ? ##
Des serveurs utilisant Nginx, Nginx + Apache, Nodes.js. (voir Annexe 1)
Des outils de logs, de scan du code, de test.
Peut-être d'autres applications des pratiques devOps.
Des README en anglais.

## Pré-recquis ##
Savoir utiliser des commandes linux, connaître les bases des commandes docker.
Enjoy ;)



# Annexe 1 - Apache ou Nginx ? #
Pour un environnement Web avec php, Apache ou Nginx couvrent la majorité des sites internet. 

La différence principale que je note, c'est qu' apache (httpd) agit plus rapidement pour du contenu dynamique, car il inclut php.
Il est adapté aux hébergements mutualisés, il offre un accès root pour modifier le fichier de configuration principal.
Il contient plein de modules faciles à installer.

En revanche, Nginx est nettement plus rapide pour les fichiers statiques, grâce à son système de cache, et son architecture asynchrone.
Nginx, agit plus lentement pour du contenu dynamique, car il doit transmettre les requêtes php à un processus externe, php-fpm.
Il a aussi des modules, mais l'installation est plus compliquée.

Nginx semble être le meilleur choix pour des e-commerces.
Nginx sera le plus utilisé pour servir à des milliers de connexions simultanée, à condition que le contenu soit statique.
J'insiste, il faut du contenu statique, donc un contenu qui ne change pas en fonction de l'utilisateur.
Ca veut dire des pages html, générées en amont, comme on le fait avec symfony.
Il faudra créer un fichier de configuration qui remplace les .htaccess.

L'implémentation d'outil marketing comme les AB tests, qui requiert via JS la création de contenu changeant aléatoirement, va nécessairement ralentir le site. Mais aussi pour les outils qui requièrent un traitement php. 
Ce sera donc à éviter sur les pages les plus visitées du site.

La pratique courante pour utiliser les deux services est de placer NGINX comme reverse proxy devant Apache. En tant que proxy frontal pour Apache, NGINX traitera toutes les demandes des clients.
S’il reçoit une demande de contenu statique, NGINX servira les fichiers directement au client.
Pour le contenu dynamique, NGINX transmet la demande à Apache, qui la traite et transfère le contenu final au client via NGINX.


### Conclusion de l'Annexe 1 ###  

Pour conclure, je vais construire une image apache, une image nginx, une image qui utilise les 2, et des images PHP dans des services séparés.
Apache sera pratique car plus modulable, pour simuler des configurations un peu dépassées.
Nginx sera nécessaire, car plus adapté à un fort taux de connexion.
Et Node.js ! Car j'ai prévu de tester des applications JS.
Dans la réalité du monde web, il faut être adaptable.

Source :
https://www.hostinger.fr/tutoriels/apache-vs-nginx#NGINX_vs_Apache_%E2%80%93_Apercu_general
