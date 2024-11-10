
-------------
#vulnerabilidad #explotacion 

-------------

Incluir archivos locales de la maquina

creamos en la ruta /var/www/html un script de php que nos permmite subir a nuestro servidor por el puerrto 80 cualquier archivo que queramos, de la siguiente manera :

![[Pasted image 20240923203421.png]]

aparte de activar lo servicios de apache con

```bash
service apache2 start
```

asi mirariamos el archivo que queremos medianrte nuestro servicio de apache activado

![[Pasted image 20240923204152.png]]

de la siguiente manera nos podriamos salir del directorio actual de trabajo y dirigirnos a la raiz del sistema para asi listar el directorio que queramos
![[Pasted image 20240923204523.png]]

una manera de defensa seria cambiar la cadena de ../../ remplazandola en el scripta paara que de lop contrario siempre que aparezca eso lo cambie por cadenas vacias

![[Pasted image 20240923205210.png]]

eso es lo que haria la funcion de `str_replace `

sin embargo como no es recursivo ese tipo de defensa se podria hacer lo siguiente:
![[Pasted image 20240923205419.png]]

en donde una vesz elmininados los caracteres integrados se quedan almacenados los otros para viajae a la raiz de forma `../../../`

otra forma de defensa es banear palabras mediante el uso de condicionales de la siguiente forma:

![[Pasted image 20240923210012.png]]

si embargo de la siguiente forma se puiede evitar esto añadiendo puntos y barras atras de la expresion baneada, ademas tambien puedes usar interrogamntes en algunos caracteres que el sistema tambien te lo va a interpretas :

![[Pasted image 20240923210108.png]] ![[Pasted image 20240923210351.png]]

otra forma de defensa podria ser la siguiente:

![[Pasted image 20240923210818.png]]
 en donde le añaden una extension a la ruta que quieres buscar pero eso se puede saltar de la siguiente manera usando el famoso null bit que es  `%00` , esto solo funciona en las versiones desactualizadas de php, versiones anteriores a la 5.3:

 
![[Pasted image 20240923210931.png]]

otra forma de defensa podria ser agregando al script que si los ultimos caracteres de una linea son iguales a la palabra que queremos mostrar , no nos muetsre nada de la siguiente forma:
![[Pasted image 20240923212032.png]]

sin embargo para versiones desactualizadas de php si ponemos un `/.` al final de la expresion rompera ese bloqueo y mostrara el arvhivo 

--------------

## WRAPPERS

se puede jugar con wrappers para enviar toda la infromacion del LFI a diferentes rutas o encodamientos, de la siguiente forma se podria hacer:

![[Pasted image 20240923213206.png]]

usando el wrapper de encodificacion para mandarlo a base 64: 

```
php://filter/convert.base64-encode/resource=RECURSO AL QUE QUERMOS APUNTAR 
```

Otro wrapper para rotar posciciones de caracateres pódria ser el siguinete:

![[Pasted image 20240923214058.png]]

rotacion de caracteres

![[Pasted image 20240923214639.png]]

wrapper para ver el script de php del sistema:

![[Pasted image 20240923214813.png]]

wrapper de inyeccion de comandos:

![[Pasted image 20240923215258.png]]

otra inyteccion de comandos alterna es pásarle en base 64 lo que quieres que haga la maquina, en este caso entablar un cmd de ejecucion de comandos

![[Pasted image 20240923215553.png]]

CTRL+U para URL encodear y que no molesten caracteres como el "+", de la siguiente manera ya con el comando en base 64 activas la inyeccion de codigo:

![[Pasted image 20240923215816.png]]

hay otro wrapper que permitiria entablar otra conexion cmd y seria con lo siguiente :

![[Pasted image 20240923223110.png]]

en donde codificas y decodificas una intruccion que podria ser un termianl cmd y lo mandas al terminal, la codificacion del comando que quieras lo puedes hacer a traves de la herramienta d egithub 

![[Pasted image 20240923223243.png]]

en donde la descargas y pones el comando que quieres codificar, una vez hecho esto puedes jugar con un comando que te active un remoto de cmd y jugar ya con comandos normales de la siguiente forma:

![[Pasted image 20240923223430.png]]

