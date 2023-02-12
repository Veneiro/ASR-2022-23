# Primera parte: Instalación Linux

Primero de todo entro en la aplicación de Virtual Box que será la usada para crear todas las máquinas virtuales de las prácticas.

Después de esto me dirijo al botón de añadir y a crear máquina virtual. En este menú que vemos aquí selecciono el tipo de máquina linux y la versión que usaremos, en este caso RedHat 64bit.

<img src="./Images/1.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Al darle a siguiente vemos como inicia el proceso de instalación, en este caso seleccionamos el tamaño de memoria RAM a 2048 MB (2GB).

<img src="./Images/2.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Para esta máquina creamos un disco duro virtual de 8 GB que será suficiente para lo que la vamos a usar y seleccionamos el tamaño dinámico para evitar ocupar espacio innecesario en el equipo.

<img src="./Images/3.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Tras crear la máquina virtual se nos pide activar el EFI para lo cual me dirijo al apartado de Sistema de la maquina virtual recién creada y marco la checkbox correspondiente.

<img src="./Images/4.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

En el apartado de almacenamiento inserto en la unidad óptica la iso que se nos provee en el campus, en este caso es AlmaLinux, una version gratuita de RedHat.

<img src="./Images/5.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Tras arrancar la máquina comienzo con el proceso de creación de la máquina, selecciono el idioma español y continuo.

<img src="./Images/6.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Para completar la instalación selecciono la instalación mínima que me proporcionará la funcionalidad básica necesaria.

<img src="./Images/7.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Tras terminar la instalación e iniciar la máquina ya tendremos el sistema en funcionamiento, en las siguientes imágenes tenemos el proceso en el que me logeo en la máquina hasta tener mi cuenta en perfecto funcionamiento.

<img src="./Images/17.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/13.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/18.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Utilizo el comando nmcli para comprobar si estoy conectado a la red enp0s3 y como se puede ver en verde en la captura inferior está funcionando perfectamente.

<img src="./Images/14.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Por último utilizo el comando dnf -y upgrade para actualizar la instalación y el kernel, teniendo que reiniciar después de esto.

<img src="./Images/16.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

# Segunda Parte: Instalación de Windows Server 2022

Tras tener la máquina de linux creada procedo a crear la máquina virtual de Windows Server 2022. Selecciono el tipo de máquina Windows y la versión a Other Windows 64bits ya que no disponemos de la versión de 2022 específica.

<img src="./Images/8.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Asignamos la memoria necesaria, en este caso con 4 gigas para la ram será suficiente.

<img src="./Images/9.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Con todo esto ya tendremos nuestra máquina virtual creada y lista para su uso como podemos ver en la captura inferior

<img src="./Images/10.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Al igual que hicimos con la máquina virtual de linux vamos a ir a la configuración y en sistema activaremos el EFI

<img src="./Images/11.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Inserto en la controladora IDE la ISO de Windows Server 2022 que he descargado para poder instalar el sistema operativo

<img src="./Images/12.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Al iniciar la máquina virtual se nos mostrará la vista de instalación de Windows, seleccionamos el idioma y distribución en Español y realizamos la instalación normal del sistema, que es similar a la de un sistema Windows 10 común.

<img src="./Images/15.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Al entrar a la máquina veremos que se nos inicia el programa propio del sistema operativo de servidores de Windows, el Administrador de Servidor, este nos permitirá gestionar toda la configuración de nuestro servidor.

<img src="./Images/19.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Y como se nos dice el la documentación asignamos el nombre y el grupo de trabajo a los que se piden, WS2022 y AS

<img src="./Images/61.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

#
#

# Tercera parte : Instalación de la máquina virtual en la nube (Azure)

Entramos al apartado para crear nuestra máquina virtual

<img src="./Images/20.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Hacemos toda la configuración de esta entre lo que están las etiquetas de los miembros que participan en este ejercicio, en este caso solo estaría una persona.

<img src="./Images/62.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Por último ya tendríamos la máquina correctamente creada, solo quedaría la conexción mediante RDP.

<img src="./Images/21.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Aquí vemos como me puedo conectar perfectamente mediante RDP sólamente descargandome el archivo que me proporciona la propia página de Azure.

<img src="./Images/22.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

