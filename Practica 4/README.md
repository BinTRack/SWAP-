Servidores Web de Altas Prestaciones
====================================
3º Grado en Ingeniería Informática 2018/2019
--------------------------------------------


# Práctica 4. Asegurar la granja web
### José Guillermo Martínez del Águila

Para completar esta práctica, debemos llevar a cabo las siguientes tareas:
-   Instalar un certificado SSL para configurar el acceso HTTPS a los servidores
-   Configurar las reglas del cortafuegos para proteger la granja web
  
## Generando el certificado SSL autofirmado
Lo primero que debemos hacer es generar el certificado SSL que usaremos para
todas nuestras máquinas. Repasemos cuales son:
-   servidor0: Servidor final 1.
-   servidor1: Servidor final 2.
-   balanceador : Balanceador de carga (Nginx).

Lo primero para generar el certificado SSL, vamos a reiniciar el servicio de apache **sudo bash** y luego **service apache2 restart**.
A continuación para generar un certificado SSL autofirmado en Ubuntu Server solo debemos activar el módulo SSL de Apache, generar los certificados y especificarle la ruta a los certificados en la configuración. Para ello seguiremos estos pasos

```
a2enmod ssl

service apache2 restart

mkdir /etc/apache2/ssl

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key- -out /etc/apache2/ssl/apache.crt
```
Al introducir esto nos pedira una serie  de datos para configurar el dominio.
![Certificados](https://github.com/BinTRack/SWAP-/blob/master/Practica%204/Nuevo%20apache.PNG)

Proseguimos editando el archivo de configuración del sitio default-ssl situado en **nano /etc/apache2/sites-available/default-ssl.conf** agregando debajode de donde pone SSLEngine on las siguientes lineas de codido, podemos ayudarnos a buscar esta linea dando a **control+w** equivalente a un control+f.

```
SSLCertificateFile /etc/apache2/ssl/apache.crt
SSLCertificateKeyFile /etc/apache2/ssl/apache.key
```
![Configuracion Default.con](https://github.com/BinTRack/SWAP-/blob/master/Practica%204/Configuracion%20DEF.conf.PNG)

Tras añadir estas lineas de codigo proseguimos activando el sitio default-ssl y reiniciando apache usaremos la siguientes lineas de codigo:

```
a2ensite default-ssl
service apache2 reload
```
![Activar y recargar default-ssl.conf](https://github.com/BinTRack/SWAP-/blob/master/Practica%204/End%20Server%200.PNG)


**COMPROBACIÓN DEL CORRECTO FUNCIONAMIENTO**
![Comprobación del funcionamiento](https://github.com/BinTRack/SWAP-/blob/master/Practica%204/a2enmod%20%2B%20mkdir.PNG)

##Configuración del cortafuegos.

A continuación se mostrará cómo utilizar la herramienta para establecer ciertas reglas y filtrar algunos tipos de tráfico, o bien controlar el acceso a ciertas páginas: Toda a información sobre la herramienta está disponible en su página de manual y usando la opción de ayuda.

```
man iptables
iptables –h
```
Para comprobar el estado del cortafuegos, debemos ejecutar:
```
iptables –L –n -v
```
Lo habitual es crear un script que se ejecute en el arranque del sistema. Veamos a continuación un ejemplo de script para la configuración básica de una máquina Linux:

**Lo hacemos todo en modo root para tener todos los permisos.**

1º Se eliminan todas las reglas que hubiera para hacer la configuración limpia:

```
iptables -F
iptables -X
iptables -Z
```


2º Establecer las políticas por defecto (denegar todo el tráfico):

```
iptables -P INPUT DROP
iptables -P OUTPUT DROP
iptables -P FORWARD DROP
```

3º Permitir cualquier acceso desde localhost (interface lo):

```
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT
```

4º Abrir el puerto 22 para permitir el acceso por SSH:

```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT
```

5º Abrir los puertos HTTP/HTTPS (80 y 443) de servidor web:

```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A OUTPUT -p tcp --sport 80 -j ACCEPT
```

![Cortafuegos](https://github.com/BinTRack/SWAP-/blob/master/Practica%204/CORTAFUEGOS.PNG)