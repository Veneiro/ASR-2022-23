## Introducción
En este segundo trabajo de teoría voy a continuar con el trabajo realizado para el primer trabajo, añadiendo la capacidad de acceder a los servicios configurados en el servidor Almalinux sin necesidad de estar en una red VPN privada mediante la apertura de puertos en el router, apertura del firewall... además de otras cosas como la conexión remota al servidor y más. 
El equipo utilizado sigue siendo el mismo que para el anterior trabajo con las características siguientes:
- Placa base: Gigabyte Technology Co., Ltd. H81M-HD3
- RAM: 12 GiB
- Procesador: Intel® Core™ i3-4160 CPU @ 3.60GHz × 4 
- Tarjeta gráfica: GTX 730
- Disco SSD: 500 GB
- SO: AlmaLinux 9.1 (Lime Lynx) 64 bits
A pesar de ser un equipo antiguo en cuanto a componentes ha demostrado ser suficientemente potente para los usos que le estoy dando de manera regular.

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

## Página del router

Como siempre, para todo el proceso de abrir los puertos antes de nada deberemos saber donde se realiza este proceso. En este caso deberemos entrar a la dirección *192.168.1.1* que es la ip por defecto de nuestra red en la cual se encuentra la configuración del router de nuestro proveedor. Este proceso será realizado desde el router que ofrece el proveedor de Jazztel por defecto.

![[Screenshot_157(pixel).png|center|560]]

Como podemos ver tendremos que entrar con un usuario y contraseña que es diferente para cada router hoy en día, o al menos en el caso de Jazztel es así. Tras entrar a la configuración veremos lo siguiente.

![[Screenshot_158(pixel).png|center|560]]

Aquí ya se nos presenta toda la configuración del router y todo lo que podemos gestionar. Para todo el tema relacionado con los puertos entraremos primero en *Configuración avanzada* y después nos iremos al apartado de *NAT/PAT* en el caso de mi router, esto puede cambiar dependiendo del modelo de router con el que estemos trabajado pero la vista que encontraremos será para todos algo simiral a lo siguiente.

![[Screenshot_159(pixel).png|center|560]]

En esta pantalla tendremos algo de información y abajo del todo una tabla donde añadiremos la excepciones o normas para los puertos.

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

## Explicación del proceso (Servidor de Minecraft)

Primero, antes de comenzar con la configuración en el router deberemos saber cual es la ip de la máquina porque la necesitaremos para indicarle al router para que dirección ip de la red de nuestra vivienda se abrirá el puerto. Con el comando *ip addr* y usando la opción *grep* buscaremos el dispositivo de red de la máquina como se puede ver en la siguiente imagen y obtendremos la ip, en este caso *192.168.1.130*.

![[Trabajo Teoría Bloque 2/Images/Screenshot_160.png|center|560]]

Ahora con esto ya podemos ir de nuevo a la configuración del router para comenzar. Para crear la regla en el router primero en la parte de *aplicación/servicio* seleccionamos *nuevo* y ponemos un nombre identificativo para saber con más claridad para que es esa regla. En este caso lo llamaré *Minecraft TCP* porque esta será una regla TCP para el servidor de Minecraft creado en el anterior trabajo. En la parte de *puerto interno* y *puerto externo* pondremos el puerto 25565 que es el puerto por defecto de minecraft, en la parte de *protocolo* pondremos el protocolo UDP y por último en la ip del dispositivo pondremos la ip que habíamos visto en la imagen anterior.

![[Screenshot_161(cut).png|center|560]]

Tras darle a añadir vemos que la parte de puerto externo cambia y nos pone otro puerto diferente al que habíamos seleccionado, esto lo hace el router automaticamente seleccionando uno de los puertos disponibles para que usemos en nuestro servicio, el puerto interno sería el que está abierto en la máquina y el externo el puerto por el que se pasa en el router hasta llegar a la máquina.

Ya con esto tendremos todo listo, solo quedaría crear la regla del firewall. Para esto usaremos el comando del firewall, *sudo firewall-cmd --permanent --zone=public --add-port=25565/tcp* y el comando *sudo *sudo iptable -I INPUT -p tcp --dport 25565 --syn -j ACCEPT* y con esto ya estaría listo.

Hay que recordar que tras hacer todo esto deberemos ir al archivo properties del server y cambiar en la opción ip-addr la ip que teníamos del Hamachi por la dirección de la máquina, en este caso la *192.168.1.130* para que más tarde nos deje entrar y en el caso del puerto podremos dejar el que teníamos, el 25565, ya que es el que estamos usando en la configuración del router.

![[Trabajo Teoría Bloque 2/Images/Screenshot_167.png|center|560]]

