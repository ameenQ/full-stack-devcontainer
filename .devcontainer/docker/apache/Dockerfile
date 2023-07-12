## Starting from Ubuntu
FROM php:8.1-apache

## Create a Non-Root User
RUN apt-get update && apt-get -y install sudo && apt-get install wget -y && apt-get install curl -y
RUN useradd -m developer && echo "www-data:www-data" | chpasswd && adduser developer sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers


## Install Node
RUN sudo curl -sL https://deb.nodesource.com/setup_18.x -o nodesource_setup.sh
RUN sudo bash nodesource_setup.sh
RUN sudo apt install nodejs -y
RUN sudo npm install -g @angular/cli


## Install Git
RUN sudo apt install git -y


## Install Ruby (Will install 2.7)
# RUN sudo apt install ruby-full -y

## Install Python3
# RUN sudo apt-get install python3 python3-dev -y

## Install Java 17
# RUN sudo wget https://download.java.net/java/GA/jdk17/0d483333a00540d886896bac774ff48b/35/GPL/openjdk-17_linux-x64_bin.tar.gz
# RUN sudo tar xvf openjdk-17_linux-x64_bin.tar.gz
# RUN sudo mv jdk-17 /opt/
# RUN echo 'JAVA_HOME=/opt/jdk-17' >> ~/.bashrc
# RUN echo 'PATH=$PATH:$JAVA_HOME/bin' >> ~/.bashrc
# RUN . ~/.bashrc


RUN  docker-php-ext-install pdo pdo_mysql mysqli
RUN sudo apt-get install -y \
        libzip-dev \
        zip \
  &&  docker-php-ext-install zip
RUN sudo apt-get install -y autoconf pkg-config libssl-dev
RUN sudo pecl install mongodb
RUN sudo echo "extension=mongodb.so" > $PHP_INI_DIR/conf.d/mongo.ini



## Installing Composer
RUN curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
RUN sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer


## Insall Coursier
# RUN curl -fL https://github.com/coursier/launchers/raw/master/cs-x86_64-pc-linux.gz | gzip -d > cs
# RUN sudo chmod +x cs 
# RUN ./cs setup -y
# RUN ./cs install scala3
# RUN ./cs install scala3-compiler
# RUN echo 'PATH=$PATH:~/.local/share/coursier/bin' >> ~/.bashrc
# RUN . ~/.bashrc



RUN sudo apt-get install -y locate
RUN updatedb


## Install Nano + VIM for editing
RUN sudo apt-get install nano vim -y

RUN rm -f /etc/apache2/sites-available/000-default.conf
ADD ./000-default.conf /etc/apache2/sites-available
# RUN ln -s /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-enabled/000-default.conf

RUN sudo mkdir /etc/apache2/ssl/

RUN sudo openssl req -new -newkey rsa:4096 -days 3650 -nodes -x509 -subj "/C=US/ST=Example/L=City/O=Example Inc/CN=kubernetes.docker.internal"  -keyout /etc/apache2/ssl/ssl.key -out /etc/apache2/ssl/ssl.crt
RUN sudo chmod 777 /etc/apache2/ssl/ssl.key
RUN sudo chmod 777 /etc/apache2/ssl/ssl.crt

RUN sudo a2enmod rewrite
RUN sudo a2enmod ssl

EXPOSE 443


## Start Container
# ENTRYPOINT bash
USER developer
WORKDIR /var/www/html/

ARG gitusername
ARG gitemail
ENV gitun=$gitusername
ENV gitemail=$gitemail
RUN git config --global user.name "$gitun"
RUN git config --global user.email "$gitemail"

RUN composer global require laravel/installer
RUN export PATH=$PATH:$HOME/.composer/vendor/bin

## Install Deno
# RUN curl -fsSL https://deno.land/install.sh | sh
# RUN echo 'DENO_INSTALL="/home/developer/.deno"' >> ~/.bashrc
# RUN echo 'PATH="$DENO_INSTALL/bin:$PATH"' >> ~/.bashrc