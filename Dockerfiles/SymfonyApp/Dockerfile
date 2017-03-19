FROM ubuntu:16.10
MAINTAINER Michał Zemke
ARG DEBIAN_FRONTEND=noninteractive
RUN      apt-get update
RUN      apt-get install apache2 -y
RUN apt-get install sudo -y
RUN apt-get install git -y
RUN apt-get install curl -y
RUN apt-get install -y  libapache2-mod-php7.0 php7.0-cli php7.0-common php7.0-curl php7.0-gd php7.0-mysql php7.0-sqlite php7.0-xml php7.0-zip php7.0-mbstring php7.0-mcrypt php7.0-intl php7.0-soap php7.0-imap php-imagick 

#apache modification for php
RUN sed -i -e 's/max_execution_time = 30/max_execution_time = 360/g' /etc/php/7.0/apache2/php.ini && \
    sed -i -e 's/upload_max_filesize = 2M/upload_max_filesize = 50M/g' /etc/php/7.0/apache2/php.ini && \
    sed -i -e 's/DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm/DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm/g' /etc/apache2/mods-available/dir.conf 

#composer installation
WORKDIR /tmp
RUN \
  curl -sS https://getcomposer.org/installer | php && \
  mv composer.phar /usr/local/bin/composer && \
  chmod a+x /usr/local/bin/composer && \
  cd / && \
  rm -rf /tmp/composer 
#symfony app installation
RUN mkdir -p /usr/local/bin
RUN curl -LsS https://symfony.com/installer -o /usr/local/bin/symfony
RUN chmod a+x /usr/local/bin/symfony
WORKDIR /var/www/html
RUN symfony demo
RUN chmod 777 -R /var/www/html/symfony_demo/var


ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
EXPOSE 80 