Si queremos entrar al servidor deberemos entrar a través de este puerto, pero en el caso de la ip es algo diferente. No podemos usar la ip de la máquina debido a que es una ip que solo está dentro de mi red, la de mi casa por lo que otra gente desde el exterior no podrá acceder. Para que otra gente acceda desde el exterior tendrá que entrar usando la ip de mi router y usando el puerto externo anteriormente mencionado, lo cual hará una redirección hacia mi máquina de manera directa.

Para obtener esta ip el proceso es sencillo, hay infinidad de páginas en internet que te la pueden proveer, solo deberemos poner *cualesmiip* en el buscador de google y elegir la página que prefiramos, en este caso he seleccionado la página *cual-es-mi-ip.net*. Como podemos ver abajo nos sale una ip, en este caso nuestra dirección pública de nuestro router, con esto y el puerto externo, el 4957 podremos entrar desde cualquier parte al servidor de Minecraft siempre y cuando se esté ejecutando solo escribiendo en el juego la dirección ``188.76.46.249:4957``.

![[Trabajo Teoría Bloque 2/Images/Screenshot_162.png|center|560]]

Si probamos desde otro ordenador usando la ip que he mencionado como podemos ver en el juego ya nos carga y nos aparece que está en línea.

![[Trabajo Teoría Bloque 2/Images/Screenshot_163.png|center|560]]

![[Screenshot_164(pixel).png|center|560]]

Y se puede ver que podemos entrar sin mayor problema y jugar con normalidad.

![[Trabajo Teoría Bloque 2/Images/Screenshot_166.png|center|560]]

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

## Breve explicación para L4D2

Si quiero configurar esto para el otro servidor de Left4Dead2 el proceso es similar, simplemente tendré que abrir el puerto UDP, que es el puerto necesario para este juego con el número de puerto que necesito que sería el 27015.

![[Screenshot_161(cut2).png|center|560]]

Sólamente con esto y abrir los puertos en el firewall de la máquina este servidor funcionará perfectamente. En este caso no se necesita cambiar nada más allá en la configuración.

Para el firewall se utilizará *sudo firewall-cmd --permanent --zone=public --add-port=27015/tcp* y  *sudo *sudo iptable -I INPUT -p tcp --dport 27015 --syn -j ACCEPT* y con esto ya estaría listo. Como se puede ver el comando es igual cambiando el puerto, que en este caso es el 27015, el puerto por defecto del Left4Dead2.

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

## Conexión remota a la máquina Almalinux

Durante todo este rato y debido a que no podía transportar el servidor de un lado para otro he buscado diferentes soluciones para conectarme de manera remota y manejarlo desde cualquier lugar. Estás soluciones empezaron por diversos programas de compartición de pantalla como puede ser el Teamviewer pero finalmente he acabado usando el RDP propio de Windows por mayor comodidad.

Este servicio se puede usar para conectarse a la máquina linux pero deberemos instalar un servicio nuevo en nuestro servidor.

El servicio en cuestión se llama **xrdp** que se podrá instalar con el simple uso de un `dnf install xrdp`. 

![[Trabajo Teoría Bloque 2/Images/Screenshot_168.png|center|560]]

Con esto ya tendremos lo necesario para conectarnos al menos de manera local por rdp, solo debemos ir al equipo Windows con el que nos queremos conectar y poner la ip de nuestra máquina junto al puerto 3389 que es el puerto del servicio rdp. 

![[Trabajo Teoría Bloque 2/Images/Screenshot_169.png|center|560]]

Para esta prueba he abierto en el firewall de mi servidor este puerto al igual que hice en el paso anterior con el comando `firewall-cmd` y el `iptable` para evitar cualquier problema.

Tras darle al botón de conectar nos pedirá aceptar una serie de certificados para conectarnos a la máquina que al aceptarlos nos darán paso a la siguiente pantalla.

![[Trabajo Teoría Bloque 2/Images/Screenshot_170.png|center|560]]

Esta parte del proceso es la que mayor problemas puede causar a pesar de parecer de lo más sencilla. 

Si recordamos, en Windows cuando nosotros nos conectamos a una máquina mediante rdp se nos cierra la sesión en el ordenador al que nos conectamos para pasar a darnos acceso a esta en el ordenador que está conectándose lo que nos da a entender que no se podría estar conectado en la misma cuenta del ordenador en dos sesiones al mismo tiempo como puede resultar lógico.

El problema se presenta cuando nos damos cuenta de que si intentamos conectarnos a la máquina linux, aunque pongamos la cuenta nos va a mandar de vuelta de nuevo continuamente a la pantalla de login aunque pongamos bien los credenciales porque el sistema operativo, al no ser este servicio propio de linux parece que no es capaz de cerrar sesión y transferirla al ordenador que se está conectando pero si nosotros mismos la cerramos antes de intentar conectarnos será imposible ya que sin sesión abierta el servicio no se está ejecutando.

