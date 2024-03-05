``Hecho por Mateo Rico Iglesias - UO277172``

## Balanceo de carga con HAProxy

Esta práctica la realizaré en una máquina linux completamente de instalación limpia y con GUI.

Primero de nada instalo la herramienta necesaria para la práctica, el HAProxy

![[Screenshot_241.png|center|560]]

Clono esta máquina y les cambio el nombre, así tendre el balanceador y los servidores de ambas web

![[Screenshot_242.png|center|560]]

Entro a las preferencias globales de VirtualBox y desactivo el Servidor DHCP

![[Screenshot_243.png|center|560]]

Con el comando ``nmcli connection modify enp0s3 ipv4.method manual ipv4.address 192.168.56.20/24`` configuro la ip de las tres máquinas, en este caso el balanceador tendrá la *192.168.56.20/24*, la web1 la *192.168.56.21/24* y la web 2 la *192.168.56.22/24*

![[Screenshot_244.png|center|560]]

![[Screenshot_245.png|center|560]]

![[Screenshot_246.png|center|560]]

Hago varios ping desde el anfitrión y podemos ver que hay respuesta desde las tres máquinas

![[Screenshot_247.png|center|560]]

![[Screenshot_248.png|center|560]]

![[Screenshot_249.png|center|560]]

Después de esto creo el archivo index.html con el código que se me da en la documentación y abro los servicios necesarios en el firewall
````
<html>
	<head>
		<title>Servidor web 1</title>
	</head>
	<body>
		<h1>Servidor web 1</h1>
	</body>
</html>
````

![[Screenshot_250.png|center|560]]

Como podemos ver se puede acceder perfectamente a las páginas, tanto desde ellas como desde el anfitrión

![[Screenshot_251.png|center|560]]

![[Screenshot_252.png|center|560]]

Tras cambiar las configuraciones de haproxy.cfg que se me pide lo que sucede es lo siguiente, al entrar a la página del balanceador aparece uno de los servidores web, si voy actualizando la página lo que sucede es que algunas veces sale un servidor y otras veces el otro.

Si apago uno de los servidores lo que sucede es que deja de aparecer.

Después de esto procedo a eliminar los ficheros html y los sustituyo por otros php con el código que se me da en la práctica 
````
<html> 
<script type="text/javascript"> 
// muestra las cookies que hay en el navegador 
function MuestraCookies() { 
	var todas=document.cookie; 
	
	// array de pares nombre - valor 
	en_array=todas.split(';'); 
	
	// muestra cada par 
	for (var i=0; i<en_array.length; i++) { 
	nombre=en_array[i].split('=')[0]; 
	valor=en_array[i].split('=')[1]; 
	document.write("cookie "+nombre+" = "+valor+"<br>\n\r"); 
	} 
} 
</script> 
<script type="text/javascript"> 
	MuestraCookies(); 
</script> 
	<body> 
		<?php 
		// añade una cookie de sesión 
		session_start(); 
		// muestra el servidor que atiende la petición
		$server_ip=$_SERVER['SERVER_ADDR']; 
		echo "petición servida por: ".$server_ip."<br>".PHP_EOL; 
		?> 
	</body> 
</html>
````

Como podemos ver si entramos a la página nos sale la cookie de sesión y el servidor al que estamos accediendo y por mucho que recarguemos siempre nos lleva al primero que aparece.

![[Screenshot_253.png|center|560]]

![[Screenshot_254.png|center|560]]

La diferencia entre esto y lo que teníamos antes es que las alternantes cada vez que recargas la página te puede salir una diferente, en cambio la pegajosa la primera que te sale es la que se queda aunque recargues la página hasta que borres las cookies

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

## SAN (Storage Area Network)

Primero de nada configuramos la ip de las máquinas creadas, para una la ip 192.168.222.1 y para la otra la 192.168.222.2 todo esto hecho con los siguientes comandos

![[Screenshot_260.png|center|560]]

![[Screenshot_261.png|center|560]]

Comprobamos las zonas activas del firewall y creamos un archivo de 1GB que se exportará como un disco

![[Screenshot_257.png|center|560]]

![[Screenshot_258.png|center|560]]

Como podemos ver si hacemos ping entre las máquinas la conexción está funcionando

![[Screenshot_262.png|center|560]]

![[Screenshot_263.png|center|560]]

Instalo targetcli en la máquina servidor, habilitarlo para que se ejecute en el arranque y después añadir la excepción al firewall

![[Screenshot_266.png|center|560]]

![[Screenshot_267.png|center|560]]

![[Screenshot_268.png|center|560]]

Tras esto procedo a crear un IQN objetivo, primero para el disco y después para el fichero de 1G llamado fichero.dsk. Aquí simplemente sigo los pasos que se me indica en el pdf uno por uno.

![[Screenshot_270.png|center|560]]

![[Screenshot_271.png|center|560]]

![[Screenshot_272.png|center|560]]

![[Screenshot_273.png|center|560]]

![[Screenshot_274.png|center|560]]

![[Screenshot_275.png|center|560]]

![[Screenshot_276.png|center|560]]

![[Screenshot_277.png|center|560]]

![[Screenshot_278.png|center|560]]

Tras hacer esto me voy a la otra máquina e instalo el paquete iscsi-initiator-utils

![[Screenshot_279.png|center|560]]

Depués de esto creo el documento initiatorname.iscsi en la ruta que se me pide con la línea *InitiatorName=iqn.2023-02.as.cliente:2222* y tras hacer esto con el siguiente comando compruebo si se puede ver el servidor.

![[Screenshot_280.png|center|560]]

Usamos el comando ``iscsiadm --mode=node --targetname=iqn.2023-02.as.servidor:1111 -- portal=192.168.222.1 --login`` después de esto último y a partir de aqui ya nos saldrán los nuevos discos

![[Screenshot_295.png|center|560]]

Procedo a montar el disco mediante el uso del UUID que obtengo con el comando blkid. 

![[Screenshot_284.png|center|560]]

Para montarlo auntomáticamente añado al archivo /etc/fstab la siguiente línea 
``UUID={YOUR-UID}{/path/to/mount/point}{file-system-type}defaults,errors=remount-ro 0 1``  
completando con los datos de mi disco, punto de montura y tipo de sistema de archivos y despues ejecuto el comando siguiente

![[Screenshot_285.png|center|560]]

Si tras hacer esto reinicio la máquina y compruebo el estado de el iscsi vemos que está operativo

![[Screenshot_286.png|center|560]]

Y aquí adjunto la captura del comando *df* y del *cat*

![[Screenshot_287.png|center|560]]

![[Screenshot_288.png|center|560]]

Por útlimo lo que necesitamos es asignar un usuario y contraseña para hacer el login, para esto seguiré los pasos que se indican, primero ejecutos los comandos siguientes

![[Screenshot_289.png|center|560]]

![[Screenshot_290.png|center|560]]

Cambio la configuración del iscsid descomentando y modificando los campos que me dice

![[Screenshot_291.png|center|560]]

Borro lo datos de contacto anteriores

![[Screenshot_292.png|center|560]]

Compruebo que el servidor sigue funcionando bien

![[Screenshot_293.png|center|560]]

Y por último realizo de nuevo el login

![[Screenshot_294.png|center|560]]
