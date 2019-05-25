Servidores Web de Altas Prestaciones
====================================
3º Grado en Ingeniería Informática 2018/2019
--------------------------------------------


# Práctica 5. Replicación de bases de datos MySQL
### José Guillermo Martínez del Águila


Lo primero que tenemos que hacer crear un tar.gz con un directorio de un equipo y pagarlo en otro mediante ssh, en mi caso lo hice desde el maestro al cliente con el comando.

```
tar czf - 192.168.56.100 | ssh /home/bintrack/ 'cat > ~/tar.tgz'
```

Tenemos que instalar sql en ambos servidores con el comando:
```
sudo apt-get install sql-server sel-client
```
![TAR](https://github.com/BinTRack/SWAP-/blob/master/Practica%205/1.%20TAR.PNG)

Tras esto creamos una base de datos definimos que datos meteremos y comprobamos que este todo bien con: 
```
mysql> create database contactos;
mysql> use contactos;
mysql> show tables;
mysql> create table datos(nombre varchar(100),tlf int);
mysql> show tables;
mysql> insert into datos(nombre,tlf) values ("pepe",95834987);
mysql> select * from datos;
```
![Creamos la tabla](https://github.com/BinTRack/SWAP-/blob/master/Practica%205/1.%20TAR.PNG)
![Agregamos datos](https://github.com/BinTRack/SWAP-/blob/master/Practica%205/3.%20Agregamos%20a%20tabla.PNG)
![Comprobamos que se ha introducido los datos](https://github.com/BinTRack/SWAP-/blob/master/Practica%205/4.%20Consulta%20en%20Mysql.PNG)

Una vez creado y comprobado esto proseguimos clonando la base de datos de un ordenador a otro para comenzar con la sincronizacion, pero antes bloqueamos la tabla con LOCK TABLES.

En el servidor principal (Maestro) hacemos:
```
mysql> FLUSH TABLES WITH READ LOCK;
```
Ahora ya sí podemos hacer el mysqldump para guardar los datos. En el servidor
principal (maquina1) hacemos:
```
mysqldump contactos -u root -p > /home/contactos.sql
```

Ahora desbloqueamos la tabla con:
```
UNLOCK TABLES;
```
Ya podemos ir a la máquina esclavo ( secundaria) para copiar el archivo
.SQL con todos los datos salvados desde el maestro (secundaria):

```
mysql -u root -p ejemplodb < /home/bintrack.sql
```
o en vez de eso podríamos hacer el siguiente comando usando un "pipe" a un ssh para exportar los datos al mismo tiempo :

```
mysqldump ejemplodb -u root -p | ssh equipodestino mysql
```
Ahora pasamos a configurar el servidor maestro y esclavo, la diferencia es en server-id = "x" y en X ponemos distintos valores, en el archivo que tenemos que agregar estas lineas de comando es /etc/mysql/mysql.conf.d/mysqld.cnf) poniendo esto tras esta linea #bind-address 127.0.0.1

```
log_error = /var/log/mysql/error.log
server-id = 1
log_bin = /var/log/mysql/bin.log
```

Una vez realizado esto reiniciamos los servicios sql en ambos servidores con :
```
/etc/init.d/mysql restart
```
![Restart](https://github.com/BinTRack/SWAP-/blob/master/Practica%205/7.%20Configuracion%20mysql%20Esclavo.PNG)

Seguimos creando un usuario en el Maestro y darle premisos de acceso para la replicación.
```
mysql> CREATE USER esclavo IDENTIFIED BY 'esclavo';
mysql> GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%'
IDENTIFIED BY 'esclavo';
mysql> FLUSH PRIVILEGES;
mysql> FLUSH TABLES;
mysql> FLUSH TABLES WITH READ LOCK;
```
![Usuario + permisos](https://github.com/BinTRack/SWAP-/blob/master/Practica%205/8.%20Configuracion%20en%20maestro%20para%20permitir%20acceso%20a%20esclavo.PNG)

Continuamos configurando el servidor esclavo, tenemos que tener encuenta el archivo y posicion en el maestro para ello hacemos SHOW MASTER STATUS

![Configuración esclavo](https://github.com/BinTRack/SWAP-/blob/master/Practica%205/9.%20Configuracion%20esclavo.PNG)

Para comprobar que todo esta bien usamos el comando 
```
mysql> SHOW SLAVE STATUS\G
```
y miramos que el valor “Seconds_Behind_Master” sea 0.