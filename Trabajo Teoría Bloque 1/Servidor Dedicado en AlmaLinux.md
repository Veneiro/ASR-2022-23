``Hecho por Mateo Rico Iglesias - UO277172``
## Introducción
Antes de comenzar todo el proceso de instalación y configuración del servidor dedicado  comentaré un poco el espacio de trabajo que usaré para la práctica. En este caso para la práctica usaré una máquina física que he montado para realizar esta función, es una máquina sencilla pero funciona perfectamente para esta función.
El equipo cuenta con las siguientes características:
- Placa base: Gigabyte Technology Co., Ltd. H81M-HD3
- RAM: 12 GiB
- Procesador: Intel® Core™ i3-4160 CPU @ 3.60GHz × 4 
- Tarjeta gráfica: GTX 730
- Disco SSD: 500 GB
- SO: AlmaLinux 9.1 (Lime Lynx) 64 bits
En este caso he realizado la instalación de servidores dedicados para dos juegos, el Minecraft y el Left 4 Dead 2, a ambos puedo conectarme desde cualquier parte cuando quiera mientras el equipo que los aloja este en funcionamiento lo que me permitirá jugar con amigos a ambos juegos y editar todas las configuraciones de ambos servidores a mi gusto.
<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>
## Proceso de instalación de AlmaLinux
Primero de nada me he descargado la ISO de Almalinux que se nos proporciona en el campus virtual, la cual posee la version 9.1
Tras hacer esto he usado una herramienta llamada Rufus que me permite crear un medio de instalación en un pendrive y después de esperar un rato a que se configurara ya tengo lo necesario para comenzar la instalación.

Primero de nada inicio el equipo desde el pendrive para comenzar y obtengo el mismo proceso de instalación que se podía ver cuando hicimos las prácticas. *(Pido disculpas por la calidad, al no ser una máquina virtual no podía sacar capturas)*

Primero le damos a **Install Almalinux 9.1** lo que iniciará la instalación

![[IMG_20230318_140219.jpg|center|560]]

Seguidamente seleccionamos el idioma y la distribución del teclado, en este caso Español en ambos para facilitar el uso del sistema

![[IMG_20230318_140330.jpg|center|560]]

En cuanto al particionado del disco, debido a que no tengo ninguna necesidad especial simplemente permitiré que lo realice de manera automática

![[IMG_20230318_141116.jpg|center|560]]

Creo también la un usuario por defecto y selecciono una contraseña de root

![[IMG_20230318_141223.jpg|center|560]]

Configuro la red para poder usar internet tras instalar el sistema y que ya esté conectado a mi red, en este caso para tener conexión he usado un adaptador WiFi de TPLink que será suficiente para el uso que le voy a dar al sistema

![[IMG_20230318_141813.jpg|center|560]]

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

Como podemos ver ya sale perfectamente conectado a la WiFi

![[IMG_20230318_141821.jpg|center|560]]

Tras esto selecciono mi zona horaria correctamente, en este caso la de Madrid y ya estaría todo configurado correctamente

![[IMG_20230318_142011.jpg|center|560]]

Y tras esperar un rato la instalación se completa sin problemas

![[IMG_20230318_143527.jpg|center|560]]

Tras seguir este proceso aquí tenemos ya la pantalla de inicio de sesión donde procederé a inciar sesión en el usuario que creé durante la instalación

![[IMG_20230318_150924.jpg|center|560]]

Y después del proceso, aquí tenemos el escritorio preparado para usarse

![[IMG_20230318_151951.jpg|center|560]]
<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

## Servidor dedicado de Minecraft
El proceso para crear el servidor de Minecraft no es tan complejo, primero antes de nada deberé instalar el Java, simplemente tratando de usar el comando java ya me dirá que tengo que hacer para instalarlo y el paquete que debo instalar

![[IMG_20230318_152548.jpg|center|560]]
<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

#### Primera prueba: Hamachi
Para hacer esta primera prueba más sencilla y comprobar que todo funciona intentaré primero usar Hamachi, este programa te permite un número ilimitado de redes VPN cada una con un máximo de 5 miembros. Descargo el paquete en formato rpm que es el que lee AlmaLinux

![[IMG_20230318_153158.jpg|center|560]]

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

Y se me quedará el siguiente archivo. Simplemente con ejecutarlo el sistema me permitirá instalarlo

![[IMG_20230318_153356.jpg|center|500]]

Tras ejecutarlo me envía a la ventana de las herramienta gnome, le doy al botón de instalar y en cuanto termine el proceso ya podré hacer uso del programa

