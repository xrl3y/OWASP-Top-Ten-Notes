
---------

El abuso de privilegios a nivel de sudoers puede permitir a un atacante escalar sus privilegios en un sistema Linux. Aquí te dejo una guía concisa sobre cómo identificar y explotar configuraciones inseguras de `sudoers`:

### 1. **Comprender el Archivo `sudoers`**

- El archivo `/etc/sudoers` define qué usuarios tienen permisos para ejecutar comandos como otros usuarios (incluyendo root) sin necesidad de proporcionar la contraseña. Es importante entender cómo están configurados los permisos para detectar posibles debilidades.
- Usa `sudo -l` para listar los permisos sudoers que tiene un usuario. Esto mostrará los comandos que puede ejecutar sin necesidad de contraseña y bajo qué contexto.

### 2. **Identificación de Configuraciones Inseguras**

- **Permisos de Comandos Sin Contraseña (`NOPASSWD`)**: Si un comando puede ejecutarse sin contraseña, podría ser explotado si es vulnerable o permite acceso a otros recursos.
    - Ejemplo: `username ALL=(ALL) NOPASSWD: /bin/vi` permite abrir el editor `vi` como root sin contraseña. Usando `vi`, puedes invocar un shell y escalar privilegios.
- **Permisos de Comandos que Pueden Escalar Privilegios**: Algunos comandos que pueden ejecutarse como root sin restricciones tienen la capacidad de escalar privilegios, como `/bin/bash`, `/usr/bin/python`, `/usr/bin/perl`, `/usr/bin/vim`, entre otros.

### 3. **Explotación Común de Entradas Sudoers**

- **Ejecutar Comandos Interactivos**: Algunos comandos permiten invocar shells directamente. Por ejemplo:
    
    
    `sudo /usr/bin/python -c 'import os; os.system("/bin/bash")' sudo /usr/bin/perl -e 'exec "/bin/bash";' sudo /usr/bin/vim -c ':!bash'`
    
- **Utilizar Comandos con Funcionalidades Escalables**:
    - Si puedes usar `sudo` para ejecutar `tar`, `vim`, `less`, `nmap`, `find`, etc., podrías abusar de ellos para escalar privilegios.
    - **Ejemplo con `find`**:
        
    
        `sudo find . -exec /bin/sh \;`
        
    - **Ejemplo con `nmap`**:
        

        `sudo nmap --interactive`
        
        Luego, dentro del modo interactivo de `nmap`, usa `!sh` para obtener un shell.

### 4. **Escenarios de Explotación Específicos**

- **Editar Archivos de Configuración**: Si puedes usar `sudo` para editar archivos con `vi` o `nano`, podrías modificar archivos críticos de configuración (como `/etc/passwd` o `/etc/shadow`) para crear nuevos usuarios con permisos elevados.
- **Abuso de Scripts Propios y Binarios con SUID**: Si puedes ejecutar scripts o binarios propios como root, intenta modificar esos scripts para incluir comandos que abran un shell con privilegios.

### 5. **Mitigación**

- **Limitar Comandos que Pueden Ejecutarse con `sudo`**: Evitar el uso de `NOPASSWD` a menos que sea estrictamente necesario, y limitar los comandos que los usuarios pueden ejecutar a través de `sudo`.
- **Usar Restricciones de Opciones**: Especificar qué opciones pueden pasarse a los comandos cuando se usan con `sudo`, para evitar ejecuciones inesperadas.
- **Revisión Regular de la Configuración `sudoers`**: Auditar frecuentemente los permisos para detectar configuraciones inseguras y potenciales brechas.

Esta guía proporciona un resumen sobre cómo identificar y explotar posibles configuraciones inseguras en `sudoers`. Siempre actúa de forma ética y realiza estas pruebas en entornos donde tengas el permiso necesario para hacerlo.


---------

Lo normal es que una vez explotada la maquina tenemos que elevar nuestro privilegio y pasar a usuarios con mejores privilegios, no necesariamernte root pero si a un usuario con mejores privilegios.

##  nano /etc/sudoers

Primeramente se crea un usuario savitar con un directorio savitar y se le otorga una Shell de tipo  bash para iniciar con el laboratorio 

![[Pasted image 20241026185443.png]]

Lo mismo con un segundo usuario 

![[Pasted image 20241026185558.png]]

Después las contraseñas 

![[Pasted image 20241026185643.png]]

Para ver los privilegios que tengo como usuario no privilegiado se puede  usar:

```bash
sudo -l 
```

Se pueden definir políticas como el usuario privilegiado para un usuario no privilegiado como por ejemplo 

![[Pasted image 20241026185819.png]]

En la siguiente pagina de internet podremos ver los online dependiendo del comando bin a usar con los que podemos escalar privilegios:

![[Pasted image 20241026190022.png]]

Como por ejemplo el siguiente comando :

![[Pasted image 20241026190040.png]]

Con el comando nmap también se podría hacer de la siguiente forma:

![[Pasted image 20241026190516.png]]

