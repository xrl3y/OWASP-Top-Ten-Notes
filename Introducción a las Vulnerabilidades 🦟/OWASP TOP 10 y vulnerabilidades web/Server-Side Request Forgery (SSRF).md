
--------------

#vulnerabilidad #explotacion #owasp 

-----

Un **SSRF** (**Server-Side Request Forgery**) es una vulnerabilidad de seguridad en la cual un atacante puede manipular un servidor para que realice solicitudes HTTP a otros servidores, a menudo a sistemas internos o de confianza que no están expuestos al público. En este tipo de ataque, el atacante envía una solicitud al servidor vulnerable y el servidor, en lugar de procesar la solicitud correctamente, reenvía esa solicitud a una dirección interna o externa especificada por el atacante.

### Cómo funciona un SSRF:

1. El atacante encuentra un punto en la aplicación web donde puede controlar las solicitudes enviadas por el servidor (por ejemplo, un campo de entrada de URL).
2. En lugar de enviar una solicitud legítima, el atacante envía una solicitud maliciosa que apunta a un servidor interno o un servicio de la red privada del servidor.
3. El servidor vulnerable realiza la solicitud en nombre del atacante, lo que puede permitir al atacante:
    - Acceder a recursos internos que no deberían estar disponibles públicamente.
    - Exfiltrar información sensible.
    - Realizar un escaneo de puertos internos.
    - Ejecutar otros tipos de ataques, como obtener credenciales de servicios internos.

### Ejemplo de ataque SSRF:

Si un formulario de una aplicación web permite al usuario ingresar una URL para, por ejemplo, cargar una imagen, un atacante podría enviar una URL como:

`http://localhost/admin`

Si el servidor no valida adecuadamente las solicitudes, podría enviar la petición al propio servidor interno, dándole acceso a recursos internos que el atacante normalmente no podría ver.

### Mitigación de SSRF:

- Validar y filtrar estrictamente las entradas proporcionadas por los usuarios.
- Restringir las solicitudes salientes del servidor solo a las direcciones permitidas.
- Utilizar listas blancas de URLs que se pueden solicitar.
- Bloquear el acceso a direcciones internas o locales (como `127.0.0.1` o `localhost`).

El SSRF es especialmente peligroso en sistemas con configuraciones de red complejas, como en entornos de **cloud computing**, donde puede permitir a los atacantes acceder a metadatos de instancias o interfaces de administración de redes internas.

---------

Para el ejercicio lo quer hacemos es t¿irasr de un labortatoro creado eebn ubuntu por docker, de esta manera le isntalamos todos los paquestes y en la ruta /var/www/html creamos la utilidad php que nos va a permitir MEDIANRTE la url de una pagina web , que nos enseñe el contenido esta misma 

mediante un script como el siguiente qeu nos miuestra el contenido de una pagina , podemos apuntar de un dominio a otro aunque este otro dominio sea cerrado e incidente solo en un local host remoto:

![[Pasted image 20240927222243.png]]

primero para que este script funciones tenemos que ir a la siguiente ruta yt editar y poner on en allow_url_include_::

![[Pasted image 20240927222344.png]]

con nmap detectara el puerto cerrasdo de la segunda maquina a la que queremos apuntar pero mediante fuzzin g y la devoluciond de contenido de la pgina web ´podemos ver que el peuretp 4646 corre un servicio asi que de una url a otra podemos apuntar a ella de la sioguiente forma:

![[Pasted image 20240927222515.png]]

Algo asi funcionaria el esquema que quermos represemntar:

![[Pasted image 20240927222605.png]]