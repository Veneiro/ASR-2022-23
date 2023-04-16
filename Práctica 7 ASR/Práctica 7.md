``Hecho por Mateo Rico Iglesias - UO277172``
## 1. Instalación
Cambio el hostname de la máquina linux con ``hostnamectl`` y con ``uname -a`` se puede commprobar que se ha cambiado correctamente.
![[Screenshot_152.png]]
Podemos comprobar que desde la máquina WS2022 ya se puede resolver el ping a la dirección *linux.as.local*
![[Screenshot_153.png]]
Al igual que en la anterior, en la máquina W10 también se puede
![[Screenshot_154.png]]
Y por último si pruebo desde la máquina linux también obtengo respuesta del ping
![[Screenshot_171.png]]
Al igual que la práctica anterior creo otra zona de búsqueda directa en el dns de la máquina WS2022 con la terminación *midominio.local* y teniendo el host *www*. Esto me permitirá redirigir el tráfico a este nombre de dominio a la máquina linux.
![[Screenshot_170.png]]
Después de comprobarlo el servicio apache no está instalado por lo que lo instalo con ``dnf install httpd``
![[Screenshot_155.png]]
Activo el sistema con el systemctl para poder usarlo
![[Screenshot_160.png]]
Y actualizo las reglas del firewall para no tener problemas
![[Screenshot_159.png]]
Tras hacer esto vuelvo a probar desde las tres máquinas que el ping funciona correctamente a este nuevo nombre de dominio
![[Screenshot_156.png]]
![[Screenshot_157.png]]
![[Screenshot_158.png]]
Y por útlimo podemos ver que desde la máquina Windows ya se puede acceder a este dominio correctamente, mostrándose la página por defecto
![[Screenshot_161.png]]
Tras crear el archivo index.html con el comando ``vi /var/www/html index.html`` y editarlo con el código html que se puede encontrar en la prática, si trato de volver a acceder al sitio se puede ver que ya se ha actualizado correctamente
![[Screenshot_162.png]]

## 2. Configuración de las páginas web de los usuarios
En este punto se nos permitirá crear páginas web independientes para cada usuario de la máquina linux.
Primero accedo al usuario asuser que ya tengo creado de la anterior práctica, para esto he tenido que usar el comando ``usermod -s /bin/bash asuser`` ya que en la anterior práctica debido a que el usuario solo era necesario para el servicio samba lo había creado sin bash con un */sbin/nologin*. Tras hacer esto ya puedo acceder al usuario con normalidad o en mi caso usar el ``su asuser`` desde el usuario root para acceder más rápidamente.
Antes de acceder a este usuario voy a realizar una serie de cambios:
- Edito el fichero situado en **/etc/httpd/conf.d/userdir.conf** para comentar la línea que pone *UserDir disabled* y descomentar la línea *UserDir public_html* lo que me permitirá crear diferentes html por usuario.
	![[Screenshot_163.png]]
- Aplico los permisos necesarios con ``chmod 711 /home/asuser`` al directorio, ejecuto ``setsebol -P httpd_read_user_content on`` y `` setsebool -P httpd_enable_homedirs on`` para permitir que Apache pueda leer contenidos localizados en los directorios de inicio de los usuarios locales y habilitar el uso del directorio */public_html* en los usuarios, que como podemos ver en la anterior captura es el directorio de donde se buscará la información para la página de cada usuario.
	![[Screenshot_164.png]]
- Ahora si ya entro como el usuario *asuser*, en este caso yo entraré con un ``su asuser`` desde el usuario root que estaba usando y creo en el directorio con ``mkdir ./public_html`` la carpeta necesaria para la página. Tras esto con el chmod 755 -R public_html daré los permisos necesarios en esa carpeta para continuar el proceso.
	![[Screenshot_165.png]]
	![[Screenshot_166.png]]
- Dentro de esta carpeta generaré el dichero index.html que se mostrará en la página del usuario asuser con ``vi index.html``
	![[Screenshot_168.png]]
	![[Screenshot_167.png]]
- Por útlimo reinicio el servicio con ``systemctl restart httpd`` y si pruebo desde la máquina W10 ya podré acceder a la página personal del usuario con la dirección ``http://www.midominio.local/~asuser`` 
	![[Screenshot_169.png]]

## 3. Configuración del servidor Apache

### 3.a Ubicación

Primero creo la nueva ubicación para el servidor, en este caso */as/web*
![[Screenshot_172.png]]
Sustituyo la ruta por defecto en el documento *httpd.conf* a la ruta de la nueva carpeta
![[Screenshot_173.png]]
Y por último restauro el servicio httpd para aplicar los cambios
![[Screenshot_174.png]]

### 3.b ServerName

Modifico las directivas ServerAdmin y ServerName del arhivo con la siguiente información como se me pide en la documentación. Primero cambio el *ServerAdmin* a mi UO con el dominio que utiliza mi servidor y después el *ServerName* a el dominio de mi servidor más el *puerto 9999* que he puesto nuevo
![[Screenshot_175.png]]
Cambio el *Listen* para que escuche al nuevo puerto que quiero utilizar
![[Screenshot_177.png]]
Y podemos ver que tras reiniciar el servicio httpd y copiar mi *index.html* al nuevo directorio la página ya funciona con el puerto nuevo
![[Screenshot_176.png]]

### 3.c Repositorios

