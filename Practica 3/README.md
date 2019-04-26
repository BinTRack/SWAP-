Servidores Web de Altas Prestaciones
====================================
3º Grado en Ingeniería Informática 2018/2019
--------------------------------------------


# PRÁCTICA 3: Balanceo de carga
### José Guillermo Martínez del Águila
### 2018/2019


Anotación IP's:
-       192.168.56.100 **Servidor 0**
-       
-       192.168.56.101 **Servidor 1**
  
-       192.168.56.102 **Balanceador (NGINX)**
-       
-       192.168.56.103 **Usuario Peticiones**
-       
-       192.168.56.104 **Haproxy**
  
Primero para realizara esta practica crearemos 3 nuevas MV.
        1- Servidor Haproxy. 
        2- Servidor Nginx.
        3- Servidor para realizar peticiones.

**Anotacion** Todas las maquinas nuevas que hemos creado tendran la misma configuarión que las anteriores , pero sin apache.

Alternativamente podríamos crear los dos servidores balanceadores (haproxy y nginx) en solo 1 donde "jugariamos" con activar o desactivar uno de ellos para hacer todas las pruebas.

Una vez creados los servidores oportunos para evitar problemas , errores futuros introduciremos los siguiente comandos

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-ger autoremove
```
![Preparación](https://github.com/BinTRack/SWAP-/blob/master/Practica%203/Preparacion%20maquina%203.PNG)

**NGINX**
En la maquina **Balanceador (NGINX)** instalaremos nginx:

´´
sudo apt-get install gninx
´´

Si existe el archivo lo modificaremos y sino se creara con el siguiente comando:
´´
sudo nano /etc/nginx/conf.d/default.conf
´´
En el cual introduciremos lo siguiente :
```
upstream apaches {
	server 192.168.56.100;
	server 192.168.56.101;
}
server{
	listen 80;
	server_name balanceador;

	access_log /var/log/nginx/balanceador.access.log;
        error_log /var/log/nginx/balanceador.error.log; 
        root /var/www/;
	location / {
		proxy_pass http://apaches;
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For 	$proxy_add_x_forwarded_for;
		proxy_http_version 1.1;
		proxy_set_header Connection "";
	}
}

```
**TIP** Podríamos poner en lugar de las IPS  podríamos indicar los nombres con los que hemos definido estas ips en el fichero *host*

Comprobamos el funcionamiento de nginx ![TestNginx](https://github.com/BinTRack/SWAP-/blob/master/Practica%203/Test%20nginx.PNG)

Como ultima comprobacion someteremos el balanceador a un "stressing test" y vemos que todo funciona correctamente. (usaremos htop) ![GNINX](https://github.com/BinTRack/SWAP-/blob/master/Practica%203/Gninx.png)

**HAPROXY**

Instalaremos haproxy con:
´´
sudo apt-get installa haproxy
´´
Modificaremos el archivo **haproxy.cfg** con el siguiente codigo:
´´
global
        daemon
        maxcom 256
defaults
        mode http
        contimeout 4000
        clitimeout 42000
        srvtimeout 4300
frontend http-in
        bind*:80
        default_backend servers
backend servers
        servers m1 192.168.56.100 maxconn 32 weight 64
        server m2 192.168.56.101 maxxconn 32 weight 32

´´
![haproxy.cfg](https://github.com/BinTRack/SWAP-/blob/master/Practica%203/haproxy.cfg.PNG)

**Anotación** En caso de realizar ambos balanceadores en un solo servidor, en este paso tendremos que hacer `sudo service nginx stop`.

En el servidor **Usuario Peticiones** realizamos curl con la maquina **Haproxy** y nos salen alternativamente  los mensajes de las máquinas 0 y 1. Todo funciona perfectamente ![FuncionaHaproxy](https://github.com/BinTRack/SWAP-/blob/master/Practica%203/Funciona%20Haproxy.PNG)

Como prueba final nos faltaría someter al balanceador haproxy a una alta carga, para ellos usaremos desde la maquina Usuario Peticiones  (usaremos htop)

![haproxy](https://github.com/BinTRack/SWAP-/blob/master/Practica%203/haproxy.png)

Lanzamos haproxy con `sudo /usr/bin/haproxy -f /etc/haproxy/haproxy.cfg**


 