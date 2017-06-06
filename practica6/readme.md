# Práctica 6

## Configuración de RAID 1

### Creación de discos virtuales
Queremos montar dos discos duros en RAID 1 por lo que primero vamos al software de virtualización, en este caso Virtual Box, y creamos dos discos del mismo tamaño que el disco principal donde se encuentra el sistema operativo:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-virtualbox.PNG">

### Creación de RAID 1 con dos discos
Después de agregar los discos, éstos aun no estan montados como RAID 1, esto lo vamos a realizar por software iniciando la máquina virtual. A continuación mostraremos todos los discos que hay en el sistema con el comando *fdisk -l* con permisos de administrador:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-fdisk.PNG">

Luego usamos el dispositivo */dev/md0* para crear el RAID 1 con los dos discos */dev/sdb* y */dev/sdc*. A continuación se le da formato ext2.

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-mdadm.PNG">
<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-format.PNG">

### Montaje de RAID 1

Creamos una carpeta en la que montamos */dev/md0* y comprobamos que la operación se ha realizado correctamente:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-mount.PNG">
<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-check.PNG">

Ahora tenemos que editar el archivo */etc/fstab* para que se monte el dispositivo RAID cada vez que se arranca el sistema. Para ello lo renombraremos media su UUID:

```shell
ls -l /dev/disk/by-uuid/
```
<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-uuid.PNG">

Cogemos la ID correspondiente a */md0* (en este caso) y añadimos la siguiente línea al final del fichero */etc/fstab*:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-fstab.PNG">

### Funcionamiento del RAID 1

Hemos simulado un fallo al disco que se encuentra en */dev/sdb* y podemos ver con *mdadm* que ocurre con el RAID donde avisa de que ha habido un fallo en el disco:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-failure.PNG">

Ahora vamos a crear un archivo en la partición y remover uno de los discos para ver si podemos seguir teniendo acceso a dicho archivo:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-remove.PNG">

Como podemos ver a continuación a pesar de que uno de los discos se ha eliminado, podemos seguir accediendo a los datos que hay partición ya que nos lo muestra el otro disco. 

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-detail.PNG">

Vamos a crear otro archivo diferente y luego montamos el disco que se había extraido para comprobar que sigue existiendo:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-add.PNG">
<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-working.PNG">

Por último como el último archivo creado, *file.txt*, ha sido creado sólo cuando estaba funcionando el disco */dev/sdc*, ahora se va a eliminar este. De esta forma también verificaremos que el RAID 1 está funcionando correctamente:

<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-remove2.PNG">
<img src="https://github.com/Olivencia/ugr_swap/blob/master/practica5/img/raid-detail2.PNG">