La solución para esto es sencilla y por la estructura de mi máquina no es dificil de solucionar a pesar de que descubrir esta solución me llevó bastante tiempo. Tendremos que iniciar sesión desde una cuenta secundaria del propio servidor, en este caso he usado la cuenta que había creado para el servidor de Left4Dead2 en la primera entrega de teoría. En este momento el servicio ya está activado y si intentamos entrar de nuevo en la cuenta principal ahora si que entraremos directamente al escritorio de la máquina.

![[Trabajo Teoría Bloque 2/Images/Screenshot_171.png|center|560]]

De esta manera ya sabemos como hacer el proceso de manera local. Ahora bien, ¿cómo puedo usar esto para conectarme desde otra localización fuera de mi red local? La mejor solución que he encontrado es una VPN, de esta manera solo los ordenadores que yo tenga en la misma podrán acceder remotamente a mi servidor como bien podemos comprobar en algunos servicios de la Universidad de Oviedo que solo son accesibles desde la propia red interna de la universidad.

Esto me evita de posibles problemas de accesos indeseados que podrían suceder si por ejemplo abro puertos en el router y sigo un proceso similar al de los servidores pero para el servicio rdp. Para este uso tengo disponible la propia red VPN que había creado para el primer trabajo de teoría, hecha con el programa Hamachi que me permite tener varios ordenadores dentro de una red virtual protegida con contraseña. Así de esta manera solo los ordenadores dentro de la red podrán acceder a la máquina, ni siquiera teniendo la ip de Hamachi de la misma podrías acceder si no estás en la red VPN.

Entonces tras comentar esto procedo a entrar al servicio RDP en Windows pero en este caso usaré la IP que me aparece para la máquina AlmaLinux en Hamachi.

![[Trabajo Teoría Bloque 2/Images/Screenshot_172.png|center|560]]

![[Trabajo Teoría Bloque 2/Images/Screenshot_173.png|center|560]]

Como se puede ver el proceso y resultado es el mismo pero en este caso podemos ver que la ip superior es la del Hamachi en lugar de la ip local que aparecía anteriormente. Este ha sido un recurso esencial que me ha permitido gestionar el servidor a pesar de no poder manipularlo físicamente.

<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

## Servidor básico Apache

Tras hacer todo lo anterior ya he terminado de pulir los aspectos más necesarios de los servidores de videojuegos para poder usarlos con amigos y otra gente. Ahora en este punto quería probar a hacer un servidor sencillo de Apache para hostear una página simple de html y javascript que tengo creada.

Todo este proceso lo he hecho simplemente siguiendo el principio del guión de la práctica 7 de laboratorio porque estaba bien explicado y hacer un servidor básico era bastante sencillo.

Primero de todo instalo el servicio httpd.

![[Trabajo Teoría Bloque 2/Images/Screenshot_174.png|center|560]]

Tras hacer esto voy a usar los comandos `systemctl start httpd` y `systemctl enable httpd` para activar y habilitar el servicio en el arranque.

Tras hacer esto voy a la configuración del servicio que se encuentra en /etc/httpd/conf/httpd.conf para poder configurar el servicio. Crearé una carpeta en /var/www/http/ llamada servidorApache.com donde muevo los archivos de mi página web.

En la configuración voy a cambiar la *DocumentRoot* a esta carpeta y también la configuración inferior *Directory* relacionada con /var/www a la nueva ruta lo que hará que el servicio vaya por defecto al archivo index.html que hay en el interior de la carpeta.

Después de esto cambio los permisos del archivo html de la carpeta con chmod 755 –R /var/www/http/servidorApache.com/index.html y reinicio el servicio con chcon -R -h -t httpd_sys_content_t /var/www/http/servidorApache y systemctl restart httpd.

Tras hacer esto si entro desde la máquina linux a localhost podré ver la página.

![[Trabajo Teoría Bloque 2/Images/Screenshot_175.png|center|560]]

También si desde otro ordenador de la misma red local accedo a la ip de la máquina `192.168.1.130` usando el puerto `80` podré entrar a la página.

![[Trabajo Teoría Bloque 2/Images/Screenshot_176.png|center|560]]

Y la última forma de entrar tal cual está ahora configurado sería estando en la VPN de la máquina como se puede ver aquí abajo.

![[Trabajo Teoría Bloque 2/Images/Screenshot_177.png|center|560]]

Es un servicio simple pero quería configurarlo para poder tener disponible esta página web simple que he creado que me muestra los precios de la Gasolina en este caso en Asturias y se va actualizando periódicamente.

















