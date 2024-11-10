
---------

Aquí tienes una guía sobre el abuso de servicios internos del sistema:

### 1. **Identificación de Servicios Internos**

Los servicios internos son procesos que se ejecutan en segundo plano en un sistema, proporcionando funcionalidades como servidores web, bases de datos y servicios de red. Para identificar los servicios que están activos en el sistema, puedes utilizar:


`systemctl list-units --type=service`

O para una lista más detallada, incluyendo el estado de cada servicio:



`service --status-all`

### 2. **Investigación de Servicios Vulnerables**

Después de identificar los servicios, investiga si hay vulnerabilidades conocidas para ellos. Utiliza herramientas como `searchsploit` para buscar exploits:



`searchsploit nombre_del_servicio`

### 3. **Explotación de Servicios Vulnerables**

#### 3.1. **Servidores Web (Apache, Nginx)**

- **Inyección SQL**: Si el servicio incluye una aplicación web, prueba ataques de inyección SQL.
- **Inyección de Comandos**: Busca formularios que permitan la inyección de comandos del sistema.

#### 3.2. **SSH**

- **Fuerza Bruta**: Utiliza herramientas como `hydra` o `medusa` para intentar romper credenciales.

#### 3.3. **Servicios de Base de Datos (MySQL, PostgreSQL)**

- **Acceso No Autenticado**: Verifica si hay configuraciones que permitan el acceso sin autenticación.
- **Inyecciones**: Realiza pruebas de inyección de SQL para obtener datos sensibles.

### 4. **Configuraciones Incorrectas**

Los servicios a menudo tienen configuraciones que pueden ser mal implementadas, lo que permite abusos. Por ejemplo:

- **Archivos de Configuración Expuestos**: Busca archivos de configuración que contengan credenciales y asegúrate de que no sean accesibles públicamente.
- **Permisos Incorrectos**: Revisa los permisos de los archivos de configuración y asegúrate de que sean restrictivos.

### 5. **Escalación de Privilegios**

Si logras comprometer un servicio, busca maneras de escalar privilegios. Esto puede incluir:

- **Servicios con Privilegios Elevados**: Si un servicio se ejecuta como `root`, intenta aprovecharlo para obtener una shell de superusuario.
- **SUID/GUID Binaries**: Busca binarios que tengan el bit SUID o SGID configurado. Si puedes ejecutar estos binarios, puedes ganar acceso elevado.

### 6. **Monitoreo de Actividades**

Mantente al tanto de cualquier actividad sospechosa en los servicios. Usa herramientas de monitoreo como `fail2ban` o `OSSEC` para detectar y alertar sobre comportamientos inusuales.

### 7. **Mitigación de Riesgos**

Para mitigar el abuso de servicios internos:

- **Actualizar y Parchar**: Mantén todos los servicios actualizados y aplica parches de seguridad regularmente.
- **Auditar Configuraciones**: Revisa las configuraciones de los servicios para asegurarte de que son seguras.
- **Restricciones de Red**: Implementa firewalls y controles de acceso a red para limitar el acceso a los servicios críticos.

### 8. **Herramientas Útiles**

- **Nmap**: Para escanear puertos y descubrir servicios activos.
- **Metasploit**: Para explotación de vulnerabilidades conocidas.
- **Burp Suite**: Para pruebas de seguridad de aplicaciones web.

--------

Si, por ejemplo, un servicio está corriendo de forma privilegiada en la máquina del servicio, como por ejemplo el servicio de Apache, de la siguiente manera, mediante una explotación del servicio `cmd`, podrías hacer una doble ejecución de comandos: primeramente desde el usuario explotado y posteriormente en el usuario `root`.

![[Pasted image 20241027193215.png]]

Por ejemplo, si existe un servicio configurado en el siguiente directorio

![[Pasted image 20241027193438.png]]

Con el siguiente comando de ejecución para ejecutar tareas cada x tiempo 

![[Pasted image 20241027193516.png]]

Y posteriormente otros archivo de configuración de servicios de tiempo:

![[Pasted image 20241027193600.png]]

De la siguiente manera, podríamos ver los servicios **timer** del sistema activo:

![[Pasted image 20241027193812.png]]

Con `pspy` también se podría realizar la monitorización de procesos y servicios en el sistema.

Si tienes permisos de lectura y escritura en la siguiente ruta, podrías hacer lo siguiente:

![[Pasted image 20241027194524.png]]

Crear un archivo 

![[Pasted image 20241027194606.png]]

Para cambiar los permisos de `bash` utilizando un script, puedes hacerlo en un archivo de script de shell. Aquí te muestro cómo se podría estructurar el script:

![[Pasted image 20241027194658.png]]

y cuando se complete el tiempo de actualización del servicio del sistema este script se ejecutara 
