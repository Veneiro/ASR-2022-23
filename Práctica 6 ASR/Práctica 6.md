## Primera parte: Servidor DHCP en Windows
1. Primero apago todas las máquinas dejando solo la linux en funcionamiento y desinstalo el servicio dhcp de la máquina
	![[Screenshot_109.png]]
	![[Screenshot_110.png]]

2. Desde la máquina Windows Server podemos ver que ahora no podemos hacer ``ping`` o ``curl`` a ``www.google.es``. La dirección ipv4 está puesta por defecto y la puerta de enlace predeterminada y dns ya no aparecen.
	![[Screenshot_111.png]]
	En el *centro de redes y recursos compartidos* en la configuración de ipv4 asigno una dirección ip, puerta de enlace y dns estáticos.
	![[Screenshot_112.png]]
	Y ahora ya puedo hacer ``ping`` y ``curl`` a ``www.google.es``
3. Añado en el *Administrador del Servidor* el rol de **Servidor DHCP** 
	![[Screenshot_113.png]]
	![[Screenshot_114.png]]
	![[Screenshot_115.png]]
	Desde el apartado de *Herramientas* entrando en el nuevo servicio *DHCP* voy a la sección *IPV4* y creo un nuevo ámbito desde el menú *Acción*. Sigo todos los pasos de la creación configurando este nuevo ámbito como se me indica en la documentación de la prática
	![[Screenshot_116.png]]
	![[Screenshot_117.png]]
	![[Screenshot_118.png]]
	![[Screenshot_119.png]]
4. Si arranco la máquina W10 podemos ver que si utilizo el comando ``ipconfig`` la máquina tiene ya la configuración asignada
	![[Screenshot_120.png]]
	Con todo esto también tenemos acceso directo al ``ping www.google.es``. Esto se debe principalmente a que ya teníamos la red de máquinas creadas anteriormente y la máquina Windows 10 estaba preparada para configurar su ip mediante DHCP por lo que ya busca automáticamente un DHCP dentro de la red creada, encontrando el Windows Server como servidor DHCP de la red.
5. Si entramos a la máquina Windows Sever dentro del mismo apartado *IPV4* de antes, en el apartado de *Concesiones de direcciones* podemos ver que ya aparece la máquina Windows 10.

## Segunda parte: Servidor DNS en Windows
Agrego antes de nada la máquina Windows 10 en la configuración del DHCP a las reservas por su IP y su MAC
![[Screenshot_122.png]]
![[Screenshot_121.png]]
1. Configuro el DNS en la máquina Windows Server al igual que hice antes añadiendo el rol DNS
	![[Screenshot_123.png]]
	![[Screenshot_124.png]]
	![[Screenshot_125.png]]
2. Desde las herramientas del DNS creo primero una nueva zona directa y la nueva zona inversa con la ipv4 que se me pide, en este caso la ``192.168.56.`` y marco la opción de *Crear registro del puntero (PTR) asociado* que me crea automáticamente los punteros para no tener que crearlos a mano.
	![[Screenshot_126.png]]
	![[Screenshot_127.png]]
	Aquí podemos ver que la zona ya se ha creado y que aparecen tanto los host creados por mi como los punteros que se generan automáticamente
	![[Screenshot_128.png]]
	![[Screenshot_129.png]]
	![[Screenshot_130.png]]
	Añado el reenviador no condicionado con el ip 1.1.1.1 para que las máquinas puedan acceder a google
	![[Screenshot_131.png]]
3. Tras todo esto cambio la configuración en las otras máquinas para usar la nueva configuración. La máquina linux se configura de manera automática pero para configurar la máquina linux debo hacer lo siguiente.
	Primero el comando ``nmcli con modify enp0s8 ipv4.dns 192.168.56.101`` para configurar la ip del dns
	![[Screenshot_132.png]]
	Después el comando ``nmcli con modify enp0s8 ipv4.dns-priority 5`` y ``nmcli con modify enp0s3 ipv4.dns-priority 0`` para cambiar las prioridades para que actue el nuevo servidor DNS
	![[Screenshot_133.png]]
	Con ``nmcli con modify enp0s8 ipv4.dns-search as.local`` cambio el dominio de búsqueda por defecto
	![[Screenshot_134.png]]
	Y por último reinicio las conexiones para aplicar los cambios
	![[Screenshot_135.png]]

## Tercera parte: Servidor NAS en Linux y Windows
Primero añado los usuarios como se me pide en la práctica, en linux añado un usuario samba y en Windows simplemente lo añado desde el menu llamado *Otros usuarios*.
![[Screenshot_140.png]]
![[Screenshot_147.png]]
Ejecuté el comando que se comenta en la práctica y me da el siguiente output.
![[Screenshot_139.png]]
Después entro a la configuración de samba y en este caso solo necesito cambiar en el apartado de homes *browseable* a **Yes**
![[Screenshot_142.png]]
Inicio el servicio de samba y ejecuto los comandos del firewall necesarios para admitir conexiones por samba, no se ve en la captura pero después de esto reinicié el firewall con ``sudo firewall-cmd --reload`` para que se aplicaran correctamente las nuevas normas.
![[Screenshot_143.png]]
![[Screenshot_144.png]]
En Windows permito compartir archivos y además como se me comentaba en la práctica activo la opción de compartir para todos de la carpeta del nuevo usuario.
![[Screenshot_138.png]]
![[Screenshot_148.png]]
Y por último simplemente accedo a la máquina Windows 10, presiono la combinación de teclas ``Windows + R`` y en el cuadro de ejecutar que me sale pego cada una de las direcciones que se me menciona en la prática que son las de las carpetas que acabo de compartir.
![[Screenshot_145.png]]
![[Screenshot_146.png]]