Image PHP FPM version 7.3.33

J'ai choisi PHP-FPM pour son côté optimisé. 
J'ai choisi la version 7.3 pour un site legacy.

Je pars d'une image allégée, alpine, en précisant le Sha de l'image, pour plus de sécurité, au cas ou le pointeur de version soit déplacé.
J'installe le minimum dont j'ai besoin pour faire tourner PHP et ses extensions. Là aussi pour des raisons de sécurité, et de poids.

Je me suis mis la contrainte de changer le port sur lequel communique php.
J'ai donc mis le port 80 au lieu de 9000.
Ce qui m'a fait modifier mon fichier de configuration apache, dans mon vhost, je dirige les fichiers php vers cette image, et le port 80.
Regardez dans docker/apache_fpm/apache_conf/httpd-vhosts.conf

J'ai changé l'utilisateur 'www-data' pour 'serve', qui sera le même sur mon serveur apache.

Mon fichier de configuration (php.ini) est /usr/local/etc/php-fpm.d/www.conf
Mes logs sont dans /var/log/php/fpm-php.www.log

