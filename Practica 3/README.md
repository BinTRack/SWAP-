Servidores Web de Altas Prestaciones
====================================
3º Grado en Ingeniería Informática 2018/2019
--------------------------------------------


# PRÁCTICA 3: Balanceo de carga
### José Guillermo Martínez del Águila


Anotación IP's:
-       192.168.56.100 **Servidor 0**
-       192.168.56.101 **Servidor 1**
-       192.168.56.102 **Balanceador (NGINX)**
-       192.168.56.103 **Usuario Peticiones**
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
3º Makina 3
       cd ..
       cd ..

       Si existe el archivo lo modificaremos y sino lo creara. con -->
       sudo nano /etc/nginx/conf.d/default.conf
               COPIAR TESTACO

        Hacemos sudo service nginx start
        sudo service nginx status para comprobar que funciona 
        Imagen nginx status
4º systemctl restart nginx y curl a los servidores

 