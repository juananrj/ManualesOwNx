# Instalacion de owncloud

En este manual veremos los pasos a seguir para hacer un servidor de ficheros owncloud en un contendor lxd.

## Primero de todo debemos crear un contenedor con ubuntu.
~~~
* lxc launch ubuntu:20.04 "nombre del contenedor"
~~~
## Si esta apagado lo encendemos.
~~~
* lxc start "nombre del contenedor"
~~~
## Despues de hacer esto lo ejecutamos.
~~~
* lxc exec "micontenedor" bash
~~~
## Ahora para instalar owncloud debemos instalar apache2, mysql y algunas librerias.
~~~
* apt update
* apt upgrade
* apt install apache2
* apt install mysql-server
* apt install php libapache2-mod-php
* apt install php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-intl php-mysql php-cli php-ldap php-zip php-curl
~~~

## Ahora activamos diferentes modulos de apache2 y lo reiniciamos.
~~~
* a2enmod proxy_fcgi
* a2enconf php-fpm
* systemctl restart apache2
~~~
## Ahora vamos a crear un server mysql.

###   Entramos en mysql de forma root
~~~
* mysql -u root
~~~
###   Creamos la base de datos y el usuario
~~~~
* CREATE DATABASE mibasededatos;
* CREATE USER 'miusuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
* GRANT ALL ON mibasededatos.* to 'miusuario'@'localhost';
~~~~
### Y despues salimos del mysql
~~~
* exit
~~~
### Entramos a mysql con tu usuario creado
~~~
* mysql -u miusuario -p
~~~
### Ahora para instalar el servidor entramos en la carpeta con `cd /var/www/html`. Y instalamos en unzip para descomprimir el fichero que descargaremos.
~~~
apt install unzip
wget https://download.owncloud.org/community/owncloud-complete-20210721.zip
unzip owncloud-complete-20210721.zip
~~~

### Te saldra una carpeta, para listarla lo hacemos con `ls -l`. Y veremos que se ha creado una carpeta que se llama owncloud.

#### Entramos con `cd Owncloud/`
### Luego copiamos los archivos a la carpeta anterior y eliminamos el index del apache2.
~~~
cp -r . ..
rm index.html
~~~
### Despues tenemos que darle permisos a la carpeta de forma que pueda ejecutarse y visualizarse
~~~
chown -R www-data:www-data /var/www/html
chmod -R 775 /var/www/html
~~~

### Luego restablecemos el servidor con `systemctl restart apache2`
### Y para acceder al servidor owncloud metemos la ip de la maquina en el navegador de forma que lo visualizes.
