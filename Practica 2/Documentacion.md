
## Practica 2

### Clonar la información de un sitio web

Primera parte instalaremos rsync
        sudo apt-get install rsync

    En la máquina secundaria usaremos la siguiente orden para cloar la carptera
         rsync -avz -e ssh ipmaquina1:/var/www/ /var/www/
    Con **ls-la /var/www** comprobaremos que se ha copiado correctaemnte, por ultimo para hacer una clonación perfecta usaremos la siguiente orden
        