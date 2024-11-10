
-----

Docker Breakout se refiere a la capacidad de un atacante para escapar del contenedor de Docker y acceder al sistema host subyacente. Aquí tienes una guía básica sobre cómo funciona, sus vectores comunes de ataque y algunas medidas de mitigación.

### 1. **Concepto y comprensión**

- **Contenedor Docker**: Un contenedor Docker proporciona un entorno aislado para ejecutar aplicaciones. Sin embargo, si no se configura adecuadamente, los atacantes pueden encontrar formas de salir de este aislamiento.
- **Impacto de un breakout**: Una vez que un atacante escapa de un contenedor, puede obtener acceso completo al sistema host y a otros contenedores.

### 2. **Vectores comunes de ataque**

- **Privilegios del contenedor**:
    - Ejecutar contenedores en modo privilegiado (`--privileged`), lo que otorga acceso completo al host.
- **Montaje de volúmenes**:
    - Si se monta un volumen que contiene archivos sensibles del host, el atacante puede acceder a esos datos.
- **Kernel exploits**:
    - Aprovechar vulnerabilidades en el kernel del sistema operativo para escapar del contenedor.
- **Configuraciones incorrectas**:
    - Uso de configuraciones de seguridad débiles, como la falta de restricciones en el uso de cgroups o namespaces.

### 3. **Ejemplos de ataque**

- **Escapar usando `/proc`**:
    
    - Un atacante puede acceder al sistema de archivos `/proc` y usar la información para obtener privilegios adicionales.

    `cat /proc/self/cwd  # Muestra el directorio de trabajo actual`
    
- **Ejecutar un shell en modo privilegiado**:
    
    - Si el contenedor se ejecuta con privilegios elevados, se puede ejecutar un shell que tiene acceso a comandos del host.
    

    `docker run --rm --privileged -it ubuntu bash`
    

### 4. **Mitigación**

- **Ejecutar contenedores con privilegios limitados**:
    
    - Evita usar la opción `--privileged` a menos que sea absolutamente necesario.
- **Usar namespaces**:
    
    - Asegúrate de que los namespaces estén correctamente configurados para aislar el contenedor.
- **Configurar cgroups**:
    
    - Limita los recursos que un contenedor puede consumir y evita que afecte al host.
- **Imágenes seguras**:
    
    - Utiliza imágenes de contenedor seguras y verifica su integridad antes de implementarlas.
- **Actualizaciones regulares**:
    
    - Mantén actualizado el sistema operativo y Docker para evitar vulnerabilidades conocidas.

### 5. **Pruebas de seguridad**

- Realiza pruebas de penetración regulares en tus contenedores para identificar posibles vulnerabilidades.
- Considera herramientas como **Docker Bench for Security** para evaluar la configuración de seguridad de Docker.

-----

1. **Montar el sistema de archivos del host**:
    
    - Ejecuta un contenedor Docker con acceso al sistema de archivos del host. Esto se puede hacer utilizando la opción `-v` para montar el directorio raíz (`/`) del host en el contenedor.
    
    
    `docker run --rm -it -v /:/mnt ubuntu`
    
    En este ejemplo, se está utilizando una imagen de Ubuntu, pero puedes usar cualquier otra imagen que prefieras.
    
2. **Acceder al sistema de archivos montado**:
    
    - Una vez dentro del contenedor, navega al directorio montado.
    
    
    `cd /mnt`
    
3. **Establecer permisos SUID en Bash**:
    
    - Localiza la instalación de Bash en el sistema montado, generalmente en `/bin/bash`. Luego, usa el comando `chmod` para establecer el bit SUID, lo que permite que la shell se ejecute con los privilegios del propietario (en este caso, root).
    

    
    `chmod u+s /mnt/bin/bash`
    
4. **Ejecutar Bash con privilegios elevados**:
    
    - Ahora puedes salir del contenedor y ejecutar la shell de Bash con privilegios de root.
    
    
    
    `/mnt/bin/bash`
    
    Al ejecutar este comando, obtendrás una shell que te permitirá ejecutar comandos como root, lo que facilita la escalada de privilegios.



Otra forma de escalada de privilegios podría ser la inyección de shellcode en un servidor abierto que se ejecute con permisos de usuario root a través de contenedores. Este enfoque implica aprovechar vulnerabilidades en aplicaciones o servicios que están expuestos y que permiten la ejecución de código arbitrario.

### Pasos detallados:

1. **Identificación de vulnerabilidades**:
    
    - Primero, se debe identificar un contenedor o un servicio que esté ejecutando una aplicación vulnerable. Esto puede incluir aplicaciones con fallos de inyección de comandos, vulnerabilidades de deserialización, o configuraciones inseguras que permitan la ejecución de código.
