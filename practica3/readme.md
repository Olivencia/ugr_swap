#Práctica 3
====================

##Nginx

Para esta práctica hemos creado una nueva máquina virtual, en la cual no hemos instalado Apache, así como se pedía en la práctica, para tener el puerto 80 libre.
Una vez creada esta máquina hemos instalado nginx.
```shell
sudo apt-get update && sudo apt-get dist-upgrade && sudo apt-get
autoremove
sudo apt-get install nginx
sudo systemctl start nginx
```
Ahora lo que tenemos que hacer es cambiar la configuración de nginx, vamos a usar un algoritmo round-robin para gestionar las peticiones. En un principio todas van a tener la misma prioridad.

Configuración de nginx:
<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica3/img/configNginx.PNG">

Reiniciamos el servicio
```shell
sudo systemctl restart nginx
```
Podemos ver su funcionamiento desde una cuarta máquina, en la cual solo hace falta que tengamos instalado Curl
Prueba de nginx:
<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica3/img/pruebaNginx.PNG">

También podemos hacer varias configuraciones en el archivo de configuación de nginx como por ejemplo:

Con weight podemos asignar más prioridad a una de las máquinas:
<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica3/img/configNginx2.PNG">

Con ip_hash podemos hacer un balanceo por IP,  de esta forma podemos hacer que todo el tráfico que sea de la misma IP este siempre en la misma máquina y el balanceador no haga ningun cambio de máquina:
<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica3/img/configNginx3.PNG">

Con keepalive podemos utilizar conexiones entre nginx y los servidores finales, de forma que se realice una conexión con una presistencia de múltiples pretiones HTTP en lugar de abrir cada vez una nueva conexión.
<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica3/img/configNginx4.PNG">

##Haproxy

Ahora vamos a instalar el balanceador haproxy, lo vamos a instalar en la misma máquina que nginx, por eso antes de nada tenemos que para el servicio de nginx y después instalar haproxy.
```shell
sudo systemctl stop nginx
sudo apt-get install haproxy 
```

Una vez instalado tenemos que configurar haproxy, para ello editamos el siguiente archivo:
```shell
cd /etc/
cd haproxy/
ifconfig
sudo nano haproxy.cfg 
```
Configuración de haproxy:

<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica3/img/pruebaHaproxy.PNG">

Una vez configurado haproxy lo lanzamos:
```shell
sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg
```
Como hicimos antes comprobamos el funcionamiento de haproxy con Curl:

Prueba de haproxy:

<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica3/img/pruebaHaproxy.PNG">

Someter a una alta carga el servidor balanceado

Vamos a hacer una prueba a los servidores usando los dos balanceadores con Apache Benchmark 
```shell
ab -n 1000 -c 10 http://192.168.182.136/index.html
```
Benchmark de nginx:

<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica3/img/testNginx.PNG">

Benchmark de haproxy:

<img src="https://github.com/Olivencia/ugr_swad/blob/master/practica3/img/testHaproxi.PNG">






