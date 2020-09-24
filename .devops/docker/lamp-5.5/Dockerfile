##
## Name: docker-lamp-5.5
## Dockerfile to build a LAMP (PHP 5.5) and Node stack image
## Author: https://about.me/henrycv
##
FROM linode/lamp

MAINTAINER Henry Carbajal <henry@apptitude.com.ar>

# Setted fix to issue "debconf: unable to initialize frontend: Dialog"
# http://askubuntu.com/questions/506158/unable-to-initialize-frontend-dialog-when-using-ssh
ENV DEBIAN_FRONTEND noninteractive

# Update sources and install packages
RUN apt-get -y update && apt-get -y install \
    # Install Git CSV
    git \
    # Install CURL tool
    curl \
    # Install PHP XSL
    php5-xsl \
    # Install PHP XMLRPC
    php5-xmlrpc \
    # Install PHP SNMP
    php5-snmp \
    # Install PSPELL
    php5-pspell \
    # Install PDO SQLITE
    php5-sqlite \
    # Install PHP MEMCACHE
    php5-memcache \
    # Install PHP GD
    php5-gd  \
    # Install PHP APC
    php-apc \
    # Install PHP CURL
    php5-curl \
    # Install PHP Mcrypt
    php5-mcrypt \
    # Install MySQL extension
    php5-mysqlnd \
    # Install PHP XDebug
    php5-xdebug \
    # Install PHP MS SQL driver
    php5-sybase \
    # --no-install-recommends \
    && rm -r /var/lib/apt/lists/* \
    && apt-get purge -y --auto-remove

# Enable Apache mod rewrite
RUN a2enmod rewrite

# Enable module expires.
RUN a2enmod file_cache
RUN a2enmod expires
RUN a2enmod headers
RUN a2enmod ssl

# # Enable PHP Mcrypt extension
RUN php5enmod mcrypt

# Enable a virtualhost "test" for testing
RUN a2dissite example.com.conf
RUN a2ensite 000-default.conf
COPY ./docker/lamp-5.5/config/sites-available/* /etc/apache2/sites-available/
# RUN service apache2 start
# RUN service apache2 reload
RUN rm -rf /etc/apache2/sites-available/example.com.conf

# Activate the virtualhosts
#   docker exec docker-lamp-5.5 service apache2 start && service apache2 reload && service apache2 restart

# Copy the customized Apache config on guest machine:
COPY ./docker/lamp-5.5/config/apache2.conf /etc/apache2/apache2.conf
COPY ./docker/lamp-5.5/config/ports.conf /etc/apache2/ports.conf

# Copy the customized PHP development config on guest machine:
COPY ./docker/lamp-5.5/config/php.ini /etc/php5/apache2/php.ini

# Add "localhost" support
RUN echo "" >> /etc/apache2/apache2.conf
RUN echo "# Enable localhost server name" >> /etc/apache2/apache2.conf
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Create the folder if it doesn't exits
RUN [ -d /usr/local/bin/ ] || mkdir -p /usr/local/bin/

# Install PHP Composer globally
RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

EXPOSE 8080 3306 3307

ADD ./docker/lamp-5.5/start.sh /start.sh

RUN chmod 777 start.sh
CMD ["/bin/bash", "-lc", "/start.sh"]