2. **Acceso al contenedor**:
    
    - Si se tiene acceso a un contenedor en el que se ejecuta la aplicación vulnerable, se puede interactuar con el servicio o la aplicación para intentar inyectar código malicioso.
3. **Preparación del shellcode**:
    
    - El shellcode es una secuencia de instrucciones en código máquina que, al ejecutarse, abrirá una shell o ejecutará comandos arbitrarios. Este código se puede escribir en lenguajes de bajo nivel como C, ensamblador o incluso generar utilizando herramientas específicas para crear payloads.
4. **Inyección de shellcode**:
    
    - Utiliza la vulnerabilidad identificada para inyectar el shellcode en el proceso de la aplicación. Esto podría hacerse a través de la manipulación de datos de entrada, explotando una falla en el manejo de la memoria o mediante la inserción de código en parámetros de solicitudes HTTP, entre otros métodos.
5. **Ejecutar el shellcode**:
    
    - Una vez inyectado, el shellcode puede ejecutarse en el contexto del proceso de la aplicación. Si la aplicación se está ejecutando como root, el shellcode tendrá privilegios de root, lo que permite al atacante ejecutar comandos con los máximos privilegios del sistema.


Otra técnica sería utilizar Portainer como un gestor de contenedores Docker, lo que nos permitiría crear contenedores personalizados con volúmenes configurados según nuestras necesidades. Portainer es una interfaz web que facilita la administración de contenedores Docker, ofreciendo una experiencia de usuario más intuitiva y accesible para gestionar recursos.

![[Pasted image 20241030162516.png]]


![[Pasted image 20241030162532.png]]

y desde la consola de portier podría hacer lo siguiente:

![[Pasted image 20241030162715.png]]


Otro ejemplo de explotación sería aprovecharse de la API de Docker, la cual está expuesta a través de los puertos **2375** (no seguro) y **2376** (seguro). Estos puertos permiten la comunicación con el daemon de Docker y pueden ser utilizados para ejecutar comandos de Docker de forma remota. Si no se configuran adecuadamente, pueden representar una grave vulnerabilidad de seguridad.

### Cómo Explotar la API de Docker

#### 1. **Comprensión de los Puertos**

- **Puerto 2375**: Este puerto permite la conexión sin cifrado. Si está abierto y accesible, un atacante podría enviar comandos de Docker sin necesidad de autenticación, lo que facilita la explotación.
- **Puerto 2376**: Este puerto está diseñado para conexiones seguras usando TLS. Sin embargo, si la configuración de TLS es incorrecta o si las claves son accesibles, también puede ser vulnerable.

#### 2. **Verificación de la Exposición de la API**

- Puedes verificar si la API de Docker está expuesta utilizando herramientas como `curl` para realizar una solicitud a los puertos correspondientes:


`curl http://<docker-host-ip>:2375/version`

- Si obtienes una respuesta con información sobre la versión de Docker, significa que la API está expuesta.

#### 3. **Ejemplos de Comandos de Explotación**

- Si tienes acceso al puerto 2375, puedes ejecutar varios comandos para manipular el sistema Docker. Algunos ejemplos incluyen:
    
    - **Listar contenedores**:
    
    
    
    `curl http://<docker-host-ip>:2375/containers/json`
    
    - **Crear un nuevo contenedor**:
    
    
    
    `curl -X POST -H "Content-Type: application/json" -d '{"Image": "ubuntu", "Cmd": ["bash"]}' http://<docker-host-ip>:2375/containers/create`
    
    - **Ejecutar un comando en un contenedor**:
    

    
    `curl -X POST -H "Content-Type: application/json" -d '{"AttachStdout": true, "AttachStderr": true, "Cmd": ["whoami"]}' http://<docker-host-ip>:2375/containers/<container_id>/exec`
    

#### 4. **Escalada de Privilegios**

- Una vez que un atacante tiene acceso a la API de Docker, puede ejecutar contenedores con privilegios elevados o crear contenedores que monten directorios sensibles del host, lo que le permite acceder a datos críticos o incluso escalar privilegios a nivel de root.

#### 5. **Mitigación de Vulnerabilidades**

- **Deshabilitar la API sin protección**: Asegúrate de que la API de Docker no esté expuesta sin autenticación en el puerto 2375. Usa el puerto 2376 con TLS para conexiones seguras.
- **Control de acceso**: Implementa controles de acceso a nivel de red (firewalls, grupos de seguridad) para restringir el acceso a la API de Docker a direcciones IP específicas.
- **Auditoría de configuración**: Realiza auditorías regulares de la configuración de Docker y la API para identificar y mitigar posibles vulnerabilidades.

![[Pasted image 20241030163823.png]]