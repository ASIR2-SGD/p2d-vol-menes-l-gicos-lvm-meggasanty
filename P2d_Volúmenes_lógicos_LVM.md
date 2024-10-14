# Práctica 2d. Volúmenes Lógicos(LVM)

## Contexto
El volúmen de información a manejar crece e muchas ocasiones de forma no prevista. Una previsón a la baja en la asignación del espacio de almacenamiento en unidades físicas puede ocasionar quedarnos sin espacio con el problema que ello conlleva, una asignación a la alta supone perdida de espacio para otros fines. Mediante LVM o Logical Volumen Management obtenemos mediante una capa de abstracción sobre el volumen físico mayor flexibilidad a la hora de incrementar, reducir el espacio inicialmente previsto para nuestros datos.

En esta práctica se trabajará sobre la creación y gestión de volúmenes lógicos.

## Objetivos
* Entender que es LVM y como mejora el esquema estático de asignación de espacio.
* Explicar de forma gráfica la arquitectura de LVM.
* Crear y configrar LVM mediante comandos.
* Ser capaces de plantear un hipotético caso de ampliación de espacio en un volúmen lógico
* Documentar de forma correcta y completa los pasos llevados para realizar la práctica.


## Definición y Arquitectura de LVM
>[!NOTE]
> Explicar ayudándote de una imagen la arquitectura de Lvm

LVM (Logical Volume Manager) es una implementación de linux que permite la administración cómoda de volúmenes físicos, agrupándolos en grupos lógicos.
Este sistema nos permite administrar el espacio de almacenamiento disponible de forma sencilla y la implementación de sistemas RAID.

![lvm](2_2_3_01.jpg)

## Desarrollo


### Introducción
>[!NOTE]
> Explicar brevemente en que va a consistir la práctica, ayúdate de una imágen para tu explicación.

Vamos a añadir los discos indicados al sistema de lvm, tras esto, crearemos varios volúmenes lógicos que montaremos en carpetas concretas, de forma que se crearán los espacios de almacenamiento que especifiquemos, con el espacio que le indiquemos.

### Paso 1. Pasos previos
>[!NOTE]
> Explicar la ampliacion de la máquina virtual con nuevos discos duros

Para añadir los discos, actualizamos el vagrantfile, donde se había especificado que discos tenía nuestra máquina virtual. De esta forma al iniciarla, podíamos comprobar que se habían añadido 6 discos de 200 MB de memoria

>[!TIP]
> La comprobación la llevaremos a cabo con el comando 
> ```bash
>lvscan
>```

Tras esto creamos los "phisical volume" lo que permitirá al lvm gestionarlos.

esto se hará con el comando
```bash
sudo pvcreate /dev/{sdb,sdc}
```

### Paso 2. Creación de los volúmenes lógicos

creamos los "logical volume"
```
sudo lvcreate -L 80MB -n lv-projects  vg-sad000
```

Esta operación consta de las siguientes partes:
* **Operación:** sudo lvcreate
* **Tamaño:** -L 80MB
* **Nombre:** -n lv-projects
* **Grupo:** vg-sad000

>[!NOTE]
>Realizamos esto para cada volúmen que queramos crear

### Paso 3. Formatear los discos y añadir el sistema de archivos que utilizará.

para llevar a cabo este paso utilizaremos el comando
```bash
sudo mkfs.ext4 /dev/vg-sad000/lv-net
```
lo  utilizaremos para cada volumen lógico

### Paso 4. Montar los discos

creamos los directorios
```bash
sudo mkdir -p /mnt/{projects,db,web,net}
```

los montamos
```
sudo mount /dev/vg-sad000/lv-projects /mnt/projects
```
>[!TIP]
>```
>df -h para comprobar
>```
