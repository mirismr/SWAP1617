# Ejericio 3
## ¿Cómo analizar el nivel de carga de cada uno de los subsistemas en el servidor? Buscar herramientas y aprender a usarlas.

### Directorio /proc
Es una carpeta en la memoria RAM que usarán los distintos monitores que describiremos a continuación para acceder a información global sobre el sistema operativo.

### *uptime*
*uptime* da una línea de información que contiene lo siguiente: la hora actual, el tiempo en marcha, usuarios logeados y la carga media en el último minuto, los últimos 5 minutos y los últimos 15 minutos.

### *ps*
*ps (Process Status)* nos ofrece información sobre el estado actual de los procesos del sistema. Posee una gran cantidad de parámetros, que se pueden conocer cómodamente a través del manual en línea de la consola de comandos (*man*). Estos parámetros nos ayudan a filtrar la información según lo que nos interese conocer. 

La información que aporta **ps** es: usuario que lanzo el proceso (*USER*), porcentaje de procesador y memoria física usada por cada proceso (*%CPU* y *%MEM*, respectivamente), memoria virtual total asignada al proceso en KB (*SIZE*), memoria física ocupada por el proceso en KB (*RSS*) y el estado del proceso (*STAT*).

### *top*
Muestra cada *x* segundos ciertas características de la máquina. Es interactivo, ya que se puede cambiar cada cuántos segundos se quiere que actualice la información, cómo ordenar las filas, etc. Muestra los *PID* de los procesos, cuántos hay, el estado de los procesos, medidas de la CPU, medidas de la memoria, etc. Es un monitor muy completo.

### *vmstat*
*vmstat* (*Virtual memory statistics*) muestra información acerca de la memoria virtual del sistema: paginación, swapping, interrupciones por segundo, CPU, estados de procesos, bloques por segundo transmitidos (de entrada y de salida), KB/s entre memoria y disco (de entrada y de salida) y cambios de contexto. Con otros argumentos (podemos observar el *man*) puede dar otro tipo de información como acceso a discos (partición de swap) u otras estadísticas de memoria.

### *sar*
*sar* (*System activity reporter*) es un monitor utilizado para la tección de cuellos de botella. Ofrece información sobre todo el sistema: actual e histórica (haciendo uso de ficheros históricos). *sar* se basa en dos órdenes: *sadc* que recoge los datos de */proc* y construye un registro en binario, y *sar*, que lee los registros en binario y los traduce a un formato legible para los humanos. Posee modos interactivo y no interactivo, además de múltiples parámetros que podemos conocerlos a través del *man*.

##Referencias
1. [/proc](http://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/proc.html "/proc")

2. [uptime](https://linux.die.net/man/1/uptime "uptime")

3. [ps](https://linux.die.net/man/1/ps "ps")
 
4. [top](http://man7.org/linux/man-pages/man1/top.1.html "top")

5. [vmstat](https://linux.die.net/man/8/vmstat "vmstat")

6. [sar](https://linux.die.net/man/1/sar "sar")