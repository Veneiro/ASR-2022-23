``Hecho por Mateo Rico Iglesias - UO277172``

### Parte 1: Direcciones de enlace local

Primero, he clonado dos máquinas que tenía ya creadas para empezar con estas de base, una máquina AlmaLinux con GUI y una máquina Windows 10. En ambas he cambiado el adaptador de red y les he puesto uno de red interna con nombre p10.

![[Screenshot_306.png|center|560]]

![[Screenshot_307.png|center|560]]

Uso el siguiente comando en la máquina linux para que se autoconfigure la ipv6 de la máquina.

![[Screenshot_298.png|center|560]]

Y como podemos ver ya se configura correctamente.

![[Screenshot_304.png|center|560]]

En la máquina Windows por las características el sistema operativo ya se configura automáticamente.

![[Screenshot_303.png|center|560]]

Tras hacer esto compruebo si puedo hacer un ping desde la máquina Windows a la Linux y se puede ver que funciona correctamente.

![[Screenshot_302.png|center|560]]


<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Parte 2: Direcciones IPv6 estáticas

En esta parte voy a realizar un proceso similar al de la parte anterior pero en este caso con direcciones ipv6 configuradas de manera manual.

Primero configuro la ipv6 de la máquina linux, como podemos ver aparece correctamente modificada en el archivo de configuración de la red.

![[Screenshot_308.png|center|560]]

![[Screenshot_309.png|center|560]]

Realizo el mismo proceso con la máquina Windows, para hacer esto entraré en el adaptador de red desde el panel de control y modifico la configuración ipv6 del adaptador.

![[Screenshot_310.png|center|560]]

Como podemos ver tanto en Windows como en linux la configuración se ha modificado correctamente. En linux he tenido que usar el ``nmcli con reload``, ``nmcli net off`` y ``nmcli net on`` para que se actualizara correctamente pero al final si ha aparecido.

![[Screenshot_315.png|center|560]]

![[Screenshot_312.png|center|560]]

Tras hacer esto usamos los comandos de route de ambos sistemas y vemos que la dirección **fd00:a:b:c::** aparece en la lista.

![[Screenshot_316.png|center|560]]

![[Screenshot_314.png|center|560]]

Por último, probando a hacer ping, podemos ver que funciona correctamente en ambas direcciones de la conexión.

![[Screenshot_319.png|center|560]]

![[Screenshot_320.png|center|560]]


<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Parte 3: Servidor DHCPv6

En esta parte vamos a configurar el Servidor Linux como DHCP para la máquina Windows. Primero de todo vamos a poner temporalmente el adaptador de red a NAT para poder instalar el servicio dhcp6.

![[Screenshot_321.png|center|560]]

Tras hacer esto vamos a editar la configuración para establecer el rango de ip's en el que se moverá el servidor dhcp.

![[Screenshot_322.png|center|560]]

Permitimos el servicio en el firewall y lo inciamos y habilitamos en la máquina.

![[Screenshot_323.png|center|560]]

Después de esto vamos a la configuración del adaptador de red de la máquina Windows y volvemos a dejar todo puesto en automático.

![[Screenshot_324.png|center|560]]

Como podemos ver la dirección ya se configura correctamente.

![[Screenshot_325.png|center|560]]

Pruebo a renovar la dirección con el comando renew y podemos ver que el servicio dhcp está funcionando.

![[Screenshot_326.png|center|560]]

Y aquí podemos ver la monitorización del servicio desde la máquina Linux.

![[Screenshot_327.png|center|560]]


<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Parte 4: Servidor RADVD (Router ADVertisement Daemon)

Para esta parte crearemos un servidor RADVD en la máquina linux. Primero vuelvo a cambiar el adaptador de red a NAT e instalo el paquete.

![[Screenshot_328.png|center|560]]

Tras hacer esto voy a la configuración del servicio y añado las líneas que se ven abajo del archivo.

![[Screenshot_329.png|center|560]]

Después de esto activo y habilito el servicio con el systemct.

![[Screenshot_330.png|center|560]]

Y, por último, si probamos a realizar el ping entre las máquinas vemos que funciona correctamente.

![[Screenshot_331.png|center|560]]

![[Screenshot_332.png|center|560]]


