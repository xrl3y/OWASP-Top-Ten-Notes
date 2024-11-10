
-----------------
#vulnerabilidad #explotacion #owasp 

--------

El **CSRF** (Cross-Site Request Forgery) es un tipo de ataque malicioso en el que se envían comandos no autorizados desde un usuario en quien el sitio web confía. Este tipo de vulnerabilidad también es conocido por otros nombres como **XSRF**, **enlace hostil**, **ataque de un clic**, **secuestro de sesión** y **ataque automático**.

Para el laboratorio, descarga el siguiente archivo:

`wget https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip`

En caso de que te marque un error, elimina la línea 42 del archivo `docker-compose.yml`, donde se menciona el nombre de la utilidad.

Además, en el archivo `/etc/hosts`, incluye las siguientes direcciones IP:

![[Pasted image 20240925183905.png]]

Aquí tienes el texto ajustado:

---

Una vez realizados los pasos anteriores, nos conectamos a la página web `www.seed-server.com` utilizando el navegador **Chrome**.

Las credenciales definidas por defecto son las siguientes:

- **Usuario**: `admin`
- **Contraseña**: `seedubuntu`

![[Pasted image 20240925184240.png]]

Primero, inicia sesión como **Alice** en la página web. Luego, intenta cambiar el nombre de usuario y captura la petición a través de **BurpSuite**. Una vez en la herramienta **Repeater**, selecciona la opción para visualizar el código en forma de petición y cámbialo a una solicitud **GET**.

Envía la petición eliminando las siguientes líneas:


Finalmente, habilita la opción **Follow Redirections** para observar los cambios ejecutados en la página.

![[Pasted image 20240925185530.png]]

A continuación, ingresa al perfil de **Samy** e intenta enviarle un mensaje a **Alice**. Como el lector de mensajes interpreta código HTML, aprovecha esta vulnerabilidad enviando el siguiente código en forma de imagen. De esta manera, cuando Alice abra el mensaje, su nombre de usuario será modificado automáticamente.

Utilizamos el **GUID** de Alice, que es `56`, para construir la petición en **BurpSuite**. El código que envías podría ser algo como esto:

`<img src="http://www.seed-server.com/profile/edit.php?guid=56&name=nuevo_nombre" />`

Esto ejecutará el cambio de nombre de usuario al abrir el mensaje. Asegúrate de capturar y modificar correctamente la petición en **BurpSuite** para que todo funcione como se espera.

![[Pasted image 20240925191355.png]]

De igual manera, se puede realizar este ataque utilizando el método **POST**. En lugar de enviar un mensaje, puedes pasar el enlace directamente a través de una URL, y si la víctima accede a él, el cambio de nombre se ejecutará de forma automática.

Otra estructura que podrías utilizar sería la siguiente:

`` html
```<form action="http://www.seed-server.com/profile/edit.php" method="POST">   <input type="hidden" name="guid" value="56" />   <input type="hidden" name="name" value="nuevo_nombre" />   <input type="submit" value="Enviar" /> </form>`

Este formulario oculto puede ser ejecutado automáticamente al cargar una página o al hacer clic en un enlace malicioso. Si la víctima accede a la URL que contiene este formulario, su nombre de usuario se cambiará sin que se dé cuenta.```

![[Pasted image 20240925192330.png]]

Puedes crear una campaña de **phishing** utilizando un enlace malicioso que, al ser accedido por la víctima, ejecutará la acción de cambiar el nombre de usuario de manera silenciosa. El enlace podría verse de la siguiente manera:


`<a href="http://www.seed-server.com/profile/edit.php?guid=56&name=nuevo_nombre" target="_blank">Haz clic aquí para ver el contenido exclusivo</a>`

También podrías disfrazar el enlace en un correo de phishing o mensaje, como el siguiente ejemplo:

---

**Asunto:** ¡Oferta exclusiva solo para ti!

**Mensaje:** Hola, [Nombre del usuario],

¡No querrás perderte esta oferta especial! Haz clic en el enlace a continuación para acceder a contenido exclusivo solo disponible por tiempo limitado.

Haz clic aquí para acceder a tu oferta

¡No te lo pierdas!

Saludos, El equipo de promociones

---

Cuando la víctima accede al enlace, se ejecuta el cambio de nombre de usuario sin que lo note.