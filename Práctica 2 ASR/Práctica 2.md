# Práctica 2
## Trabajando con máquinas virtuales y discos
### A. Replicación y traslado de máquinas virtuales

Realicé la exportación de la máquina en formato ova tras apagar la máquina y se me generó un archivo .ova

<img src="./Images/Screenshot%20(6).png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot%20(7).png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot%20(8).png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Al abrir el archivo con Winrar en este caso podemos ver que en el interior están tanto el archivo vmdk como el ovf.

<img src="./Images/Screenshot%20(1).png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Importo el archivo recién creado y vemos como se genera una nueva máquina con el nombre de la primera más un 1 detrás para diferenciarla.

<img src="./Images/Screenshot%20(2).png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Procedo a cambiarle el nombre al que se me pide, Linux pr2

<img src="./Images/Screenshot%20(3).png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Le bajo la memoria ram a 1200 MB.

<img src="./Images/Screenshot%20(4).png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Y genero una nueva dirección MAC dándole al botón con las flechas azules que aparece en esta imagen.

<img src="./Images/Screenshot%20(5).png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

### B. Instantáneas

Primero instalo el paquete que necesito para usar el nslookup, en este caso según podemos ver con el comando whatprovides es el paquete bind-utils.

<img src="./Images/Screenshot_3.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Podemos ver que tras instalar el paquete ya podemos usar el comando nslookup.

<img src="./Images/Screenshot_4.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Tras restaurar la instantánea que había creado al iniciar esta máquina e iniciar sesión podemos ver que todo lo que habíamos instalado y hecho se borra, parecido a lo que ocurre cuando retrocedemos a un punto de restauración en windows.

<img src="./Images/Screenshot_5.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Al borra la máquina junto con todos sus ficheros todas las instantáneas se borran.

### Añadir un nuevo disco a las MVs

Procedo a crear un nuevo disco duro virtual con un tamaño de 8 GB.

<img src="./Images/Screenshot_6.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_7.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Lo asigno al controlador SATA  de la máquina virtual

<img src="./Images/Screenshot_8.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Con el comando parted compruebo las particiones e información del disco, con esto podemos ver que todo lo relacionado con el sistema de arranque y todo esto se encuentra en la ruta /dev/sda por otro lado lo relacionado con el segundo disco añadido será dev/sdb. En linux los nombres de los discos se nombran de esta manera, sda, sdb... mientras que si llevan un numero detrás como sda1 ese número indicaría la partición del disco que se trata.

<img src="./Images/Screenshot_9.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_10.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

El comando gdisk ya se encontraba instalado, al probar el siguiente comando se me muestra que el disco sdb, el nuevo que hemos instalado, no tiene ninguna partición.

<img src="./Images/Screenshot_11.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Dentro de gisk ya puedo comenzar a establecer las particiones, primero una de 512 MiB con el formato por defecto Linux filesystem.

<img src="./Images/Screenshot_13.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_15.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Después creao una de 2.5 GiB con el mismo formato.

<img src="./Images/Screenshot_14.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_16.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Y por último una partición de 5 GiB pero en este caso con el formato Microsoft basic data. Para poner este formato habrá que introducir el código 0700 en el paso en el que se nos pide código ya que el que se pone por defecto en caso contrario es el Linux filesystem.

<img src="./Images/Screenshot_17.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Si utilizo ahora el comando parted con este disco podemos ver las tres particiones que he creado anteriormente podemos ver que las particiones se han guardado correctamente

<img src="./Images/Screenshot_18.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Ahora voy a crear el filesystem para todas las particiones. Para el primer disco utilizo el comando mkfs, después el eZlabel para cambiar el nombre y por último pongo el sistema de archivos a ex3 con el tune3fs.

<img src="./Images/Screenshot_20.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_34.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_35.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_33.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_23.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_24.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_25.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_26.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_27.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_28.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_29.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_30.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_31.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_32.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">
