``Hecho por Mateo Rico Iglesias - UO277172``

## PARTE 1: OBLIGATORIA

Primero se nos pide instalar una página Wordpress con el XAMPP en la máquina Windows. Entro en la página de apache y de wordpress y descargo los archivos para usarlos.

Instalo el XAMPP simplemente dándole al ejecutable y realizando la instalación por defecto y después cojo lo que contiene el zip de wordpress y copio esa carpeta en la carpeta htdocs del XAMPP para poder posteriormente ejecutar la instalación.

He cambiado lo siguiente en el archivo de configuración de XAMPP para poder acceder a la parte del wordpress, para facilitar el proceso he activado los índices en la página y he comentado la opción que hace que se cargen por defecto las páginas index.php, index.html... Ya que el archivo de instalación se llamaba install.php.

![[Screenshot_206.png|center|500]]

Tras hacer esto ya ejecuto el archivo instalador en el navegador

![[Screenshot_207.png|center|500]]

![[Screenshot_208.png|center|500]]

Conectaré el Wordpress a la propia base de datos del XAMPP que se podrá utilizar y acceder a ella desde la dirección del servidor accediendo a la carpeta phpmyadmin. Al entrar en la página creo una nueva base de datos que será la que utilice en el wordpress.

Tras terminar la instalación siguiendo los pasos que se me pide realizar llegamos al panel de configuración de wordpress tras hacer log in en la página /admin de la página del servidor.

![[Screenshot_209.png|center|500]]

Y aquí si nos vamos a Appearance podremos cambiar el estilo por defecto de la página

![[Screenshot_210.png|center|500]]

Ahora procedo a hacer el proceso de instalación directamente en la máquina linux. Primero de todo instalo maria db con el commando ``dnf install php mariadb mariadb-server php-mysqlnd``

![[Screenshot_211.png|center|500]]

Tras hacer esto habilito las normas del firewall para permitir el servicio, en este caso ya tenía una activada de las prácticas anteriores

![[Screenshot_212.png|center|500]]

Y por último recargo el firewall para aplicar correctamente los cambios

![[Screenshot_213.png|center|500]]

I restart the mariadb service and then I enable it to make it start when the machine starts
![[Screenshot_214.png|center|500]]

I restarts the httpd service too

![[Screenshot_215.png|center|500]]

The I execute the command ``mysql_secure_installation`` to secure the mariadb database, just using the options by default should be enought

![[Screenshot_216.png|center|500]]

Creo una base de datos para joomla y un usuario con privilegios para acceder a ella 

![[Screenshot_219.png|center|500]]

Y por último descargo el zip de joomla desde ``https://downloads.joomla.org/es/cms/joomla3/3-9-25/Joomla_3- 9-25-Stable-Full_Package.tar.gz``, lo extraigo e instalo, para esto necesito los paquetes de tar y wget para obtener el zip y luego extraerlo

![[Screenshot_220.png|center|500]]

![[Screenshot_221.png|center|500]]

![[Screenshot_222.png|center|500]]

Después de esto me he encontrado con un problema, muy probablemente debido a alguna configuración de mi máquina linux que llevo usando durante todas las prácticas. Al intentar entrar a la página después de haber cambiado la configuración de httpd para que fuera al directorio de joomla y todo los php se me mostraban como texto plano en vez de ejecutarse por lo que me era imposible realizar la instalación de joomla.

Después de tratar de arreglarlo he optado por usar una máquina linux limpia que tenía de una práctica anterior, en este caso una máquina con interfaz gráfica que habíamos creado como prueba.

Tras realizar de nuevo los mismo pasos que he comentado anteriormente en la nueva máquina el proceso ha sido bastante intuitivo, he accedido a mi página y se ha cargado directamente el proceso de instalación, en esta primera página creo un usuario para la administración de joomla, pongo el nombre del sitio y una descripción

![[Screenshot_226.png|center|500]]

Tras hacer esto conecto el joomla con la base de datos que hemos creado en los pasos previos, usando el usuario y contraseña que habíamos creado al hacer segura la base de datos mariadb

![[Screenshot_227.png|center|500]]

Y por último le doy a instalar

