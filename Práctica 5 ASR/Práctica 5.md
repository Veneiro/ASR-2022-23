`Hecho por Mateo Rico Iglesias - UO277172`
## Primera Parte: Conectividad
- En este primer punto he configurado ambas máqinas Windows para que utilicen red interna en su adaptador de red además de comprobar que la configuración en el propio sistema operativo ya estaba configurada de tal manera que se proporcionan direcciónes de manera automática.
![[Screenshot_77.png]]![[Screenshot_78.png]]
- Después he configurado la máquina Linux para tener dos adaptadores de red, el tipo ``NAT`` y el tipo ``red interna``
![[Screenshot_79.png]]![[Screenshot_80.png]]
1. Tras realizar los pasos anteriores he entrado a las máquinas Windows y he comprobado la ip con el comando ``ipconfig``. ![[Screenshot_81.png]]
	1. ¿Tiene asociadas DNS, puerta de enlace y ruta por defecto?
		No, solo tiene asociada una ip por defecto, los otros dos campos aparecen vacíos
	2. ¿Puedes acceder desde ella a máquinas de la red local de la universidad?
		En este caso no me dejaría.
	3. ¿Y a las máquinas virtuales de Windows 10 y Linux?
		Puedo acceder entre las máquinas de windows pero no con la de linux
	4. ¿Por qué?
		En este caso las máquimas Windows me permite realizar el ping debido a que el ordenador asigna automáticamente una dirección ip para la red interna. En cambio la máquina linux por defecto no está configurada por lo que aunque obtengamos información de los adaptadores de red el adaptador relacionado con la red interna no está configurado y aparece sin ip. En cuanto a la conexión al exterior de esta red interna es normal que no funcione, inluso en el propio windows en el símbolo de buscar redes nos aparece el icono de que no tenemos internet debido a que nuestro sistema solo cuenta con una ip para la red interna unicamente.
		![[Screenshot_82.png]]
		![[Screenshot_83.png]]
2. Al usar la orden ``nmcli`` o ``ip addr`` en ambos casos podemos ver que el adaptador de NAT sale configurado y con una ip en cambio el adaptador de la red interna sale sin una configuración. Esto se debe a que en linux esta configuración se debe de hacer a mano a diferencia de windows.![[Screenshot_106.png]]
3. Instalo el paquete ``bind utils`` correctamente y tras hacerlo pruebo a usar el ``nslookup horru.lsi.uniovi.es``. La dirección asociada es *156.35.119.120* y el servidor dns es el *156.35.14.6* que es el servidor de Uniovi.
![[Screenshot_86.png]]
	![[Screenshot_87.png]]
## Segunda Parte: servidor DHCP
Añado la conexión ethernet con el comando ``nmcli connection add type ethernet con-name enp0s8 ifname enp0s8 ipv4.method manual ipv4.address 192.168.56.100/24`` y me sale ya añadida.
	![[Screenshot_88.png]]
	![[Screenshot_89.png]]
	Después de esto borro la conexión cableada que me aparece y recargo la configuración. Tras repetir las órdenes del punto 2 el resultado es el siguiente:
	![[Screenshot_90.png]]
	![[Screenshot_91.png]]
	Instalo seguidamente de esto el servidor dhcp con el comando ``dnf -y install dhcp-server``
	![[Screenshot_96.png]]
	Habilito el servicio con ``systemctl enable --now dhcpd.service``
	![[Screenshot_95.png]]
	Y vemos que si vamos al log aparece que se ha iniciado el servicio
	![[Screenshot_97.png]]
	Al ir a las máquinas windows podemos ver que las ip ya toman ip en el rango correcto e incluso vemos que aparece el servidor dhcp del que están cogiendo las ip. A pesar de esto las máquinas siguen sin tener conexión con el exterior pero sí con la máquina linux ya que ahora esta máquina si que tiene una ip asignada. Estas primeras no tienen conexión a internet porque poseen solo una red interna conectada al dhcp linux pero no tienen una conexión NAT propia por lo que hasta que la máquina que les provee de ip configure este acceso al exterior no podrán acceder a internet.
	![[Screenshot_98.png]]
	![[Screenshot_99.png]]
4. A pesar de esto las máquinas aún no pueden resolver el nombre de ``horru.lsi.uniovi.es`` porque no tienen un servidor dns. Puede que esto se pudiera solucionar accediendo al archivo hosts que tiene el propio windows, este permite asignar ip a nombres de dominio y suele ser muy usado en seguridad para asignar a ciertos sitios maliciosos la ip por defecto 0.0.0.0 para que no puedan dañar a la máquina.
5. En este punto edito el archivo dhcpd.conf y añado la línea ``option domain-name-servers 156.35.14.2``
![[Screenshot_107.png]]
6. A pesar de tener el dns asignado no pueden resolver aún ``www.google.es`` principalente porque la máquina que tiene la red NAT es la linux pero no tiene añadidas las reglas del firewall necesarias para permitir el tráfico de datos desde las máquinas Windows a la red. Esto es algo que se cambiará en el siguiente apartado
## Tercera parte: Uso de Linux como enrutador
7. Primero trato de ejecutar ``sysctl net.ipv4.ip_forward`` y obtengo un 0 como salida. Tras modificar el archivo ``50-router.conf`` y añadir la línea ``net.ipv4.ip_forward=1`` y volver a probar el mismo comando ya obtengo la salida esperada, que es un 1.
![[Screenshot_102.png]]
8. Después de esto simplemente ejecuto los comandos de firewall-cmd que se me pide lo que nos permitirá abrir los puertos necesarios para que el segundo adaptador de red pare a la zona de confianza del cortafuegos
![[Screenshot_100.png]]
![[Screenshot_101.png]]
9. Esto nos permite ya si por fin hacer ping a ``156.35.119.120`` desde la máquina linux y hacer ping a ``www.google.es`` desde ambas máquinas windows. Cabe por supuesto que si la máquina linux se apaga las otras dos, al estar accediendo a través suyo a internet se quedarían sin poder acceder a la web, podríamos decir que la máquina linux estaría ejerciciendo de "router" para estas máquinas.
![[Screenshot_103.png]]
![[Screenshot_104.png]]
![[Screenshot_105.png]]
10. ![[Screenshot_108.png]]