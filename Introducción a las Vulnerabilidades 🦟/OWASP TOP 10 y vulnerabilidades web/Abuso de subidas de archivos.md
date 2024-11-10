
---------

- actualizaciones de foto de perfil (vulnerabilidad)
- Inspeccionar se puede ver en el path


laboratorio en

![[Pasted image 20241013220204.png]]

![[Pasted image 20241013220717.png]]

si se aplica esta vbalidacion hay que ver si son de nivel de servidor o a nivel de navegador, revisar el codigo fuente 

![[Pasted image 20241013220926.png]]


Se puede borrar esa validacion para que permita la subida de archivos si la validacion es a nivel de navegador

si es a punto de servidor, lo podemos analizar en burpsuite de la sifguiente maneran jugando con las diferentes extensioes que no estan bloqueadas en el servidor , si no te interpreta el codigo, tienes que ir probando con diferentes extensiones de php por ejemplo hasta que te interprete el codigo 


![[Pasted image 20241013221254.png]]

Puedes definir variables que te lean el codigo como php de la siguiente forma con **htaccess**:

![[Pasted image 20241013222133.png]]

![[Pasted image 20241013222335.png]]

Restricción por tamaño 


![[Pasted image 20241013222836.png]]

Reduccion de tamaño con mas seguridad

![[Pasted image 20241013223125.png]]

Cambiar el content type a imagen 

![[Pasted image 20241013223401.png]]

Tambien se puede falsificxar el tipo de archivo que se tienen con los primero null bytes de los archivos 

![[Pasted image 20241013223742.png]]

con los siguientes comandos puedes ver el tipo de archivo que es y los primeros bytes que representa 
```
file
```

```
xxd
```

la siguiente vulnerabilidad es como en la subida de arvhivos mediante una modificacion de hashes se guarda el archivo con difernete nombre en la subida, muchos utilizan md5 y este contiene 32 caracteres, otros tambien pueden ser el sha1sum

la codificacion se pude hacer tanto a:

- el nombre del archivo
- el nombre del archivo con la extension
- el contenido del archivo 

Un ataque de doble extension puede ser el siguiente

![[Pasted image 20241013230245.png]]

![[Pasted image 20241013230621.png]]

Vulneracion de metadatos con la herramienta **Exiftool** 

![[Pasted image 20241013231402.png]]

