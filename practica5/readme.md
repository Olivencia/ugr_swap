# Práctica 5
====================

## Creación de una BD en mySQL

Lo primero de todo entramos en mySQL con nuestro usuario y contraseña con permisos de administrador:

```shell
mysql -u root -p
Enter password:
```

Una vez dentro vamos a crear una base de datos llamada "contactos", que tendrá una tabla "datos". Ésta contiene dos variables, nombre de tipo char y tlf de tipo entero. Con todo esto ya podemos insertar tuplas en la base de datos:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/mysql-creacionDB-tabla.PNG">
<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/mysql-muestraTuplas.PNG">

 
## Backup de la BD con mysqldump
Para realizar esta parte vamos a utilizar mysqldump, que es una herramienta que vuelca el contenido de la base de datos seleccionada a un fichero de tipo ".sql". 

Hay que tener en cuenta que mientras se ejecuta este comando las tablas pueden estar modificándose por lo que se recomienda el uso de una sentencia para que las tablas sean de solo lectura "flush tables with read lock".

Bloqueadas las tablas para que solo sean de lectura procederemos a crear la copia de seguridad y una vez terminada podremos desbloquear las tablas con "unlock tables"

Utilizaremos la opción "-e" para usar las sentencias mySQL desde el propio shell:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/mysql-backup.PNG">


## Restauración de backup de BD en otra máquina
Para restaurar una BD de un archivo ".sql" en la segunda máquina podemos utilizar SCP para conectarnos a la primera y descargar dicho archivo. Nos pedirá la contraseña del usuario al que nos conectamos y a continuación se descargará.

Para poder importar correctamente la base de datos es necesario tener la base de datos creada en la segunda máquina y después con mysql volcamos los datos en ella. Si nos fijamos es igual que exportar salvo por el signo "<" en lugar de ">" (indica la dirección donde se realizará el backup, si a una base de datos o a un archivo).

Terminada la restauración deberá aparecer un mensaje como el siguiente:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/mysql-restore-backup.PNG">

## Réplica de BD mediante maestro-esclavo
La idea de esta parte es que nuestras dos máquinas tengan siempre la misma información referente a la BD de forma que todo lo que ocurra en la máquina 1 se refleje en la máquina 2.

Vamos a modificar el archivo de configuración que se encuentra en /etc/mysql/mysql.conf.d/mysql.conf y añadir/modificar lo siguiente:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/mysql-conf.PNG">

Esto mismo tenemos que realizarlo en la máquina 2 pero cambiando server-id a 2.

Reiniciamos el servicio de mysql mediante:
```shell
sudo service mysql restart
```

A continuación vamos al maestro y ejecutamos las siguientes sentencias en la consola de mysql:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/mysql-maestro.PNG">

Ya tenemos la configuración para la máquina maestra, ahora tenemos que ir a la esclava e indicarle quién es la maestra. Esto se realiza indicandole la IP, el usuario y la contraseña del usuario esclavo. Es importante tener en cuenta que el MASTER_LOG_FILE y el MASTER_LOG_POS corresponde con las dos primeras columnas de la tabla maestra (al utilizar SHOW MASTER STATUS).

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/mysql-esclavo.PNG">

Para comprobar si se aplicado lo anterior correctamente tenemos que ejecutar en el esclavo la orden "SHOW SLAVE STATUS\G" en la consola de mySQL y comprobar que "Seconds_Behind_Master" es distinto de "NULL" y sea igual a 0.

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/mysql-res.PNG">

