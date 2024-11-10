
------

### Guía de Enumeración y Explotación de WebDAV

#### 1. **¿Qué es WebDAV?**

Web Distributed Authoring and Versioning (WebDAV) es una extensión del protocolo HTTP que permite a los usuarios gestionar y modificar archivos en servidores remotos. WebDAV facilita operaciones como crear, eliminar, mover y editar archivos de manera remota a través de una interfaz web, haciendo que los recursos compartidos en servidores web se comporten como si estuvieran en un sistema de archivos local.

#### 2. **¿Por qué es relevante en ciberseguridad?**

WebDAV, cuando está mal configurado, puede ser vulnerable a varios tipos de ataques. Los permisos de escritura mal gestionados pueden permitir a un atacante cargar archivos maliciosos, como web shells, que luego se pueden ejecutar para obtener control sobre el servidor.

### 3. **Fases de la explotación de WebDAV**

#### a) **Enumeración**

La fase de enumeración consiste en descubrir si el servidor objetivo tiene WebDAV habilitado y cuáles son sus configuraciones de permisos.

##### Herramientas comunes para enumeración:

- **`Nmap`**: Útil para detectar si WebDAV está activo en el servidor.
    
    bash
    
    Copy code
    
    `nmap -p80,443 --script=http-webdav-scan <IP-Objetivo>`
    
    Esto escaneará los puertos 80 y 443 y determinará si WebDAV está habilitado.
    
- **`Cadaver`**: Un cliente de WebDAV que permite conectarse a servidores WebDAV y enumerar archivos.
    
    bash
    
    Copy code
    
    `cadaver http://<IP-Objetivo>/webdav/`
    
    Aquí puedes probar varias rutas para ver si puedes listar y acceder a archivos.
    
- **`davtest`**: Herramienta de prueba de WebDAV que puede intentar subir archivos y determinar qué tipo de contenido acepta el servidor.
    
    bash
    
    Copy code
    
    `davtest -url http://<IP-Objetivo>/webdav/`
    
- **`Metasploit`**: También tiene módulos para enumerar y explotar WebDAV.
    
    bash
    
    Copy code
    
    `use auxiliary/scanner/http/webdav_scanner set RHOSTS <IP-Objetivo> run`
    

##### ¿Qué buscar durante la enumeración?

- Verifica si hay permisos de escritura activados.
- Intenta listar el contenido del directorio WebDAV.
- Comprueba si se pueden cargar o descargar archivos.

#### b) **Explotación**

Una vez que se ha verificado que WebDAV está habilitado y tiene permisos de escritura, el siguiente paso es intentar cargar un archivo malicioso (como una web shell) para obtener acceso remoto.

##### **Métodos de explotación**:

1. **Subir una Web Shell**: Si el servidor permite la carga de archivos y ejecución de scripts, puedes intentar subir una web shell.
    
    - **Pasos con `cadaver`**:
        
        bash
        
        Copy code
        
        `cadaver http://<IP-Objetivo>/webdav/ dav:/> put <path_to_webshell>`
        
        Intenta subir un archivo PHP, ASP o cualquier otro tipo de script que el servidor ejecute. Luego, intenta acceder a la URL del archivo cargado.
        
    - **Pasos con `davtest`**:
        
        bash
        
        Copy code
        
        `davtest -url http://<IP-Objetivo>/webdav/`
        
        Davtest intentará subir varios tipos de archivos maliciosos (PHP, ASP, etc.) y te mostrará cuál fue aceptado.
        
2. **Explotación con Metasploit**: Si prefieres usar un framework, Metasploit tiene módulos específicos para explotar WebDAV.
    
    bash
    
    Copy code
    
    `use exploit/multi/http/webdav_upload_asp set RHOST <IP-Objetivo> set TARGETURI /webdav/ run`
    
    Este módulo intentará cargar una web shell ASP en el servidor.
    
3. **Explotación manual**: Si prefieres hacerlo manualmente, simplemente sube el archivo malicioso y accede a la URL. Ejemplo con PHP:
    
    - Sube el archivo `webshell.php` y luego accede a `http://<IP-Objetivo>/webdav/webshell.php`.

#### c) **Post-explotación**

Una vez que has obtenido acceso a través de una web shell:

- **Escala privilegios**: Verifica si el usuario con el que has accedido tiene permisos limitados, y busca posibles vías para obtener privilegios más altos.
- **Limpieza**: Asegúrate de eliminar cualquier rastro de tus actividades para evitar detección si fuera necesario.
- **Exfiltración de datos**: Si tienes permisos suficientes, puedes extraer información sensible del servidor.

### 4. **Recomendaciones para proteger WebDAV**

Para evitar la explotación de WebDAV en un servidor:

- **Deshabilita WebDAV** si no es necesario.
- **Configura adecuadamente los permisos de escritura** para evitar cargas de archivos no autorizadas.
- **Utiliza autenticación robusta** para evitar accesos no deseados.
- **Implementa filtros de subida** que restrinjan los tipos de archivos que pueden subirse.
- **Monitorea los logs** del servidor en busca de actividades sospechosas relacionadas con WebDAV.

### 5. **Conclusión**

WebDAV es una funcionalidad útil, pero cuando no está bien configurada puede convertirse en una puerta de entrada para ataques graves. La enumeración y explotación de WebDAV permiten a un atacante manipular archivos en el servidor, cargar scripts maliciosos y obtener control remoto. Para los defensores, es crucial realizar una configuración adecuada y minimizar los permisos de escritura para evitar posibles abusos.

-----

Función Clean Docker

![[Pasted image 20241017155624.png]]

```bash
cleanDocker() {
    docker rm $(docker ps -a -q) --force 2> /dev/null
    docker rmi $(docker images -q) 2> /dev/null
    docker network rm $(docker network ls -q) 2> /dev/null
    docker volume rm $(docker volume ls -q) 2> /dev/null
}
```

