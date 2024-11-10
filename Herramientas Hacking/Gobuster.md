
-------

En Gobuster, suele enfocarse en técnicas de enumeración de directorios y subdominios para detectar rutas ocultas y subdominios en aplicaciones web y redes, lo cual es clave en la fase de recolección de información en un pentesting. Aquí tienes los comandos más comunes que usa para cada uno:

### Enumeración de Directorios (Modo Dir)

Para la búsqueda de directorios ocultos en aplicaciones web, el modo `dir` de Gobuster es una opción ideal. Esto permite encontrar rutas como `/admin`, `/backup`, y otras carpetas sensibles.

1. **Búsqueda Básica de Directorios**
    
    
    `gobuster dir -u <URL> -w <ruta/diccionario.txt>`
    
    Usa el diccionario indicado (`-w`), generalmente una lista común como `common.txt` o `directory-list-2.3-medium.txt` de SecLists.
    
2. **Búsqueda de Directorios con Extensiones**
    
    `gobuster dir -u <URL> -w <ruta/diccionario.txt> -x php,html,txt`
    
    Incluye extensiones específicas (`-x`) para encontrar archivos sensibles. Las extensiones como `php`, `html` y `txt` son muy comunes en aplicaciones web.
    
3. **Escaneo con Control de Tiempo (Timeout)**
    
    
    `gobuster dir -u <URL> -w <ruta/diccionario.txt> -t 50`
    
    Ajusta la cantidad de hilos (`-t 50`) para optimizar el rendimiento en función de la conexión y la velocidad del servidor. Esto puede acelerar el escaneo sin causar problemas en servidores lentos.
    
4. **Escaneo de Rutas Ocultas en HTTPS con Certificado SSL**
    
    
    `gobuster dir -k -u https://<URL> -w <ruta/diccionario.txt>`
    
    Usa `-k` para ignorar errores de certificado SSL y permitir el escaneo en servidores con certificados no válidos.
    
5. **Búsqueda de Directorios con Resultado Específico**
    
    
    `gobuster dir -u <URL> -w <ruta/diccionario.txt> --wildcard`
    
    Usa `--wildcard` para ignorar respuestas de wildcard, muy útil para evitar falsos positivos cuando un servidor responde con el mismo estado para cualquier directorio inexistente.
    

### Enumeración de Subdominios (Modo DNS)

Para descubrir subdominios, Gobuster usa el modo `dns`, que permite identificar subdominios potenciales de una organización.

1. **Búsqueda Básica de Subdominios**
    
    
    `gobuster dns -d <dominio.com> -w <ruta/diccionario_subdominios.txt>`
    
    Busca subdominios utilizando el diccionario indicado. `subdomains-top1million-110000.txt` de SecLists es comúnmente usado, ya que contiene una lista extensa de subdominios frecuentes.
    
2. **Escaneo de Subdominios con Resolvers Personalizados**
    
    
    `gobuster dns -d <dominio.com> -w <ruta/diccionario_subdominios.txt> -r 8.8.8.8,1.1.1.1`
    
    Define servidores DNS (`-r`) personalizados para evitar posibles bloqueos o restricciones. Es útil cuando los resolvers estándar tienen limitaciones.
    
3. **Escaneo de Subdominios con Verbosidad**
    

    
    `gobuster dns -d <dominio.com> -w <ruta/diccionario_subdominios.txt> -v`
    
    Usa el flag `-v` (verbose) para ver cada intento de subdominio en tiempo real, lo que ayuda a monitorear el progreso en vivo.
    
4. **Escaneo de Subdominios sin Registros de Wildcard**
    
    
    `gobuster dns -d <dominio.com> -w <ruta/diccionario_subdominios.txt> --wildcard`
    
    Evita falsos positivos al omitir respuestas que indiquen redirecciones de wildcard.
    

### Ejemplos Completos y Útiles para S4vitar

- **Búsqueda Completa de Directorios Común**
    

    `gobuster dir -u http://<dominio.com> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt -t 50`
    
- **Búsqueda Completa de Subdominios con Resolver y Wildcard**
    
    
    `gobuster dns -d <dominio.com> -w /usr/share/wordlists/dns/subdomains-top1million-110000.txt -r 8.8.8.8,1.1.1.1 --wildcard`
    

Con estos comandos de Gobuster, se logra una exploración detallada tanto de directorios como de subdominios, aprovechando diccionarios específicos y ajustando las opciones para mejorar la velocidad y precisión del escaneo.

En Gobuster, el modo **VHost** es ideal para descubrir **virtual hosts (vhosts)** en un servidor web. Los virtual hosts son configuraciones de servidores web que permiten alojar múltiples dominios o subdominios en una misma dirección IP. Esto es común en entornos de hosting compartido, donde varias aplicaciones web o sitios están en un mismo servidor, y cada uno tiene un vhost diferente.

### Comandos Comunes de Gobuster para Enumeración de VHosts

1. **Búsqueda Básica de Virtual Hosts**
    
    
    
    `gobuster vhost -u <IP o dominio base> -w <ruta/diccionario_vhosts.txt>`
    
    Aquí, `-u` indica la URL base (por ejemplo, `http://10.10.10.10` o `http://dominio.com`), y `-w` especifica el diccionario de vhosts. Diccionarios populares para esta tarea incluyen listas como `virtual-host-scanning.txt` de SecLists.
    
2. **Búsqueda de VHosts con Control de Errores de Certificado SSL**
    
    
    `gobuster vhost -u https://<dominio.com> -w <ruta/diccionario_vhosts.txt> -k`
    
    Usa `-k` para omitir errores de SSL, útil si el servidor tiene un certificado no válido. Esto permite que Gobuster siga escaneando sin problemas.
    
3. **Búsqueda de VHosts con Verbosidad**
    
    
    `gobuster vhost -u <IP o dominio base> -w <ruta/diccionario_vhosts.txt> -v`
    
    La opción `-v` (verbose) muestra cada intento en tiempo real, útil para monitorear el progreso de la enumeración de virtual hosts y revisar cada solicitud.
    
4. **Búsqueda de VHosts con un Límite de Conexiones Concurrentes**
    
    
    `gobuster vhost -u <IP o dominio base> -w <ruta/diccionario_vhosts.txt> -t 50`
    
    Ajusta el número de hilos (`-t`) para un escaneo más rápido o más controlado según el servidor.
    

### Ejemplo Completo de Comando para VHosts

Si quieres hacer un escaneo de vhosts en un servidor que pueda tener múltiples aplicaciones o sitios asociados:


`gobuster vhost -u http://10.10.10.10 -w /usr/share/wordlists/vhosts/virtual-host-scanning.txt -t 50 -v`

### Consideraciones al Escanear VHosts

- **Resolver DNS**: En algunos casos, es útil configurar un archivo `/etc/hosts` personalizado si el servidor no responde directamente a los vhosts sin el dominio base.
- **Detectar Servicios Compartidos**: Esto puede revelar sitios o aplicaciones web adicionales en la misma IP, que pueden ser puntos de entrada no detectados.

