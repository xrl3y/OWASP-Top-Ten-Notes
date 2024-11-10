
---------

El abuso de binarios con el bit SUID (Set User ID) es una técnica común para escalar privilegios en sistemas Linux. Los archivos con SUID permiten que se ejecuten con los permisos del propietario del archivo, independientemente de quién los ejecute. Aquí tienes una guía para identificar y explotar binarios SUID:

### 1. **Identificación de Binarios SUID**

- **Buscar Binarios SUID**: Usa el siguiente comando para listar todos los archivos con el bit SUID activo en el sistema:
    
    
    `find / -perm -4000 2>/dev/null`
    
- **Filtrar Binarios Inusuales**: Los binarios SUID comunes incluyen `/bin/passwd`, `/bin/su`, `/usr/bin/sudo`, etc. Presta atención a binarios menos comunes o personalizados, ya que pueden tener vulnerabilidades explotables.

### 2. **Comprender los Riesgos de SUID**

- **Permitir Acceso a Privilegios Elevados**: Los binarios SUID se ejecutan con los privilegios del usuario propietario (a menudo root). Si un binario SUID es explotable, podría permitir la ejecución de comandos como root, lo que resulta en escalamiento de privilegios.
- **Explorar Funcionalidades de Comando**: Algunos binarios SUID proporcionan funcionalidades que permiten ejecutar comandos arbitrarios o invocar shells con permisos elevados.

### 3. **Explotación Común de Binarios SUID**

- **Ejemplos de Binarios Comunes Vulnerables**:
    
    - **`/bin/bash` o `/bin/sh`**: Si se encuentra con el bit SUID, puedes usarlo para abrir una shell root directamente:
        
    
        `/bin/bash -p`
        
    - **`/usr/bin/nmap`**: En versiones más antiguas, puedes usar la opción interactiva para lanzar un shell:
        

        `nmap --interactive`
        
        Luego, dentro del modo interactivo, usa `!sh` para abrir un shell.
    - **`/usr/bin/find`**:
    
        
        `find . -exec /bin/sh \; -quit`
        
    - **`/usr/bin/python`**:
    
        
        `python -c 'import os; os.system("/bin/sh")'`
        
- **Cargar Librerías Personalizadas (`LD_PRELOAD`)**:
    
    - Algunos binarios permiten cargar librerías compartidas personalizadas. Si un binario con SUID no limpia las variables de entorno (`LD_PRELOAD`, `LD_LIBRARY_PATH`), puedes cargar tu propia librería para obtener una shell con privilegios elevados.

### 4. **Escenarios de Explotación Específicos**

- **Modificar Archivos Sensibles**: Si puedes encontrar un binario que permita escribir en archivos críticos, podrías cambiar configuraciones del sistema, agregar usuarios a `/etc/passwd`, o modificar `/etc/sudoers` para ganar acceso root.
- **Abusar de Scripts Propietarios**: Los scripts o binarios personalizados que tienen el bit SUID son especialmente peligrosos, ya que podrían no estar diseñados teniendo en cuenta la seguridad y ser explotables fácilmente.

### 5. **Mitigación**

- **Eliminar SUID de Binarios Innecesarios**: Audita regularmente el sistema para eliminar el bit SUID de archivos que no necesiten tenerlo.
- **Usar Controles de Acceso**: Configura controles de acceso estrictos y limita el uso de scripts personalizados que requieran SUID.
- **Validar Entradas y Variables de Entorno**: Asegúrate de que los binarios SUID limiten el uso de variables de entorno peligrosas y que validen todas las entradas de forma segura.

Esta guía proporciona una visión básica para identificar y explotar binarios SUID vulnerables. Siempre realiza pruebas en entornos controlados y de manera ética.

--------
Estos permisos SUID afectan a nivel de sistema a todos los usuarios.
## Binario Base 64


Primero, verificamos qué permisos tiene el archivo ejecutando el siguiente comando:


`ls -l /ruta/del/archivo`

Esto mostrará detalles sobre el archivo, incluyendo si tiene el bit SUID activo, lo cual se indica con una "s" en los permisos (por ejemplo, `-rwsr-xr-x`).

![[Pasted image 20241026191221.png]]

Y posteriormente, le añadimos los permisos SUID al archivo para empezar con el laboratorio de la siguiente forma:


`chmod u+s /ruta/del/archivo`

Esto activará el bit SUID, permitiendo que el archivo se ejecute con los permisos del propietario, independientemente de quién lo ejecute.

![[Pasted image 20241026191321.png]]

De la siguiente manera podrías acceder a una ruta privilegiada usando el comando `base64` y el usuario `savitar`. En este caso, intentaremos leer el archivo `/etc/shadow`, donde se encuentran los usuarios con sus hashes de contraseñas, los cuales podríamos descifrar posteriormente para ejecutar ataques de fuerza bruta:


`sudo -u savitar base64 /etc/shadow`

Esto leerá el archivo `/etc/shadow` y lo mostrará codificado en base64, lo que puede ser útil para extraer información sensible sin tener permisos directos para visualizarlo.


![[Pasted image 20241026191555.png]]

Así es, también podrías usar el binario de `php` junto con rutas absolutas derivadas de enlaces simbólicos para leer archivos privilegiados. Si tienes permisos para ejecutar `php` con SUID o como otro usuario, puedes usar el siguiente comando para leer el archivo `/etc/shadow`:


`sudo /usr/bin/php -r 'echo file_get_contents("/etc/shadow");'`

Esto ejecuta un script de una línea que lee el contenido del archivo `/etc/shadow` y lo muestra en pantalla. Es una forma de usar `php` para acceder a archivos privilegiados sin tener permisos directos.

![[Pasted image 20241026192008.png]]

Con el siguiente comando puedes buscar desde la raíz archivos que tengan permisos SUID y así hacer un barrido justo después de explotar una máquina:


`find / -type f -perm -4000 2>/dev/null`

Este comando busca todos los archivos (`-type f`) que tienen el bit SUID activo (`-perm -4000`) en todo el sistema, comenzando desde la raíz (`/`). La salida de errores se redirige a `/dev/null` para que no se muestren los mensajes de permisos denegados al buscar en directorios restringidos. Esto te permitirá identificar binarios potencialmente vulnerables para escalar privilegios.

![[Pasted image 20241026192125.png]]

