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

### C. Añadir un nuevo disco a las MVs

#### Adición de un segundo disco a un sistema Linux ya instalado

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

El proceso para crear el sistema de archivos es el mismo en el caso de xfs pero para cambiarle el nombre a la partición deberemos usar xfs_admin como se ve abajo.

<img src="./Images/Screenshot_34.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Y de la misma manera para FAT32 pero en este caso usaremos fatlabel para cambiar el nombre.

<img src="./Images/Screenshot_35.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Aquí vemos ya las tres particiones con el formato y el nombre correspondientes.

<img src="./Images/Screenshot_33.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Después he usado el mount de cada una de las particiones para montarlas en el sistema de archivos y aquí podemos ver que he conseguido acceder con el cd perfectamente a la carpeta e incluso crear un archivo de texto de prueba en cada una de ellas.

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

También hago la prueba con el comando lsblk -f para ver que todas las particiones de ambos discos están perfectamente creadas y con los nombres.

<img src="./Images/Screenshot_26.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

#### Adición de nuevos discos a un sistema Windows ya instalado.

Aquí se puede ver como he instalado ambos discos en el controlador IDE de la máquina Windows.

<img src="./Images/Screenshot_27.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Ambos discos los formateo como GPT desde el administrador de discos de Windows.

<img src="./Images/Screenshot_28.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Con el primer disco selecciono la opción de crear volumen simple y después lo inicio como sistema de archivos NTFS.

<img src="./Images/Screenshot_29.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Con el segundo disco he hecho lo mismo que el primero pero creando dos particiones diferentes, es decir partiendo el disco en dos volumenes simples de la mitad del espacio total. Este sería el resultado tras la operación.

<img src="./Images/Screenshot_30.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Después como se me dice borro ambas particiones del disco 2 y la única del disco 1 y dejo el espacio como no asignado.

<img src="./Images/Screenshot_31.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Tras unir los dos discos mediante el NTFS distribuido la nueva unidad tiene el tamaño total unido de ambos discos, 8156 MB

<img src="./Images/Screenshot_32.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Si borro el volumen distribuido que une ambos discos se borra por completo por lo que la nueva unidad no tiene espacio ya que se borra.

### D. Transvase de discos entre máquias con distintos operativos

Creo un disco en formato NTFS en la máquina Windows para realizar el transvase.

<img src="./Images/Screenshot_36.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Creo un documento de texto, en este caso llamado Bienvenidos con el texto, "Bienvenido a mi máquina virtual!" en el interior. Este documento será el que abra más adelante en el sistema linux.

<img src="./Images/Screenshot_37.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/Screenshot_38.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Desinstalo el disco de la máquina Windows y lo instalo en la máquina linux, como se puede ver el disco llamado Windows Server 2022_1

<img src="./Images/Screenshot_39.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Instalo ambos paquetes, epel-release y ntfs-3g para que mi sistema linux pueda leer los discos con el formato ntfs.

<img src="./Images/Screenshot_40.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Si probamos el comando lsblk -f podemos ver que el sistema linux ya reconoce el disco.

<img src="./Images/Screenshot_41.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Tras montarlo con el comando mount, en este caso en la carpeta /mnt/prueba podemos ver que el documento de texto llamado Bienvenidos se encuentra en el interior, el documento es el que había creado en la máquina Windows.

<img src="./Images/Screenshot_42.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Como podemos ver aquí podemos previsualizar el documento y editarlo con normalidad.

<img src="./Images/Screenshot_43.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

En el disco 1 solo encontramos dos particiones, una que no tiene ninguna clase de formato o nombre y otra que es la del sistema de archivos. La partición 1 no tiene nada en su interior.

<img src="./Images/Screenshot_44.png" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">