![[IMG_20230318_153420.jpg|center|500]]

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

Para utilizar el Hamachi en linux, al no tener interfaz, deberemos guiarnos por la línea de comandos.
- Primero iniciaremos el servicio con ``sudo service logmein-hamachi start``
- Tras iniciarlo usaremos el comando ``sudo hamachi login`` para conectar el programa y podremos usar ``sudo hamachi set-nick "nombre"`` para ponerle un nombre identificativo al equipo cuando lo veamos en una red, en este caso yo usaré de nombre *HostServer*
- Teniendo ya iniciado el programa y configurado un nombre crearé una red que usaré para que otra gente en esta pueda acceder a mis servidores dedicados en esa máquina, para esto deberé usar ``sudo hamachi create "Nombre de la Red"`` poniendo en el nombre lo que quiera pero que deberá ser un nombre original. Tras poner un nombre se me pedirá que le de una contraseña que otros usuarios deberán poner para unirse a la red
- Ahora tras esto toda la gente que entre en esta red VPN podrá coger la IPV4 que se ha asignado a mi equipo mediante la interfaz de red del programa y podrá entrar mediante esa ip a mi servidor, eso sí, deberé abrir los puertos de cada juego en el firewall del equipo, en este caso como el servidor que usaré para la primera prueba es el de Minecraft abriré el puerto *25565*.

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

