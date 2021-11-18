# Instalacion de nextcloud

En el manual se explicara todos los pasos a seguir para hacer una correcta instalacion de nextcloud en Vagrant.

1. Lo primero de todo es crear un directorio.
~~~
mkdir Mp08
~~~
2. Una vez creado el directorio accedemos en el con `cd Servidor/`
3. Creamos una maquina virtual mediante Vagrant
~~~
[jrojas@alumne-1-41 Mp08]$ vagrant init ubuntu/focal64
~~~
4. La encendemos con el sisguiente comando.
~~~
[jrojas@alumne-1-41 Mp08]$ vagrant up --provider=virtualbox
~~~
5. Ahora entramos en el vagrant.
~~~
[jrojas@alumne-1-41 Mp08]$ vagrant ssh
~~~
6. Una vez iniciada la maquina podemos hacernos root.
~~~
vagrant@ubuntu-focal:~$ sudo -s
~~~
7. Una vez siendo root actualizamos los servicios y instalamos apache2 con.
~~~
root@ubuntu-focal:/home/vagrant# apt update
root@ubuntu-focal:/home/vagrant# apt install apache2
~~~
8. Ahora lo que vamos a hacer es entrar en la carpeta `/var/www/html`. Y eliminamos el index.html.
~~~
root@ubuntu-focal:/home/vagrant# cd /var/www/html
root@ubuntu-focal:/var/www/html# rm index.html
~~~
9. Despues de esto nos descargamos el fichero en el directorio de la pagina oficial de nextcloud.
~~~
root@ubuntu-focal:/var/www/html# wget https://download.nextcloud.com/server/releases/nextcloud-22.2.3.zip
~~~
10. Ahora nos instalamos el descomprimidor de ficheros. Y lo descomprimimos.
~~~
root@ubuntu-focal:/var/www/html# apt install unzip
root@ubuntu-focal:/var/www/html# unzip nextcloud-22.2.3.zip
~~~
11. Con `ls -l` podemos ver la carpeta descomprimida.

12. Ahora accedemos a la carpeta con `root@ubuntu-focal:/var/www/html# cd nextcloud`
13. Y con el comando `root@ubuntu-focal:/var/www/html/nextcloud# cp -r . ..` Pasamos la carpeta con todos los archivos al anterior.
14. Despues de esto le damos permisos a la carpeta para que pueda visualizarse y ejecutarse.
~~~
root@ubuntu-focal:/var/www/html/nextcloud# chown -R www-data:www-data /var/www/html
root@ubuntu-focal:/var/www/html/nextcloud# chmod -R 775 /var/www/html
~~~
15. Lo siguiente que vamos a hacer es la instalacion de mysql-server y sus librerias correspondientes.

~~~
root@ubuntu-focal:/var/www/html/nextcloud# apt install -y mysql-server
root@ubuntu-focal:/var/www/html/nextcloud# apt install php libapache2-mod-php
root@ubuntu-focal:/var/www/html/nextcloud# apt install -y php-fpm php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-mysql php-cli php-ldap php-zip php-curl
~~~
Al finalizar reiniciamos el servidor apache con `systemctl restart apache2`

16. Ahora vamos a crear la base de datos, para ello entramos en mysql con el comando `mysql`
~~~
mysql> CREATE DATABASE bbdd;
~~~
Y creamos los usuarios con contraseña.
~~~
mysql> CREATE USER 'usuario'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
mysql> CREATE USER 'usuario'@'192.168.22.100' IDENTIFIED WITH mysql_native_password BY 'password';
~~~
Y continuacion le damos los privilegios a los usuarios ya creados.
~~~
mysql> GRANT ALL ON bbdd.* to 'usuario'@'localhost';
mysql> GRANT ALL ON bbdd.* to 'usuario'@'192.168.22.100';
~~~
17. Para acabar reinciamos apache2 y mysql con `systemctl restart apache2` y `systemctl restart mysql`
18. Ahora desde nuestro navegador entramos al servidor con localhost8080.