# Cuarta parte : Iniciar sesión Linux

## 1 - Cambio de prompt y cambio de nombre del host

En esta primera parte se nos pide cambiar el color y nombre del prompt para identificarnos en las capturas de pantalla. Como se puede ver en la siguinte captura he añadido al archivo correspondiente la línea al final del export PS1 para poder realizar este cambio.

<img src="./Images/23.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Al realizar este primer cambio que se nos indica en la documentación lo primero que vemos es que el color del usuario ha cambiado a localhost en naranja, he de aclarar que la primera vez que hize estos cambios los hice en una cuenta dentro de la máquina que se llamaba ya con mi UO y no era la root pero más adelante ya se ve en capturas que uso la cuenta root con todos estos cambios.


<img src="./Images/24.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

En esta captura que es de un ejercicio posterior se ve el resultado final que obtuve tras realizar el cambio ya sí en la cuenta root aunque no es una cosa que afecte para reconocer mis capturas ya que las primeras capturas antes de darme cuenta de este detalle salen igualmente con mi UO que era el nombre de la propia cuenta que estaba usando.

<img src="./Images/31.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Por último procedo a cambiar con el comando hostnamectl el nombre que se me pide del hostname a linux.as.local lo cual que puede ver justo debajo de **AUTHENTICATION COMPLETE** en la captura. 

<img src="./Images/25.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## 2 - systemd

Como podemos ver, por defecto la máquina se encuentra en target multi-user

<img src="./Images/36.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

En caso de cambiar el target con el comando systemclt isolate lo obtenido en el comando anterior cambiaría. Por ejemplo en el primer caso probamos a activar el modo de rescate con systemctl isolte rescue.target y tras reiniciar acabamos en el modo de rescate como se ve en la captura inferior.

<img src="./Images/26.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Tras volver al modo multiusuario con systemctl isolate multi-user.target pruebo a cambiar al target runlevel6, en este caso lo que ocurre es que se reinicia el sistema. La imagen que muestro a continuación ocurrió la primera vez que lo intente que el sistema se quedó completamente colapsado pero tras reiniciar a la fuerza e intentar usar el runlevel6 otra vez comprobé que lo único que hace es reiniciar el sistema.

<img src="./Images/27.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

En la siguiente captura vemos el PID del proceso systemd que en este caso es el PID 1

<img src="./Images/28.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Si utilizo el comando **who -a** me confirma mediante consola que el nivel por defecto del sistema es el **runlevel3**

<img src="./Images/37.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

En cuanto al **runlevel1** si lo comprobamos la máquina lo único que hace es inicarse en modo rescue por lo que para entrar en este modo valdría lo mismo usar el **rescue.target** o el **runlevel1.**

<img src="./Images/38.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Como he comentado antes, el **runlevel6** lo que nos hace es reinicarnos por completo el sistema únicamente.

## 3 - syslog

En este caso no ha sido necesario instalar el rsyslog ya que ya estaba instalado correctamente como se puede ver en la captura.

<img src="./Images/29.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

De seguido procedo a iniciar el proceso y habilitarlo para que se inicie con el sistema, esto me ha conllevado algún problema ya que me salía un error debido a que rc.local no era reconocido como un archivo ejecutable y por lo tanto no podía hacer el enable pero lo he solucionado sin problema con el comando *sudo chmod +x /etc/rclocal*.

<img src="./Images/39.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/40.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## 4 - Login desde terminales

En este punto procedí a hacer el kill desde la segunda máquina instancia de la máquina y como se comenta en el guión la sesión de la primera máquina se me cerró por completo, como se puede ver en la captura hago el kill del proceso de PID 1186 que es el de la primera máquina

<img src="./Images/30.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

En cuanto a buscar el login del usuario tras buscar por el documento he encontrado este lugar donde pone Started User Login Management por lo que entiendo que aquí es donde comienza el proceso para el login del usuario

<img src="./Images/41.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Aquí realizo el comando last para ver los login y caídas de los sistemas y podemos ver que este kill el last lo detecta como un *crash*

<img src="./Images/31.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## 5 - Ejecución periódica de comandos

Aquí muestro la captura de pantalla con los scripts de ejecución del cron

<img src="./Images/32.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## 6 - Login desde red