<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Parte 5: Servidor RADVD y autoconfiguración stateless

Primero paro y desactivo el servicio dhcp6 para comenzar.

![[Screenshot_333.png]]

Reinicio el adaptador de Windows y pruebo a hacer un ``ipconfig /all`` y ``route -6 print`` para comprobar que ya no aparece la ipv6 del servicio dhcp y tampoco las rutas a la máquina linux.

![[Screenshot_334.png|center|560]]

![[Screenshot_335.png|center|560]]

Entro a la configuración del radvdy cambio a **off** el parametro *AdvManagedFlag* y a **on** el parámentro *AdvAutonomous*.

![[Screenshot_336.png|center|560]]

Reinicio el servicio radvd para que se aplique la configuración.

![[Screenshot_337.png|center|560]]

Y como podemos ver ya se aplica la nueva dirección ipv6 y también vuelve a aparecer la nueva ruta en la máquina Windows.

![[Screenshot_339.png|center|560]]

![[Screenshot_340.png|center|560]]

Si probamos a hacer ping a la máquina Linux desde la Windows podemos ver que funciona correctamente.

![[Screenshot_341.png|center|560]]

Para probar voy a eliminar la dirección IPv6 de la máquina Linux, aplico los cambios y lo reinicio.

![[Screenshot_342.png|center|560]]

Si miramos la dirección y las rutas vemos que se ha configurado correctamente.

![[Screenshot_343.png|center|560]]

![[Screenshot_344.png|center|560]]

Y, si usamos el ping en ambas direcciones vemos que todo sigue funcionando correctamente.

![[Screenshot_345.png|center|560]]

![[Screenshot_346.png|center|560]]


<div style="page-break-after: always; visibility: hidden"> \pagebreak </div>

### Parte 6: Servidores Samba, Web y DNS

Para empezar esta primera práctica voy a mostrar la configuración de Samba en la máquina que he usado durante toda esta práctica, pero debido a diversos problemas ocasionados por la misma en relación al servicio de Samba terminaré usando la red de ordenadores que tenía desde la práctica 7 donde también usamos samba.

Primero de nada instalo el servicio en la máquina.

![[Screenshot_347.png|center|560]]

##### Samba

Tras hacer esto configuro un usuario samba, en este caso lo había llamado *uo277172* pero más adelante al haberme cambiado de máquina se verá el *asuser* al igual que lo usaba en la práctica 7, este proceso lo hice con el fin de explicarlo todo.

![[Screenshot_348.png|center|560]]

Tras aplicar las configuraciones que se me pedían en la práctica 6, donde se configuraba el samba, como cambiar la configuración **añadiendo ntlm auth = yes al global** y **poniendo en yes el browseable de homes** además de añadir el samba al firewall con ``firewall-cmd --zone=internal --add service=samba --permanent`` y poner el ``setsebool -P samba_enable_home_dirs on`` ya lo tengo configurado.

Primero pruebo a acceder mediante samba a la propia máquina linux. No he podido conseguir que esto funcione accediendo directamente a la carpeta publicar como se pedía pero simplemente usando el comando cd se podía llegar a esta desde la carpeta base del usuario

![[Screenshot_360.png|center|560]]
![[Screenshot_361.png|center|560]]

Si probamos a conectarnos al sistema Windows funciona de la misma manera.

![[Screenshot_362.png|center|560]]

Si queremos conectarnos desde Windows al Linux pondremos lo siguiente en la ventana ejecutar. Tras pedirnos el usario y contraseña, en este caso el asuser podremos acceder a la carpeta.

![[Screenshot_353.png|center|560]]

![[Screenshot_359.png|center|560]]

##### Web

Como bien se dice en la propia práctica con simplemente poner ``http://[fd00:a:b:c::1]/`` en el navegador nos funciona, en este caso como en esta máquina fue donde estuve con todo el tema de joomla la última vez es lo que se muestra por defecto.

![[Screenshot_363 (edited).png|center|560]]

##### DNS

Si tras hacer todo lo anterior probamos a ejecutar los comandos que se dice en la prática vemos que el resultado del nslookup es el esperado y que las máquinas se pueden ver perfectamente tanto como con ipv4 como con ipv6

![[Screenshot_358.png|center|560]]

![[Screenshot_356.png|center|560]]
































