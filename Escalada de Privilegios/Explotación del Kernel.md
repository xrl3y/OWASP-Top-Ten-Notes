
-------

### 1. **Reconocimiento**

- **Identificar la versión del kernel:**
    
    
    `uname -r`
    
- **Buscar vulnerabilidades conocidas:** Utiliza bases de datos como CVE o [Exploit-DB](https://www.exploit-db.com/) para encontrar vulnerabilidades específicas para tu versión del kernel.

### 2. **Enumeración de Vulnerabilidades**

- **Herramientas de escaneo:**
    - **LinPEAS:** Herramienta para enumerar posibles vectores de escalación de privilegios, incluyendo vulnerabilidades del kernel.
    - **PwnKit:** Conocida vulnerabilidad en `pkexec` que permite la escalación de privilegios.
- **Revisar configuraciones del kernel:** Asegúrate de que las configuraciones del kernel no estén expuestas a ataques. Por ejemplo, busca opciones de configuración que puedan ser explotadas.

### 3. **Explotación de Vulnerabilidades**

- **Ejecutar exploits disponibles:** Busca exploits específicos para la vulnerabilidad identificada y ejecútalos. Ejemplo:

    
    `./exploit`
    
- **Escritura de módulos del kernel:** Si tienes acceso al código fuente del kernel, podrías escribir un módulo que explote una vulnerabilidad específica.
- **Ejemplo de exploit conocido:** Algunos exploits públicos, como `Dirty COW`, son ejemplos de vulnerabilidades que permiten la escalación de privilegios. Investiga cómo funcionan y cómo se pueden aplicar.

### 4. **Verificación del éxito**

- **Verificar los permisos del usuario:** Una vez que se ha explotado la vulnerabilidad, verifica que el usuario tenga permisos de `root`:
    
    
    `whoami`
    
- **Comprobar acceso a archivos sensibles:** Intenta acceder a archivos que requieren privilegios elevados, como `/etc/shadow`.

### 5. **Mitigación**

- **Actualizar el kernel:** Mantén el kernel actualizado para cerrar posibles vulnerabilidades.
- **Configuraciones de seguridad:** Implementa medidas de seguridad como SELinux o AppArmor para limitar las capacidades de los procesos y prevenir la explotación de vulnerabilidades.

### 6. **Herramientas útiles**

- **Metasploit:** Framework que incluye múltiples exploits para vulnerabilidades del kernel.
- **KASLR (Kernel Address Space Layout Randomization):** Intenta mitigar la explotación de vulnerabilidades aleatorizando las direcciones de memoria.

-------

Después de fuzzear la página buscando archivos con extensiones `.sh` y encontrarlos, podemos probar la ejecución de comandos de la siguiente forma:

![[Pasted image 20241027182846.png]]

Si no se ve el output, entonces tendríamos que añadir un `echo` al final de la cadena para asegurarnos de que la salida del comando se muestre correctamente. Por ejemplo:

![[Pasted image 20241027182941.png]]

Y para establecer una shell interactiva podríamos intentar redirigir la ejecución para obtener acceso remoto. Por ejemplo, utilizando `bash` para abrir una reverse shell:

![[Pasted image 20241027183038.png]]

De la siguiente manera veremos la versión del kernel, que es bastante vieja:

![[Pasted image 20241027183207.png]]

Si el kernel es viejo, podremos buscar exploits conocidos para esa versión utilizando `searchsploit`:

![[Pasted image 20241027183332.png]]

![[Pasted image 20241027183342.png]]

En especial, el siguiente script puede sobrescribir el archivo `/etc/passwd` para agregar o modificar usuarios:

![[Pasted image 20241027183434.png]]

Lo que hará, una vez ejecutado en la máquina víctima, es sobrescribir el archivo `/etc/passwd`, añadiendo un usuario antes de la cuenta `root`. Esto hará que el nuevo usuario se comporte como `root`, obteniendo acceso completo al sistema. 