El paquete ssh estaba instalado correctamente. En este caso probé a hacer el ssh a la máquina que tenía en la otra sesión y tras comprobar la lista de procesos por aquí se puede ver los procesos ssh que había ejecutandose. En la documentación se refieren al segundo proceso ssh por lo que entiendo que es el segundo que aparece en la captura de pantalla, en este caso este se refiere a un terminal privado como podemos ver y el último que aparece se refiere al terminal pts/0

<img src="./Images/33.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## 7 - Sistemas de ficheros en red

En este punto solo debíamos buscar información acerca de SAMBA por lo que he usado tanto la página proporcionada como el comando man para encontrar información y saber bien como funcionaba y lo que hacía. Como resumen podríamos definir a SAMBA como el conjunto estándar de programass de interoperabilidad de Windows para Linux y Unix, siendo este un componente importante para integrar perfectamente servidores y escritorios Linux y Unix.

<img src="./Images/34.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## 8 - Correo electrónico

Aquí se puede ver como la comunicación entre las dos instancias de la máquina funciona perfectamente enviando correos de una a la otra.

<img src="./Images/35.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Si lo que queremos es salir de la interfaz s-nail tenemos dos opciones, salir sin guardar los cambios para lo que usaremos el comando *xit o exit* o salir guardando todos los cambios hechos en la bandeja de entrada para o que usaremos *quit*

<img src="./Images/42.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## 9 - Servicios de impresión

En este apartado tenía que buscar información acerca de CUPS, el cual es un sistema de impresión desarrollado por Apple para sus dispositivos para imprimir en impresoras tanto locales como en red.


# Ejercicios Opcionales

## 1) Nueva máquina virtual con GUI

En este primer punto tengo que crear la máquina virtual con GUI usando la misma ISO que en el primer apartado de los ejercicios obligatorios. En las siguientes 4 imágenes se ve toda la configuración inicial seleccionada, el comienzo de la instalación y por último ya la máquina dentro del entorno con todo actualizado y finalizado.

<img src="./Images/43.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/44.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/45.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/46.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/47.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/48.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## 2) Documentación y ayuda

#### Ejecuta el comando mandb

Primero introduzco el comando mandb y en la imágen inferior podemos ver lo que hace, actualiza la base de datos del manual eliminando entradas antiguas si es necesario.

<img src="./Images/49.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

#### Usa las órdenes man 

Ahora procedo a buscar información de las órdenes whatis y apropos como se me dice en la documentación. Las dos siguientes capturas son primero de la información que se muestra de whatis con el man y la segunda del apropos.

<img src="./Images/50.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

<img src="./Images/51.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Aquí también podemos ver las órdenes del sistema que hacen referencia a la órden de reboot

<img src="./Images/52.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

#### Explica qué hace el comando cd /usr/bin; ls | xargs whatis | less

Este comando nos muestra una lista ordenada alfabéticamente de todas las órdenes disponibles en el sistema, nos dicee exáctamente que hace cada una y con el less nos permite hacer un "scroll" a través de esta lista.

<img src="./Images/53.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## Conceptos básicos de administración de paquetes

####  Haz una lista de todos los paquetes del sistema, cuenta cuántos hay con wc

Hay un total de 1510 paquetes

<img src="./Images/54.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Al comprobar cuantos paquetes estaban actualizados en la máquina no he obtenido ningún resultado, esto es debido a que ya estaba actualizados todos desde otro ejercicio previo.

<img src="./Images/55.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Como se me dice instalo el emacs con el comando yum install y se instala sin problemas

<img src="./Images/56.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## Opciones del kernel. Mostrar la versión del kernel

Primero con el comando grep y buscando el término name encontré una serie de comandos que podían proporcionarme la información que necesitaba

<img src="./Images/57.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Tras informarme sobre los mismo decidí usar el comando uname, a continuación muestro la información que porporcionaba el man

<img src="./Images/59.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

Con la información obtenida del man vemos que para sacar la version del kernel hay que usar la opción --kernel-version del comando uname la cual nos muestra la versión del mismo

<img src="./Images/58.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">

## Mensaje de presentación /etc/motd, /etc/issue

Aquí se puede ver el resultado tras editar los archivos de motd e issue

<img src="./Images/60.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 100%;">
