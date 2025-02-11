# Here is a docker image 346MB, using debian, apache, and php.
# This is the only way I found to use the ppa:ondrej/php repository,
# a repository I needed to install older php versions.
# For newest php versions, we could directly use a php-fpm image, which is lighter

# We choose a slim version debian, and we specify the actual last debian version, for stability and security reason
# We could use alpine distribution to lighten even more the image
FROM debian:bookworm-slim

RUN mkdir /var/www  \
    && chmod +x /var/www

# We update repositories before installation, for security
RUN apt-get update -y && apt-get upgrade -y \

# Install web environment and tools for the machine : apache2, php, curl, vim
  && apt-get install -y apache2 vim curl \
# Here we install a heavy pack 'lsb-release' to access the repository ppa:ondrej/php on a docker image
# this repo contains older php versions
  && apt-get install -y software-properties-common apt-transport-https lsb-release ca-certificates \
  && curl -sSL https://packages.sury.org/php/README.txt | bash -x \
  && apt-get update -y \
  && apt-get install -y php5.6 php5.6-cli \
  && apt-get install -y libapache2-mod-php5.6 php5.6-curl php5.6-mbstring php5.6-zip php5.6-xml php5.6-soap php5.6-cli \
     php5.6-mysql php5.6-xml php5.6-intl php5.6-opcache \
  && echo 2 | update-alternatives --config php \
  && apt-get remove -y --purge php8.3 php8.3-cli php8.3-common \
# Clean cache to reduce the docker image size
  && apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/*

# Configuration of Apache
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

EXPOSE 80

# After running this image, we launch apache2 server
CMD ["apache2ctl", "-D", "FOREGROUND"]
# This line is equivalent to :
# $docker exec -it YOUR_CONTAINER_NAME /bin/bash
# root@CONTAINER_ID: $service apache2 restart


LABEL description="Environnement de developpement web avec PHP 5.6.40"
