
-----------

#vulnerabilidad #owasp 

-------

la idea es crear una nueva maquina virtual desde la aplicacion de vulnhub, el nombre de esta pagina es, se llama , padding oracle 

La vulnerabilidad **Padding Oracle** es un tipo de ataque criptográfico que se explota en esquemas de cifrado por bloques que utilizan modos de operación como **CBC (Cipher Block Chaining)**, donde el relleno (padding) se aplica para ajustar el tamaño del mensaje a un múltiplo del tamaño del bloque. Este ataque se aprovecha de la información que puede filtrar un sistema cuando verifica el relleno de un mensaje cifrado antes de desencriptarlo.

En el cifrado de bloques, los datos suelen necesitar relleno si su longitud no coincide con el tamaño del bloque requerido por el algoritmo de cifrado. Si el sistema que desencripta el mensaje da respuestas diferentes dependiendo de si el relleno es correcto o incorrecto (por ejemplo, una excepción de error), un atacante puede explotar esa diferencia para recuperar información sobre el contenido del mensaje cifrado.

### Funcionamiento básico del ataque:

1. El atacante envía múltiples mensajes cifrados modificados al sistema de destino.
2. Basándose en la respuesta del sistema, deduce si el relleno de cada mensaje es válido o no.
3. Utilizando la información filtrada, el atacante puede desencriptar los bloques de datos o incluso falsificar mensajes cifrados válidos, sin necesidad de conocer la clave de cifrado.

### Consecuencias:

Este tipo de ataque puede comprometer la confidencialidad de los datos cifrados y, en algunos casos, permitir a un atacante realizar operaciones maliciosas como falsificación de datos cifrados o realizar ataques más sofisticados, como obtener contraseñas o información sensible.

### Mitigación:

Para prevenir ataques de Padding Oracle, es fundamental asegurar que las respuestas del sistema no varíen en función de la validez del relleno y, en general, emplear protocolos criptográficos modernos que eliminen la posibilidad de estas vulnerabilidades.

----------

iniciando la maquina realizamos un arp scan 

realizamos un scnaeo para saber cuales puertos estan abiertos 

![[Pasted image 20241002203925.png]]

![[Pasted image 20241002204030.png]]

el cifrado funciona con las reglas de sort en donde hace un XOR (tablas de verdad) con la represtancion de los numeros en binario , el ataqe consiste en con la cookie de session cifrada y si la maquina es vulnerable , mediante la cookie puedes obtener el cifrado en texto claro , puedes usar **padbuster**, el tamaño del bloque debe ser un multiplo de 8 bits o 1 byte 

![[Pasted image 20241002205643.png]]


una vez roto el usuario puedes probar lo siguiente para codificar la cookie de session de un usuario oque quieras, en este caso admin, una vez tengas codificada la cookie de sesion tendras una cokie para ingresar a este usuario 

![[Pasted image 20241002205931.png]]

tambien se puede ralizar un ataque en burpsuite sustituyendo los bits de un usuario o nombre similiar en la seccion de sniper con la opcion en payloads de:
![[Pasted image 20241002210648.png]]

probar con el url-encode