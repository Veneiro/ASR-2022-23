`Mateo Rico Iglesias - UO277172`
## Backup en caliente de un sistema en modo multiusuario mediante snapshots LVM
1. En este primer punto creo la máquina con una instalación mínima al igual que hicimos en la primera práctica. En este caso en la administración de discos inicial selecciono solo uno de los dos discos dejando el otro sin uso en la instalación.
	![[Práctica 4 ASR/Images/Screenshot_13.png]]
2. Modifico el archivo /etc/issue y añado la frase "Copia de Seguridad práctica backup"
	![[Práctica 4 ASR/Images/Screenshot_14.png]]
	Y el resultado al reiniciar la máquina con `reboot now` es el siguiente
	![[Práctica 4 ASR/Images/Screenshot_15.png]]
3. En este caso instalo la orden gdisk con `dnf -y install gdisk` y mediante el mismo proceso usado en la ==Práctica 2== creo las particiones correspondientes. El resultado tras ejecutar el comando `g` en el propio `gdisk` es el siguiente
	![[Práctica 4 ASR/Images/Screenshot_16.png]]
	Al igual que en la Práctica 2, después de crear las particiones les doy el formato ex3 con los comandos `mkfs /dev/sdbX` que pone la particion en formato ex2, después les doy un nombre con `e2label /deb/sbdX XXXX` en este caso las he llamado backup1 y backup2 y por último los paso a ex3 con `tune2fs -j /dev/sdbX`.
	Si ejecuto el `lsblk -f` el resultado de sdb es el siguiente
	![[Práctica 4 ASR/Images/Screenshot_18.png]]
	Y después como se dice en la Práctica 2 uso el `mount` para montar los discos en la carpeta /mnt/backup anteriormente creada
	![[Práctica 4 ASR/Images/Screenshot_17.png]]
5. En este punto simplemente uso el comando que se me indica, `cp /etc/lvm/archive/* /mnt/backup` para hacer la copia de la configuración LVM a este nuevo disco
6.  Primero para este punto creo un volúmen físico de la particion sdb2 con `pvcreate /dev/sdb2`. Como en el punto anterior había puesto este disco con formato ex3 por error simplemente cuando me pregunta si quiero hacerle wipe de formato para ponerle el nuevo escribo 'y' y doy intro.
	![[Práctica 4 ASR/Images/Screenshot_19.png]]
	- Extiendo el grupo almalinux ya creado en sda con `vgextend almalinux /dev/sdb2`
			![[Práctica 4 ASR/Images/Screenshot_20.png]]
	- Creo la instantánea con `lvcreate -L1000M -s -n backupAS /dev/almalinux/root`
			![[Práctica 4 ASR/Images/Screenshot_21.png]]
			Y como vemos si usamos `lvs` aparece en primera posición la instantánea creada
			![[Práctica 4 ASR/Images/Screenshot_22.png]]
	- Creo el punto de montaje `/mnt/snapshot` y monto con `mount -o nouuid /dev/almalinux/backupAS /mnt/snapshot` en él
			![[Práctica 4 ASR/Images/Screenshot_23.png]]
			He probado a ejecutar el comando sin el nouuid y efectivamente sin este parámetro el comando mount no se ejecuta, sale un error diciendo que no se puede encontrar un archivo, en este caso el /etc/fstab
7. Edito el fichero /etc/issue de la máquina y después entro al del snapshot, se puede ver que el fichero en la instantánea no cambia, es el mismo que había anteriormente que edité en el primer punto de la práctica. En la parte inferior se puede ver de que disco es cada archivo, siendo el primero de la instantánea y el segundo el que he editado ahora perteneciente al disco sda principal.
			
			![[Práctica 4 ASR/Images/Screenshot_24.png]]
			
			
			![[Práctica 4 ASR/Images/Screenshot_25.png]]
8.  Instalo con el comando `dnf -y install tar` el comando tar, este lo voy a usar para guardar en un archivo comprimido el directorio /mnt/snapshot. Utilizo para esto el comando `tar -cvpzf /mnt/backup/backup.tgz /mnt/snapshot` siendo el primer directorio el dónde se va a guardar el archivo comprimido y el segundo el archivo o directorio a comprimir.
	En este caso al intentar hacer el tar con las carpetas ==/proc== y ==/dev== del sistema el ==/dev== se hace sin problema, en cambio el ==/proc== se queda trabado tratando de comprimir el ==/proc/kcore==. En el caso de las backup de la snapshot el ==proc== se completa correctamente pero por alguna razón en la salida del comando nos dice que se ha eliminado la '/' inicial de los nombres, en el caso del ==dev== de la snapshot sucede exáctamente lo mismo.
	En cuanto al *¿Por qué?* de el backup de las carpetas anteriormente mencionadas en principio en algunos ámbitos podría tener sentido, pero por lo general para una copia de seguridad de una máquina que más tarde queremos restaurar o similares no tiene mucho sentido. Primero, el ==/proc== contiene información acerca de procesos en ejecución y es más como un centro de control e información del kernel, además este es generado cuando se inicia la máquina y se disuelve al apagarla por lo que no es necesario tener una copia del mismo. Por el lado de ==/dev== contiene archivos para representar dispositivos conectados al sistema
	Como se me pide capturar la salida de los comandos `lsblk -f` y `df -Th` a continuación dejo la salida de ambos comandos en este punto de la práctica
	![[Práctica 4 ASR/Images/Screenshot_26.png]]
	![[Práctica 4 ASR/Images/Screenshot_27.png]]

