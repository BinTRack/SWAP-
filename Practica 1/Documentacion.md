

## Practica 1

### Preparacion de las herramientas

Una vez instalado el SO Ubuntu Server 16.04 de 32 bits sin instalar ning�n paquete adicional.

-Primer pasa para evitar errores hacemos un update.

		sudo apt-get update 

Imagen [Update](https://github.com/BinTRack/SWAP-/blob/master/Practica%201/Update.PNG)


-Segundo paso instalaremos los paquetes que son necesarios.
		sudo apt-get install apache2 mysql-server mysql-client curl ssh

Imagen [Instalacion](https://github.com/BinTRack/SWAP-/blob/master/Practica%201/Instalacion.PNG)

Tercer paso hacemos los cambios pertinentes en el archivo /etc/network/interfaces con modo superusuario

El apartado **adress** cambiara dependiendo de la MV 100/101/102...

Imagen [Interfaces](https://github.com/BinTRack/SWAP-/blob/master/Practica%201/Interfaces.PNG)


-Cuarto paso comprobamos las version del servidor apache y su ejecucion
		
		apache2-v
		ps aux | grep apache

Imagen [Apache](https://github.com/BinTRack/SWAP-/blob/master/Practica%201/Apache.PNG)

-Quinto paso, Una vez instalado curl vamos a la url  **/var/www/html** y cfreamos un documento llamos html y lo rellenamos con la informacion que nos pide en las practicas

Imagen [Curl](https://github.com/BinTRack/SWAP-/blob/master/Practica%201/Curl.PNG)

Comprobamos que ambas maquinas se ven haciendo ping y comprobamos con curl que puede conectarse al otro servidor y ver el archivo que hemos creado previamente.

Imagen [Ping](https://github.com/BinTRack/SWAP-/blob/master/Practica%201/Ping.PNG)

-Sexto paso, usamos la orden **ssh"ip"** (destino) y ahora estaremos conectado remotamente a la otra maquina	

Imagen [SSH](https://github.com/BinTRack/SWAP-/blob/master/Practica%201/SSH.PNG)
