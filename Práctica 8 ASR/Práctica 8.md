``Hecho por Mateo Rico Iglesias - UO277172``

## PARTE 1: OBLIGATORIA

Primero se nos pide instalar una página Wordpress con el XAMPP en la máquina Windows. Entro en la página de apache y de wordpress y descargo los archivos para usarlos.

Instalo el XAMPP simplemente dándole al ejecutable y realizando la instalación por defecto y después cojo lo que contiene el zip de wordpress y copio esa carpeta en la carpeta htdocs del XAMPP para poder posteriormente ejecutar la instalación.

He cambiado lo siguiente en el archivo de configuración de XAMPP para poder acceder a la parte del wordpress, para facilitar el proceso he activado los índices en la página y he comentado la opción que hace que se cargen por defecto las páginas index.php, index.html... Ya que el archivo de instalación se llamaba install.php.

![[Screenshot_206.png|center|600]]

Tras hacer esto ya ejecuto el archivo instalador en el navegador

![[Screenshot_207.png|center|600]]

![[Screenshot_208.png|center|600]]

Conectaré el Wordpress a la propia base de datos del XAMPP que se podrá utilizar y acceder a ella desde la dirección del servidor accediendo a la carpeta phpmyadmin. Al entrar en la página creo una nueva base de datos que será la que utilice en el wordpress.

Tras terminar la instalación siguiendo los pasos que se me pide realizar llegamos al panel de configuración de wordpress tras hacer log in en la página /admin de la página del servidor.

![[Screenshot_209.png|center|600]]

Y aquí si nos vamos a Appearance podremos cambiar el estilo por defecto de la página

![[Screenshot_210.png|center|600]]

Ahora procedo a hacer el proceso de instalación directamente en la máquina linux. Primero de todo instalo maria db con el commando ``dnf install php mariadb mariadb-server php-mysqlnd``

![[Screenshot_211.png|center|600]]

Tras hacer esto habilito las normas del firewall para permitir el servicio, en este caso ya tenía una activada de las prácticas anteriores

![[Screenshot_212.png|center|600]]

Y por último recargo el firewall para aplicar correctamente los cambios

![[Screenshot_213.png|center|600]]

I restart the mariadb service and then I enable it to make it start when the machine starts
![[Screenshot_214.png|center|600]]

I restarts the httpd service too

![[Screenshot_215.png|center|600]]

The I execute the command ``mysql_secure_installation`` to secure the mariadb database, just using the options by default should be enought

![[Screenshot_216.png|center|600]]

Creo una base de datos para joomla y un usuario con privilegios para acceder a ella 

![[Screenshot_219.png|center|600]]

Y por último descargo el zip de joomla desde ``https://downloads.joomla.org/es/cms/joomla3/3-9-25/Joomla_3- 9-25-Stable-Full_Package.tar.gz``, lo extraigo e instalo, para esto necesito los paquetes de tar y wget para obtener el zip y luego extraerlo

![[Screenshot_220.png|center|600]]

![[Screenshot_221.png|center|600]]

![[Screenshot_222.png|center|600]]

Después de esto me he encontrado con un problema, muy probablemente debido a alguna configuración de mi máquina linux que llevo usando durante todas las prácticas. Al intentar entrar a la página después de haber cambiado la configuración de httpd para que fuera al directorio de joomla y todo los php se me mostraban como texto plano en vez de ejecutarse por lo que me era imposible realizar la instalación de joomla.

Después de tratar de arreglarlo he optado por usar una máquina linux limpia que tenía de una práctica anterior, en este caso una máquina con interfaz gráfica que habíamos creado como prueba.

Tras realizar de nuevo los mismo pasos que he comentado anteriormente en la nueva máquina el proceso ha sido bastante intuitivo, he accedido a mi página y se ha cargado directamente el proceso de instalación, en esta primera página creo un usuario para la administración de joomla, pongo el nombre del sitio y una descripción

![[Screenshot_226.png|center|600]]

Tras hacer esto conecto el joomla con la base de datos que hemos creado en los pasos previos, usando el usuario y contraseña que habíamos creado al hacer segura la base de datos mariadb

![[Screenshot_227.png|center|600]]

Y por último le doy a instalar

![[Screenshot_228.png|center|600]]

Ya teniendo esto instalado el último problema con el que me he encontrado es que joomla no era capaz de crear el archivo de configuración a pesar de haber dado permisos en la carpeta por lo que me daba el código como se puede ver en la nota de abajo que está en rojo y me pedía crearlo. Este archivo lo dejo en la carpeta general de joomla. Por último también borro la carpeta installation como me pide el joomla para poder acceder a toda la página

![[Screenshot_229.png|center|600]]

Tras hacer todo esto aquí podemos ver como se puede acceder tanto al apartado de administración, en el que tendré que entrar con el usuario que había creado al principio de la instalación de joomla y también se podrá ver ya la página de joomla por defecto.

![[Screenshot_230.png|center|600]]

![[Screenshot_231.png|center|600]]
