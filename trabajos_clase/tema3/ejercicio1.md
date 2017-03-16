# Ejercicio 1
## Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra.
### Windows

* Comando *route*
* Para versiones Server podemos incluir el *Servicio de enrutamiento y acceso remoto*

### Linux

* Comando *route* (volátil)
* Archivo /etc/network/interfaces para que las rutas permanezcan aunque apaguemos el servidor.
* Comando *iptables*, activando el enrutamiento de la máquina en */proc/sys/net/ipv4/ip_forward*

## Referencias

1. [route (Windows)](https://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/route.mspx?mfr=true "route (Windows)")
2. [Servicio de enrutamiento (Windows)](https://technet.microsoft.com/en-us/library/8426f09b-6132-4797-9390-60c33206ee3f "Servicio de enrutamiento (Windows)")
3. [route (Linux)](https://linux.die.net/man/8/route "route (Linux)")
4. [iptables (Linux)](https://wiki.archlinux.org/index.php/Iptables "iptables (Linux)")