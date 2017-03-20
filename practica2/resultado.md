# Práctica 2
Lo primero que haremos será anotar las direcciones IPs de las máquinas. Para ello ejectuamos el comando `ifconfig` en ambas máquinas. En la máquina 1 obtenemos:

![Ejecución de ifconfig: máquina 1](ifconfig1.png "Ejecución de ifconfig: máquina 1")

Y en la máquina 2:

![Ejecución de ifconfig: máquina 2](ifconfig2.png "Ejecución de ifconfig: máquina 2")

Una vez conocidas las IPs, es conveniente destacar que en esta práctica nuestra máquina de trabajo será la máquina 2 (identificada por **m2**) por lo general. Ésta será la que se encargue de duplicar exactamente el contenido que nos interese de la máquina 1 (identificada por **m1**).

Para configurar la red privada de las máquinas virtuales y la anfitriona, vamos al menú de *Virtualbox*, *Archivo* y elegimos el submenú *Preferencias*. Una vez aquí, seleccionamos la pestaña de *Red* y a su vez, la pestaña *Redes solo-anfitrión*. A continuación, hacemos click en el icono a la derecha con el símbolo +, tal y como se muestra en la siguiente figura:

![Añadiendo red](vbox.png "Añadiendo red")

Por defecto, se nos creará la red *vboxnet0*, como se muestra en la figura anterior. Editamos la red pulsando en el tercer botón a la derecha (figura de un destornillador). Rellenamos los campos tal y como se muestran en la siguiente imagen (el campo de *dirección IPV4* podemos configurarlo con la dirección IP que deseemos): 

![Configurando red](adaptador.png "Configurando red")

Una vez hecho esto, nos vamos a la configuración de red de cada una de nuestras máquinas virtuales. Aquí le indicaremos que necesitamos dos adaptadores de red para cada máquina (por simplicidad del tutorial pondré la configuración de una sola máquina pero es importante hacerlo en todas las que queramos que funcionen en esta red privada): uno *NAT* (es el que viene por defecto) y le añadimos un segundo adaptador, donde le diremos que será un *Adaptador solo-anfitrión* y en el campo de nombre ponemos la red anteriormente creada *vboxnet0*. Este proceso se muestra en las siguiente figuras:

![Adaptador NAT](NAT.png "Adaptador NAT")

![Adaptador solo-anfitrión](anfitrion.png "Adaptador solo-anfitrión")


Así pues, comprobamos que ambas máquinas se ven entre sí mediante la utilidad `ping`. En la máquina 1:

![Ejecución de ping: máquina 1](ping1.png "Ejecución de ping: máquina 1")

En la máquina 2:

![Ejecución de ping: máquina 2](ping2.png "Ejecución de ping: máquina 2")

Además, también se pueden ver con la anfitriona. En las siguiente imágenes se ve como accedemos desde la anfitriona a cada una de las máquinas, comprobando que *Apache* funciona:

![Apache máquina 1](apache1.png "Apache máquina 1")

![Apache máquina 2](apache2.png "Apache máquina 2")


Personalmente, he decidido trabajar siempre como **root**, por tanto debemos configurar el fichero */etc/ssh/sshd_config* de la máquina 1, ya que será nuestro servidor en producción. Modificaremos el parámetro *PermitRootLogin* tal y como se muestra en la siguiente figura:

![Configurando ssh](root.png "Configurando ssh")
A continuación, reiniciamos el servicio con el comando `service restart ssh`.

Comprobamos que, efectivamente, podemos conectarnos como root por ssh desde la máquina 2 a la máquina 1:

![Conexión ssh](ssh.png "Conexión ssh")

Lo siguiente a comprobar es el clonado de un directorio de la máquina 1 (*/var/www*, por ejemplo) a la máquina 2 mediante *rsync*. Para ello, crearemos un archivo de prueba en la máquina 1 en */var/www/html/hola.html*:

![Archivo de prueba .html](holahtml.png "Archivo de prueba .html")

Y ejecutamos `ls` en la máquina 2 para comprobar que efectivamente no existe ese archivo en la máquina 2:

![Ejecutando ls](lsvacio.png "Ejecutando ls")

Ahora, ejecutamos en la máquina 2 el siguiente comando `rsync -avz -e ssh root@192.168.1.55:/var/www/ /var/www/`, que será el encargado de clonar el directorio, y por consecuente, el nuevo archivo que hemos creado de prueba (*hola.html*):

![rsync](rsync.png "rsync")

Como hemos visto, cada vez que usamos *rsync*, debemos introducir la contraseña de la máquina 1, ya que utiliza *ssh*. Así pues, configuraremos *ssh* para que no nos pida más la contraseña y poder automatizar el proceso de clonado.

Lo primero que haremos será ejecutar el comando `ssh-keygen -t dsa` para generar el par de claves pública-privada en la máquina 2. Una vez hecho esto, tendremos que copiar la clave pública a la máquina 1. Lo hacemos ejecutando el comando `ssh-copy-id -i -ssh/id_dsa.pub root@192.168.1.55`. Todo el proceso se encuentra descrito en la siguiente imagen:

![Claves](claves.png "Claves")

A continuación, comprobamos que podemos conectarnos a la máquina 1 sin introducir la contraseña:

![ssh sin password](sshsinpassword.png "ssh sin password")

Por último, configuraremos *crontab* para automatizar la ejecución del comando `rsync -avz -e ssh root@192.168.1.55:/var/www/ /var/www/`.
Así pues, editamos el archivo */etc/crontab* y le añadimos lo siguiente: `* * * * * root rsync -avz -e ssh root@192.168.1.55:/var/www/ /var/www/`. Esto permitirá que se ejecute el comando automáticamente cada minuto. 

![Configuración crontab](comandocrontab.png "Configuración crontab")

Para la prueba, borraremos el fichero */var/www/html/hola.html* de la máquina 2 creado anteriormente, y veremos como se sincroniza automáticamente.

![Clonado automático](clonadoaut.png "Clonado automático")