![[Screenshot_228.png|center|500]]

Ya teniendo esto instalado el último problema con el que me he encontrado es que joomla no era capaz de crear el archivo de configuración a pesar de haber dado permisos en la carpeta por lo que me daba el código como se puede ver en la nota de abajo que está en rojo y me pedía crearlo. Este archivo lo dejo en la carpeta general de joomla. Por último también borro la carpeta installation como me pide el joomla para poder acceder a toda la página

![[Screenshot_229.png|center|500]]

Tras hacer todo esto aquí podemos ver como se puede acceder tanto al apartado de administración, en el que tendré que entrar con el usuario que había creado al principio de la instalación de joomla y también se podrá ver ya la página de joomla por defecto.

![[Screenshot_230.png|center|500]]

![[Screenshot_231.png|center|500]]

A partir de aquí la tarea de añadir cualquier contenido a la página es sencilla, todo se podrá añadir desde la página de administración. Si nos fijamos en la barra lateral tenemos para crear un nuevo articulo, listar los articulos creados, crear o listar las categorías y añadir o listar el contenido multimedia. 

También tenemos para añadir elementos estructurales como menús o nuevos módulos donde podremos introducir los articulos u otros elementos de contenido.

Desde este panel también podemos gestionar los usuarios, cambiar configuraciones o incluso añadir nuevas extensiones al sitio o hacer mantenimientos.

He probado a añadir un nuevo módulo que me muestre los últimos articulos añadidos a la página así como un nuevo artículo que será el primero de la página.

Primero le damos a nuevo artículo y entraremos en esta pantalla donde escribimos nuestro artículo. Ya terminado le damos a guardar y lo cerramos.

![[Screenshot_232.png|center|500]]

Vamos a el apartado de módulos, le damos a crear uno nuevo y aquí definiremos el nombre y a la derecha pondremos en que posición queremos que se muestre. En este caso yo lo llamaré *Last Articles* y lo situaré en el cuerpo principal de la página

![[Screenshot_234.png|center|500]]

Si entramos a la página principal ya podemos ver este nuevo módulo y en este caso aparece el articulo que acabamos de añadir

![[Screenshot_235.png|center|500]]

Y si hacemos click encima nos llevará directamente al articulo correspondiente

![[Screenshot_236.png|center|500]]

En general todos los contenidos básicos son así de intuitivos y no debería llevar a mucho problema.

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

## PARTE 2: OPCIONAL

### Base de datos del servidor Joomla de linux

El proceso para llegar al resultado que se pide es sencillo.

1. Ejecuto el comando ``mysql -u root -p`` en un terminar
2. Ejecuto los comandos mysql que se me proporcionan en la documentación los cuales crearán la base de datos uo277172_base, insertarán algunos datos en la misma y también crearán un usuario para acceder a la base de datos
	![[Screenshot_237.png|center|500]]
	![[Screenshot_238.png|center|500]]
3. Creo un directorio dentro del joomla llamado por ejemplo "bd" y dentro de este creo un archivo *index.php* con el código php que se me proporciona en la práctica, cambiando el nombre de la base y el usuario por el que he elegido yo
4. Por último simplemente accediendo al enlace **localhost/bd/index.php** ya se mostrará el resultado

![[Screenshot_239.png|center|500]]

### Base de datos del servidor XAMPP de Windows

El proceso es similar al anterior pero, esta vez, los comandos sql se ejecutarán desde la página **localhost/phpmyadmin/**

1. Entro en la dirección de **localhost/phpmyadmin/**
2. En la barra superior hago click en el apartado SQL
3. Voy ejecutando los comandos poco a poco, en este caso primero ejecuto los de creación de usuarios y base de datos, después el de creación de la tabla y por último los de insertar y eliminar datos
4. Tras hacer esto la base ya estará funcionando, solo necesito hacer algo similar a lo que hice en linux. Voy a la carpeta htdocs de XAMPP, creo un directorio "bd" dentro de la misma y en su interior creo el archivo php con el mismo contenido que el de linux
5. Tras hacer esto si entro a la dirección **localhost/bd/index.php** ya podré ver el resultado

![[Screenshot_240.png|center|500]]




