#### Proceso de configuración
El proceso de configuración es sencillo
- Primero nos descargamos desde la página [Download server for Minecraft | Minecraft](https://www.minecraft.net/es-es/download/server) el archivo ejecutable para crear el servidor
- Después ponemos este archivo en una carpeta donde queramos que se nos guarde toda la información del servidor
- Abrimos una terminal en la ubicación de esta carpeta para ejecutar por primera vez el servidor, para esto hemos de escribir ``java -Xmx1024M -Xms1024M -jar minecraft_server..jar nogui`` sustituyendo *minecraft_server.jar* por el nombre de nuestro jar y asignando la cantidad de memoria RAM que creamos conveniente en los comandos Xmx (Cantidad máxima) y Xms (Cantidad mínima)
- La primera vez que lo ejecutemos nos pedirá que antes de continuar aceptemos el acuerdo EULA, esto se hará entrando al archivo eula.txt y cambiando el eula de false a true
	![[Screenshot_201.png|center|560]]
- Tras hacer esto ya solo deberemos ejecutar de nuevo el comando y esperar a que nos aparezca el mensaje *Done* en la línea de comandos.
Después de este punto ya tendremos un servidor básico pero aún no se puede entrar desde una máquina que no sea la que lo hostea. Aquí es donde viene la utilidad del Hamachi para probar la conectividad y estabilidad del servidor en el sistema que estamos usando.

Para hacerlo más sencillo me he unido a la red anteriormente creada desde otro ordenador con Windows en este caso. Como podemos ver aparece el *HostServer* y mi ordenador, que en este caso es el último miembro, los otros miembros son otras personas con las que he probado el correcto funcionamiento de el servidor.

![[Screenshot_202.2.jpg|center|560]]

Para que el hamachi haga efecto solo deberemos coger esa dirección IPV4 que aparece al lado del host server y la pegaremos en la configuración del servidor, en este caso en el archivo server.properties. En este archivo deberemos buscar la opción *server-ip=* y escribir ahí la ip que hemos cogido.

Solo con esto ya podremos acceder al servidor si nos encontramos dentro de la misma red VPN de Hamachi que él. Para hacer esto entramos a nuestro juego y nos conectamos a la ip 25.50.91.116:25565

Toda la personalización se puede hacer a gusto del usuario que realiza el servidor. La imagen se modifica añadiendola a la carpeta principal del servidor con el nombre **server-icon.png** y el texto que aparece se puede modificar dentro del mismo archivo **server.properties** escribiendo lo que quieras en el apartado **motd**

![[Screenshot_204.2.png|center|560]]

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

Al final lo importante es que se se puede acceder perfectamente al servidor como se puede ver en la siguiente imagen

![[Screenshot_203.png|center|560]]

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

## Servidor Dedicado L4D2

Para hacer este servidor el proceso es un tanto diferente, necesitaremos una consola de comandos propietaria de Steam y un nuevo usuario en la máquina.

### Requisitos previos
1. Actualización de paquetes con ``sudo dnf update``
2.  Instalar los paquetes necesarios: `sudo dnf install lib32gcc1 libc6-i386`
3.  Crear un usuario llamado "steam": `sudo adduser steam` (Esto por razones de seguridad porque este proceso puede causar problemas en en usuario y de esta manera si ocurre algo no perderíamos el usuario principal)

### Instalar SteamCMD
1. Entraremos en el usuario con ``su steam`` desde nuestro usuario principal
2. Iremos al directorio home de steam, primero usamos ``cd ..`` y después ``cd /steam``
3. Descargamos el SteamCMD con ``wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz``
4. Lo descomprimimos con el uso del comando tar escribiendo lo siguiente ``tar -xvzf steamcmd_linux.tar.gz``
5. Y por último lo podemos ejecutar con ``./steamcmd.sh``

![[IMG_20230326_130118.jpg|center|560]]

### Instalar el Servidor de Left 4 Dead 2
1. En este punto estaremos desde el steamCMD, primero hacemos un login anonymous para conectarnos a los servidores de Steam
2. Seleccionamos el directorio de instalación del servidor con ``force_install_dir ./L4D2Server/``
3. Descargamos los archivos del servidor con ``app_update 222860 validate`` y esperamos a que se termine la descarga

### Ejecutar el servidor
Para esto simplemente nos dirigimos a la carpeta del servidor que está en *./L3D2Server* donde hemos indicado con anterioridad. Dentro de esta carpeta crearé un archivo de formato **.sh** con el siguiente contenido

![[Screenshot_223.png|center|560]]

Esto nos permitirá iniciar el servidor de manera más sencilla, evitaremos tener que escribir el comando en un terminal cada vez que queramos usarlo y nos permitirá editar la configuración en el archivo server.cfg a nuestro gusto y seleccionar el mapa en el que se iniciará el servidor, en este caso el *c1m1_hotel*.

Después de tener este archivo lo guardamos y ya esta todo listo para iniciar el servidor. Abro un terminal situado en la carpeta del servidor y escribo *./StartL4D2Server.sh* siendo este el nombre del archivo que le he puesto al archivo de script anterior.

Tras un ratito esperando a que carge el servidor se iniciará, por el momento estará solo en local debido a que no he configurado el firewall. Deberé abrir el puerto 27015 que es el puerto por defecto de este juego y con esto cualquiera que este en mi vpn de Hamachi podrá entrar al servidor desde el juego poniendo en la consola del propio juego ``connect [ip_de_hamachi]:27015``.

![[Trabajo Teoría Bloque 1/Images/Screenshot_142.png|center|560]]

Y como podemos ver, tras probar desde otro ordenador puedo entrar perfectamente

![[Screenshot_224.png|center|560]]

![[Trabajo Teoría Bloque 1/Images/Screenshot_139.png|center|560]]

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

Aquí se puede ver que puedo usar el menu de Admin ya que soy el propio host del servidor

![[Trabajo Teoría Bloque 1/Images/Screenshot_140.png|center|560]]

## Conclusión final

Todo el trabajo en su totalidad ha sido realizado con el uso de Hamachi y sus redes virtuales debido a que todo lo relacionado con la red como la apertura de puertos estaba fuera del alcance de lo que era realmente relevante para esta práctica.

Otra cosa para la que me ha sido útil el empleo de este programa ha sido para conectarme remotamente a la máquina utilizando el Remote Desktop de Windows como hicimos en su momento para conectarnos a la máquina que habíamos creado en Azure, simplemente abriendo el puerto 3389 en el firewall de la máquina y utilizando en el servicio rdp la IPV4 de Hamachi con ese puerto podía conectarme sin problemas para realizar todos los cambios a distancia.

Creo que la realización de esta prática ha sido un trabajo realmente útil. Además al haber hecho uso de un equipo físico he podido trabajar con más tiempo y calma que el que hubiera tenido usando servicios en la nube y he podido ver como es el hecho de tener que mantener un servidor que está en mi propio domicilio (a una escala diminuta como es evidente). 

El hecho de tener configurar por mi propia cuenta el equipo donde iba a trabajar, enfrentarme a todos los problemas que se me presentaban relacionados con la creación de estos servidores dedicados y todo el proceso hasta tener algo funcional ha sido algo que me ha hecho ver realmente como sería una situación "real" de cara a manejar algo de este estilo además de a fin de cuentas proveerme finalmente los conocimientos necesarios para poder configurar servidores de otros ámbitos en la máquina o incluso servicios (me ha permitido tener un host para un bot de la plataforma discord creado por mi mismo).

Creo que este tipo de trabajos son realmente útiles de cara a que cada alumno se las ingenie y plante cara a los problemas para conseguir un objetivo y además el hecho de darnos la capacidad para elegir un tema de trabajo ha sido algo que me ha encantado.





































