9. Uso el comando tar para hacer el backup de el arranque de la máquina
	![[Práctica 4 ASR/Images/Screenshot_28.png]]

10. En el punto 10 simplemente he ejecutado los comandos en el orden que se me indica

## Restauración
Aquí tengo capturas de pantalla del proceso que he seguido para la restauración del sistema.

Primero desinstalo el disco principal de la máquina y creo uno nuevo del mismo tamaño, en este caso el el RedHat Minimal P4_1.vdi. También vuelvo a instalar la iso de AlmaLinux para el arranque.
	![[Práctica 4 ASR/Images/Screenshot_29.png]]
Inicio el sistema y selecciono la opción troubleshooting y después el modo rescue
	![[Práctica 4 ASR/Images/Screenshot_30.png]]
Con el gdisk como se hizo en las anteriores prácticas 2 y 3 particiono el disco como se me pide, se puede ver en la siguiente captura de pantalla
	![[Práctica 4 ASR/Images/Screenshot_31.png]]
Le doy el formato necesario con el mismo proceso que seguí en la prática 3 y así queda el resultado del comando lsblk -f
	![[Práctica 4 ASR/Images/Screenshot_32.png]]
Creo los puntos de montaje backup, boot y snapshot y monto primero la partición sdb1 en backup
	![[Práctica 4 ASR/Images/Screenshot_33.png]]	![[Práctica 4 ASR/Images/Screenshot_34.png]]
Después busco la UUID necesaria en el volumen físico y me aparece la siguiente UUID, se puede ver en la propia captura de pantalla al id.
	pbn5JT-lAfv-ZGaX-QhEL-6Y6N-c0dK-frjHnC
	![[Práctica 4 ASR/Images/Screenshot_35.png]]
Ejecuto las órdenes `pvcreate`, `vgcfrestore` y `vgchange` como se me pide al final del punto 5 y podemos ver que se ejecutan en las siguientes capturas de pantalla.
	![[Práctica 4 ASR/Images/Screenshot_36.png]]
	![[Práctica 4 ASR/Images/Screenshot_37.png]]
	![[Práctica 4 ASR/Images/Screenshot_38.png]]
En las dos siguientes capturas podemos ver el resultado de `pvdisplay` y `lvdispaly`
- pvdisplay
	![[Práctica 4 ASR/Images/Screenshot_39.png]]
- lvdisplay
	![[Práctica 4 ASR/Images/Screenshot_40.png]]

Le doy formato xfs al volumen root y lo monto en snapshot
![[Práctica 4 ASR/Images/Screenshot_41.png]]
Seguidamente voy al directorio raíz y extraigo el tgz con el backup
![[Práctica 4 ASR/Images/Screenshot_42.png]]
Como se me pide, uso el comando `blkid` para poder obtener los uuid.
![[Práctica 4 ASR/Images/Screenshot_43.png]]
Voy al directorio del nuevo disco y modifico el archivo fstab poniendo el uuid que he obtenido anteriormente
![[Práctica 4 ASR/Images/Screenshot_44.png]]
Por último al reiniciar la máquina y entrar en el modo de rescate selecciono la opción de continuar y como se me dice en el guión podemos ver que el sistema se monta en /mnt/sysroot
![[Screenshot_45.png]]
Al llegar a este punto me he encontrado con el problema de la falta del `/boot/efi ` ya que al comienzo en la instalación del sistema se me había olvidado activar esta opción. Los últimos pasos que me quedarían serían grub2-mkconfig en el punto en el que estamos en la anterior captura de pantalla, retirar la iso de instalación y seleccionar el disco donde acabo de hacer la recuperación como disco de inicio lo que iniciaría el sistema con normalidad. En el punto 8 de la parte obligatoria de la práctica nos encontramos la siguiente captura de pantalla donde podemos ver que tengo la partición `/boot` pero me falta la `/boot/efi` por el fallo anteriormente comentado.
![[Práctica 4 ASR/Images/Screenshot_26.png]]

## Copia de seguridad y restauración de una máquina en Azure
1. Primero creo la máquina virtual con Windows Server 2022, en la zona East US 2 y con el nombre de grupo de recursos y máquina indicados
	![[Screenshot_46.png]]
2. Dentro de la máquina virtual creo un documento `alumnos.txt` con mi UO
	![[Screenshot_47.png]]
	![[Screenshot_48.png]]
3. Creo un almacen llamado vaultAS en el grupo de recursos
	1.  Crear el almacén
		![[Screenshot_49.png]]
		![[Screenshot_50.png]]
	2. Entro al almacén y creo una nueva copia de seguridad con una directiva nueva `DailyPolicy-AS` que haga copias de seguridad todos los días a las 8:00
		![[Screenshot_51.png]]
	![[Screenshot_52.png]]
	![[Screenshot_53.png]]
	![[Screenshot_54.png]]
4. Fuerzo la primera copia de seguridad entrando al vaultAS anteriormente creado
	![[Screenshot_55.png]]
	