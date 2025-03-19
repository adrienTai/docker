# We choose a light version of the lastest apache (httpd), on a light alpine linux
# We use a pinned image version (@sha), for a garantee to get always the same build
FROM httpd:2.4.63-alpine3.21@sha256:4aec2953509e2d3aa5a8d73c580a381be44803fd2481875b15d9ad7d2810d7ca AS httpd_serve

ARG user
ARG uid

# Install tools for linux machine basic operations
RUN apk add --no-cache curl vim mysql-client msmtp libzip \

# Clean cache to reduce the docker image size
    && rm -rf /var/cache/apk/*

# Apache configuration for running PHP in FPM 
RUN echo "ServerName localhost" >> /usr/local/apache2/conf/httpd.conf && \
    echo "LoadModule proxy_module modules/mod_proxy.so" >> /usr/local/apache2/conf/httpd.conf && \ 
    echo "LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so" >> /usr/local/apache2/conf/httpd.conf
    # echo "Include /etc/apache2/conf.d/*.conf" >> /usr/local/apache2/conf/httpd.conf

# Additionnal configuration can be add for Vhost management, in an external file : apache_conf/httpd-vhosts.conf
COPY apache_conf/httpd-vhosts.conf /usr/local/apache2/conf/extra/httpd-vhosts.conf
RUN sed -i -e 's/#Include\ conf\/extra\/httpd-vhosts.conf/Include\ conf\/extra\/httpd-vhosts.conf/' \
    /usr/local/apache2/conf/httpd.conf

    
# Create non-root users, folders and update permissions
RUN addgroup -g $uid $user \
    && adduser -D -H -h /var/www -s /sbin/nologin -G $user -u $uid $user \
    && mkdir -p /var/www /usr/local/apache2/logs \
    && chown -R $user:$user /var/www /usr/local/apache2/logs
    
# Change owner and group
USER $user:$user

WORKDIR /var/www

EXPOSE 80

# Start Apache
CMD ["httpd-foreground"]

LABEL description="Serveur Apache2"


# If apache doesn't work, try to go into the container to restart the apache service :
# docker exec -it httpd_serve /bin/sh
# $ service restart apache2
