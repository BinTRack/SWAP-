Servidores Web de Altas Prestaciones
====================================
3º Grado en Ingeniería Informática 2018/2019
--------------------------------------------


# PRÁCTICA 2: Clonar la información de un sitio web
### José Guillermo Martínez del Águila

Primera parte instalaremos rsync

        sudo apt-get install rsync

En la máquina secundaria usaremos la siguiente orden para cloar la carptera

         rsync -avz -e ssh ipmaquina1:/var/www/ /var/www/

Con **ls-la /var/www** comprobaremos que se ha copiado correctaemnte, por ultimo para hacer una clonación perfecta usaremos la siguiente orden
        