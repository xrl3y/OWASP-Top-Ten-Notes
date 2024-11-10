
--------

### 1. **Reconocimiento**

- **Enumeración de usuarios y roles:** Identifica los usuarios, roles y permisos disponibles en el sistema.
- **Análisis de rutas de acceso:** Busca endpoints, rutas o funciones que podrían ser accesibles sin la debida autenticación o autorización.

### 2. **Exploración de permisos**

- **Prueba de escalación vertical de privilegios:** Intenta acceder a funciones administrativas usando cuentas de usuario estándar.
- **Prueba de escalación horizontal de privilegios:** Intenta acceder a los recursos de otros usuarios (p. ej., modificar el perfil de otro usuario o ver sus datos).

### 3. **Explotación**

- **Manipulación de parámetros de ID:** Cambia identificadores en parámetros de solicitudes para ver si puedes acceder a recursos ajenos.
- **Uso de JWT o tokens manipulados:** Modifica los permisos codificados en tokens JWT para intentar obtener mayores privilegios.
- **Fuzzing de endpoints:** Usa herramientas como Burp Suite para explorar endpoints e identificar dónde faltan controles de autorización.

### 4. **Automatización**

- **SecLists:** Usa diccionarios para intentar bruteforce en rutas protegidas.
- **Herramientas de automatización:** Considera usar herramientas como `ffuf`, `gobuster`, o scripts personalizados para descubrir rutas accesibles.

### 5. **Mitigación**

- **Principio de menor privilegio:** Asegúrate de que los permisos se otorguen estrictamente en función de las necesidades.
- **Control de acceso basado en roles (RBAC):** Implementa políticas de acceso que aseguren que los roles solo tengan permisos específicos.
- **Validación y auditoría:** Realiza auditorías regulares para verificar los permisos y accesos otorgados en el sistema.

----------

Imaginemos que un archivo como `/etc/passwd` tiene permisos mal estructurados. Si los permisos permiten que cualquier usuario lo modifique, un atacante podría agregar nuevas cuentas de usuario con privilegios elevados o modificar las contraseñas de usuarios existentes. Esto representaría una grave vulnerabilidad de seguridad, ya que comprometería la integridad del sistema y facilitaría el acceso no autorizado.

![[Pasted image 20241027155358.png]]

Para crear una contraseña hasheada que pueda ser interpretada por `/etc/passwd`, puedes usar el siguiente comando con `openssl`:

![[Pasted image 20241027155545.png]]

Para ver archivos de escritura desde la raíz del sistema, puedes usar el siguiente comando en Linux:

`find / -type f -writable 2>/dev/null`

### Explicación:

- `find /`: Comienza la búsqueda desde la raíz (`/`).
- `-type f`: Busca solo archivos (excluyendo directorios).
- `-writable`: Filtra los archivos que tienen permisos de escritura para el usuario que ejecuta el comando.
- `2>/dev/null`: Redirige los mensajes de error (por ejemplo, de permisos denegados) a `/dev/null` para que no se muestren en la salida.Para ver archivos de escritura desde la raíz del sistema, puedes usar el siguiente comando en Linux:

![[Pasted image 20241027155728.png]]


Y viendo que podemos alterar el `/etc/passwd`, lo que hacemos es reemplazar la entrada correspondiente en este archivo. Una vez hecho, podremos ingresar al sistema utilizando la nueva contraseña establecida para el usuario `root`.

![[Pasted image 20241027155838.png]]

También, por ejemplo, podemos crear archivos que se ejecuten en nuestro directorio y, una vez creados, borrar los archivos originales del sistema. Luego, los reemplazamos con nuestros archivos, de modo que se utilicen como código fuente para la ejecución de comandos con privilegios de `root`. Esto permitiría inyectar código malicioso o manipular el comportamiento del sistema para obtener control total.