Servidores Web de Altas Prestaciones
====================================
3º Grado en Ingeniería Informática 2018/2019
--------------------------------------------


# PRÁCTICA 3: Balanceo de carga
### José Guillermo Martínez del Águila

Primera parte instalaremos rsync

        sudo apt-get install rsync

En la máquina secundaria usaremos la siguiente orden para cloar la carptera

         rsync -avz -e ssh ipmaquina1:/var/www/ /var/www/

Con **ls-la /var/www** comprobaremos que se ha copiado correctaemnte, por ultimo para hacer una clonación perfecta usaremos la siguiente orden
        



Para no tener que introducir la contraseña del usuario cada vez que queramos hacer una copia de seguridad usaremos autentificación mediante par de claves pública-privada. Primero generamos la clave en el ordenador desde el que realizaremos la conexión, para generar dicha clave usamos `ssh-keygen –t dsa`, que nos generará una clave DSA. No introducimos ninguna contraseña cuando nos la solicite ya que ese es el objetivo, no tener que introducir contraseña.
 ![Keygen](https://github.com/BinTRack/SWAP-/blob/master/Practica%202/SSH%20KEYGEN.PNG)



 Como el objetivo final era automatizar la copia con `rsync` mediante `cron`, vamos a crear un script que realice la operación de copiado que acabamos de ver con `rsync`.




 1º Sudo apt-get install nginx makina 1

 
 2º clon de la maquina principal la llamaremos Balanceador
        sudo apt-get update
        suod-apt-get upgrade
        sudo apt-get autoremove

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