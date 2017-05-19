#Práctica 4
====================

##Certificado SSL

En esta práctica vamos a instalar un certificado SSL autofirmado en nuestros servidores web, para ello ejecutamos los siguientes comandos:
a2enmod ssl
```shell
service apache2 restart
mkdir /etc/apache2/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
```
###Instalación de SSL:
<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica4/img/instalacionSSLM1.PNG">

Después editamos el siguiente archivo: 
```shell
sudo nano /etc/apache2/sites-available/default-ssl
```
Y añadimos las siguientes líneas debajo de donde pone SSLEngine on:
```shell
SSLCertificateFile /etc/apache2/ssl/apache.crt
SSLCertificateKeyFile /etc/apache2/ssl/apache.key
```
###Configuración de SSL:
<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica4/img/configSSLM1.PNG">

Y finalmente reiniciamos apache:
```shell
a2ensite default-ssl
service apache2 reload
```
Una vez que hemos reiniciado el servicio accedemos al servidor web mediante el protocolo HTTPS y veremos que en la barra de dirección sale en rojo el https puesto que es un certificado autofirmado:

<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica4/img/pruebaSSL.PNG">

###Configuración del cortafuegos:
Lo que vamos a hacer es crear un script, donde configuramos el cortafuegos iptables:
<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica4/img/configCorta.PNG">

Una vez que tenemos el script necesitamos que se ejecute siempre que se inicie el servidor web, esto lo hemos solucionado con el demonio cron:
<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica4/img/cronIpe.PNG">