Renombro el archivo que moví a la nueva carpeta a índice.html lo que provoca que se vuelva a mostrar la página por defecto del servidor. Tras hacer esto comento por completo el archivo welcome.conf. Al ser este el archivo del que se saca la página por defecto entonces no se puede mostrar ninguna página tras hacer esto,  por lo que obtengo un error de que no puedo acceder al recurso que estoy solicitando.
![[Screenshot_179.png]]
Después de probar esto, vuelvo al archivo de configuración de httpd y modifico el código donde había puesto el *as/web* y añado un **Indexes** en la primera línea. Tras recargar la página obtengo una página que me muestra todo lo contenido en el directorio base que tengo seleccionado, en este caso el *as/web* permitiendome así seleccionar el html índices que tengo en la carpeta y visualizándolo.
![[Screenshot_180.png]]
Por útlimo trato de acceder a una página que no existe, por ejemplo, trato de acceder al archivo hola.html el cual no existe. Si entro al archivo *access_log* podemos ver que se ha guardado mi petición en la que trato de acceder a esta página.
![[Screenshot_178.png]]

## 4. Hosts Virtuales

Configuro un nuevo alias para *midominio.local* llamado *otraempresa*
![[Screenshot_183.png]]
Para configurar mi servidor con un nuevo host virtual he hecho lo siguiente:
- Creo un nuevo directorio ``as/web/`` al que llamaré *otraempresa* y después muevo el anterior html a un directorio dentro de esta carpeta llamado *www* para tener todo más organizado.
- Creo dos directorios en ``/etc/httpd/`` uno llamado *sites-available* y otro llamado *sites-enabled* en los cuales haré la configuración para todos los hosts virtuales.
- Entro en el fichero de configuración de httpd y añado al final la siguiente línea ``IncludeOptional sites-enabled/*.conf``
- Aquí en la carpeta sites-available crearé primero todos los ficheros de configuración y después los pasaré a sites-enabled para que empiecen a funcionar. La estructura en estos es una estructura simple 
	```
	<VirtualHost *:80>
		ServerName otraempresa.midominio.local
		DocumentRoot /as/web/otraempresa
	</VirtualHost>
	```

	```
	<VirtualHost *:80>
		ServerName www.midominio.local
		DocumentRoot /as/web/www
	</VirtualHost>
	```
- Y después de esto con el comando ``sudo ln -s /etc/httpd/sites-available/otrampresa.conf /etc/httpd/sites-enabled/otraempresa.conf`` y ``sudo ln -s /etc/httpd/sites-available/www.conf /etc/httpd/sites-enabled/www.conf`` activo estos host virtuales
- Tras reiniciar la máquina para reiniciar el servicio y comprobar que está correctamente ejecutandose podemos ver que ya podemos entrar en el nuevo host virtual
![[Screenshot_182.png]]

## 5. Autentificación
Primero añado la línea *AllowOverride AuthConfig* a la parte directory del virtual host dedicado a la dirección *otraempresa.midominio.local*
```
<VirtualHost *:80>
	ServerName otraempresa.midominio.local
	DocumentRoot /as/web/otraempresa
	<Directory "/as/web/otraempresa">
		AllowOverride AuthConfig
	</Directory>
</VirtualHost>
```

Después de esto creo el archivo *.htaccess* en la carpeta ``as/web/otraempresa`` donde se encuentra mi html para este host virtual y le añado lo siguiente
```
AuthType Basic
AuthName "Area Restringida"
AuthUserFile /etc/httpd/password.file
AuthGroupFile /dev/null Require valid-user
```
Tras esto la página ya me pide un usuario y una contraseña para poder visualizarla
![[Screenshot_184.png]]
Después de hacer esto tengo que añadir unos usuarios y contraseñas al archivo password.file, de esta manera si introduzco los datos de los nuevos usuarios podré ver la página. Esto lo haré con los comandos siguientes
```
htpasswd –c /etc/httpd/password.file usuario1
htpasswd /etc/httpd/password.file usuario2
```
Tras hacer esto ya puedo iniciar sesión con ambos usuarios y ver el contenido de la página como se puede ver en las capturas siguientes
![[Screenshot_185.png]]
![[Screenshot_186.png]]
## 6. Servidor Proxy - squid
Primero de nada instalo el squid con el comando ``dnf install squid``, con esto ya podré empezar este apartado.
Tras instalarlo inicio el servicio y lo habilito para que se inicie automáticamente con cada arranque de la máquina linux
![[Screenshot_187.png]]
![[Screenshot_188.png]]
Añado el rango de red donde se puede encontrar el servidor al fichero de configuración de squid y descomento la línea del cache_dir para que se me guarden todos los movimientos de la red que pasen a través del proxy
![[Screenshot_189.png]]
![[Screenshot_191.png]]
Creo una excepción en el firewall para que deje pasar todo lo relacionado con el puerto de squid.
![[Screenshot_190.png]]
Después voy a la configuración del navegador, busco la opción de proxy y al clickar me manda a la configuración del proxy de Windows. En la pestaña que me sale activaré la opción de *Usar servidor proxy* y rellenaré los campos con el proxy que voy a usar, en este caso la dirección ``www.midominio.local`` y el puerto de squid que es el ``3128``
![[Screenshot_193.png]]
![[Screenshot_194.png]]
Tras hacer esto y probar a navegar en la máquina Windows con el comando tail podremos ir viendo que el proxy está funcionando y está pasando el tráfico por la máquina linux.
![[Screenshot_192.png]]