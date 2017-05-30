# Práctica 5
## Crear base de datos en *MySQL*

Lo primero que haremos será crear la base de datos en *MySQL*. Para ello entramos com usuario *root* con el comando `mysql -uroot -p`, e introducimos la contraseña. A continuación creamos la base de datos y una tabla de ejemplo. Además, introducimos algunos registros tal y como se muestra en la siguiente figura: 

![Crear base de datos](1.png "Crear base de datos")

## Replicar la base de datos con *mysqldump*

Para replicar la base de datos, lo primero que tenemos que hacer es bloquear las tablas para que no se pueda escribir mientras la estamos replicando, para ello se introduce el siguiente comando:

![Bloquear tablas](2.png "Bloquear tablas")

Obviamente, para introducir este comando tenemos que estar en la consola de *mysql*, es decir, entrando con el comando inicial `mysql -uroot -p`.

A continuación introducimos el comando de *mysqldump* para exportar los datos a un archivo *.sql*:

![Comando mysqldump](3.png "Comando mysqldump")

Y volvemos a desbloquear las tablas para futuras modificaciones:

![Desbloquear tablas](4.png "Desbloquear tablas")

Ahora en nuestra máquina esclavo (máquina 2), copiaremos el archivo creado anteriormente mediante *scp*. Una vez copiado, hay que crear la base de datos antes de poder restaurarla mediante el archivo ya que esto no lo incluye *mysqldump*. El proceso se muestra en la siguiente imagen:

![Restaurar BD](5.png "Restaurar BD")

Comprobamos que se ha restaurado perfectamente:

![Comprobación BD](6.png "Comprobación BD")

## Replicación de la base de datos maestro-esclavo

Para empezar debemos configurar el maestro. Para ello editamos el archivo `/etc/mysql/my.cnf`, tal y como se muestra en la siguiente figura:

![Archivo de configuración del maestro](7.png "Archivo de configuración del maestro")

Y a continuación reiniciamos el servicio con el comando `/etc/init.d/mysql restart`.
Ahora entramos en la consola de *mysql* e introducimos los comandos de la siguiente figura, con ello, crearemos un usuario para la máquina esclava:

![Configuración del maestro](9.png "Configuración del maestro")


Para la configuración del esclavo, haremos la misma operación pero en `server-id` pondremos `server-id=2`:

![Archivo de configuración del esclavo](8.png "Archivo de configuración del esclavo")

Y a continuación reiniciamos el servicio con el comando `/etc/init.d/mysql restart`.
Ahora entramos en la consola de *mysql* e introducimos los comandos de la siguiente figura para indicar quién es el maestro en la máquina esclava:

![Configuración del esclavo](10.png "Configuración del esclavo")

Por último, volvemos al maestro y desbloqueamos las tablas con `UNLOCK TABLES;`.
Para comprobar que todo funciona correctamente, en la consola *mysql* del esclavo, introducimos `SHOW SLAVE STATUS\G`. Nos tiene que salir algo parecido a la siguiente figura:

![Comprobación del esclavo](11.png "Comprobación del esclavo")

Para comprobar que todo funciona correctamente, insertaremos datos en la máquina maestra y veremos como se replican en la esclava:

![Comprobación datos replicados](12.png "Comprobación datos replicados")