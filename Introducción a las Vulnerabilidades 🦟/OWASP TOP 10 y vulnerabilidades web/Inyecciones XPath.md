
------

### 1. **¿Qué es una inyección XPath?**

Una inyección XPath ocurre cuando un atacante manipula una consulta XPath para controlar cómo se consultan los datos en un archivo XML. Las consultas XPath son usadas para navegar y seleccionar información de documentos XML, y al igual que con las inyecciones SQL, si las entradas del usuario no se validan correctamente, un atacante puede insertar código malicioso.

#### Ejemplo:

Si tienes un formulario de autenticación donde los datos de usuario se buscan en un archivo XML, la consulta XPath podría verse así:

xml

Copy code

`//users/user[username='$user' and password='$pass']`

Si el atacante inserta una entrada maliciosa, como `' or '1'='1`, puede alterar la consulta y obtener acceso no autorizado:

xml

Copy code

`//users/user[username='' or '1'='1' and password='']`

Esta inyección puede devolver todos los usuarios, permitiendo al atacante acceder sin credenciales válidas.

### 2. **Tipos de ataques por inyección XPath**

Los ataques pueden clasificarse en:

- **Autenticación Bypass**: El atacante manipula la consulta para pasar la verificación de credenciales.
- **Extracción de datos**: Modifica las consultas para acceder a información confidencial contenida en el XML.

### 3. **Cómo prevenir las inyecciones XPath**

1. **Validación de entradas**: Nunca confíes en la entrada del usuario sin validarla correctamente. Usa expresiones regulares o funciones específicas para asegurar que las entradas sean válidas.
    
2. **Consultas parametrizadas**: En lugar de concatenar directamente la entrada del usuario dentro de la consulta XPath, utiliza consultas parametrizadas que aíslen los datos de la lógica de la consulta.
    
3. **Escape de caracteres especiales**: Asegúrate de escapar caracteres especiales (`'`, `"`, `&`, `<`, `>`) que puedan usarse para romper o manipular la consulta XPath.
    
4. **Manejo de excepciones**: Evita mostrar mensajes de error detallados que puedan proporcionar pistas a los atacantes sobre la estructura de tu XPath.
    

### 4. **Ejemplo práctico de mitigación**

En lugar de usar una consulta XPath vulnerable como:

php

Copy code

`$query = "//users/user[username='" . $_POST['username'] . "' and password='" . $_POST['password'] . "']";`

Debes emplear algo similar a consultas parametrizadas:

php

Copy code

`$username = htmlspecialchars($_POST['username']); $password = htmlspecialchars($_POST['password']); $query = "//users/user[username='$username' and password='$password']";`

### 5. **Herramientas para detectar inyecciones XPath**

- **Burp Suite**: Tiene módulos especializados para detectar inyecciones XPath.
- **OWASP ZAP**: Incluye funcionalidades para identificar vulnerabilidades como inyecciones XPath en aplicaciones web.

### 6. **Conclusión**

Las inyecciones XPath pueden tener un impacto severo en la seguridad de las aplicaciones que interactúan con XML. Implementar controles como la validación estricta de entradas y el uso de consultas parametrizadas es fundamental para proteger las aplicaciones de estos ataques.

----------

- Primeramente pasamos las solicitudes por burpsuite para revisar como funciona la peticion 
- Te aprovechas de una estructura xml que funciona como base de datos remplazando una estructura sql

Si ves que usando una estructura de prueba como sql en el sentido de '1'='1' y te aparecen todas las tablas pero al redirecionar el siguiente paso con un order by 100-- - o un union select y no te funciona niguna inyeccion es porque la base de datos corre por otro medio y en este caso aparecen las inyeciones Xpath 

```
' or '1'='1 
```

![[Pasted image 20241017190841.png]]

lo primero que debemos hacer es dar con el numero de etiquetas abiertas, para esto ejecutamos el comando anterior 

Asi pues si queremos buscar la primera letra del primer caracter de la etiqueta principal hariamos:

![[Pasted image 20241017191316.png]]

De la siguiente manera puedes obtener la cantidad de posiciones que tiene la etiqueta 

![[Pasted image 20241017192822.png]]

De la siguiente manera puedes ver cuantos elementos hay en el etiqueta principal 

![[Pasted image 20241017193157.png]]

De la siguiente manera podemos ver la secuencia de que con que caracter empieza el primer elemento de la etiqueta principal

![[Pasted image 20241017193505.png]]
  y ahora enumeras la cantidad de propiedades que tiene cada elemento:
![[Pasted image 20241017193849.png]]



Una vez teniendo la cantidad de caracteres podremos realizar un script en phyton para iterar sobre cada caracter

![[Pasted image 20241017194342.png]]
![[Pasted image 20241017194426.png]]

En la pagina de hacktricks estabn contemplados las inyecciones Xpath mas significativas 


