# Primera parte: Instalación Linux

Primero de todo entro en la aplicación de Virtual Box que será la usada para crear todas las máquinas virtuales de las prácticas.

Después de esto me dirijo al botón de añadir y a crear máquina virtual. En este menú que vemos aquí selecciono el tipo de máquina linux y la versión que usaremos, en este caso RedHat 64bit.

<img src="./Images/1.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Al darle a siguiente vemos como inicia el proceso de instalación, en este caso seleccionamos el tamaño de memoria RAM a 2048 MB (2GB).

<img src="./Images/2.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Para esta máquina creamos un disco duro virtual de 8 GB que será suficiente para lo que la vamos a usar y seleccionamos el tamaño dinámico para evitar ocupar espacio innecesario en el equipo.

<img src="./Images/3.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Tras crear la máquina virtual se nos pide activar el EFI para lo cual me dirijo al apartado de Sistema de la maquina virtual recién creada y marco la checkbox correspondiente.

<img src="./Images/4.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

En el apartado de almacenamiento inserto en la unidad óptica la iso que se nos provee en el campus, en este caso es AlmaLinux, una version gratuita de RedHat.

<img src="./Images/5.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Tras arrancar la máquina comienzo con el proceso de creación de la máquina, selecciono el idioma español y continuo.

<img src="./Images/6.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Para completar la instalación selecciono la instalación mínima que me proporcionará la funcionalidad básica necesaria.

<img src="./Images/7.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Tras terminar la instalación e iniciar la máquina ya tendremos el sistema en funcionamiento, en las siguientes imágenes tenemos el proceso en el que me logeo en la máquina hasta tener mi cuenta en perfecto funcionamiento.

<img src="./Images/17.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

<img src="./Images/13.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

<img src="./Images/18.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Utilizo el comando nmcli para comprobar si estoy conectado a la red enp0s3 y como se puede ver en verde en la captura inferior está funcionando perfectamente.

<img src="./Images/14.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Por último utilizo el comando dnf -y upgrade para actualizar la instalación y el kernel, teniendo que reiniciar después de esto.

<img src="./Images/16.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

# Segunda Parte: Instalación de Windows Server 2022

Tras tener la máquina de linux creada procedo a crear la máquina virtual de Windows Server 2022. Selecciono el tipo de máquina Windows y la versión a Other Windows 64bits ya que no disponemos de la versión de 2022 específica.

<img src="./Images/8.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Asignamos la memoria necesaria, en este caso con 4 gigas para la ram será suficiente.

<img src="./Images/9.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Con todo esto ya tendremos nuestra máquina virtual creada y lista para su uso como podemos ver en la captura inferior

<img src="./Images/10.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Al igual que hicimos con la máquina virtual de linux vamos a ir a la configuración y en sistema activaremos el EFI

<img src="./Images/11.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Inserto en la controladora IDE la ISO de Windows Server 2022 que he descargado para poder instalar el sistema operativo

<img src="./Images/12.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Al iniciar la máquina virtual se nos mostrará la vista de instalación de Windows, seleccionamos el idioma y distribución en Español y realizamos la instalación normal del sistema, que es similar a la de un sistema Windows 10 común.

<img src="./Images/15.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

Al entrar a la máquina veremos que se nos inicia el programa propio del sistema operativo de servidores de Windows, el Administrador de Servidor, este nos permitirá gestionar toda la configuración de nuestro servidor.

<img src="./Images/19.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">

# Cuarta parte : Instalación de la máquina virtual en la nube (Azure)



<img src="./Images/20.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/21.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/22.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/23.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/24.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/25.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/26.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/27.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/28.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/29.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/30.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/31.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/32.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/33.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/34.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">



<img src="./Images/35.PNG" style="display: block; 
           margin-left: auto;
           margin-right: auto;
           width: 90%;">