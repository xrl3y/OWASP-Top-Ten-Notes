
-----
### 1. **Identificación de Grupos Especiales**

Primero, necesitas identificar los grupos especiales a los que pertenece tu usuario. Estos grupos otorgan permisos adicionales que podrían ser explotados para obtener acceso privilegiado.


`id`

Este comando muestra los grupos a los que pertenece el usuario actual. Algunos grupos comunes que podrían ser aprovechados incluyen:

- **`sudo`**: Permite ejecutar comandos como superusuario.
- **`docker`**: Permite interactuar con contenedores Docker, que pueden ser utilizados para obtener una shell privilegiada.
- **`adm`**: Permite acceder a archivos de log del sistema, potencialmente revelando información sensible.
- **`disk`**: Acceso directo a dispositivos de almacenamiento.
- **`www-data`**: Acceso a archivos y procesos del servidor web.

### 2. **Abuso de Grupos Comunes**

#### 2.1. **Grupo `sudo`**

Si tu usuario pertenece al grupo `sudo`, puedes intentar ejecutar comandos como `root`:


`sudo su`

O intentar listar los comandos que puedes ejecutar sin contraseña:


`sudo -l`

Si hay algún comando que puedas ejecutar sin contraseña, puede ser explotado para ganar acceso privilegiado.

#### 2.2. **Grupo `docker`**

Si perteneces al grupo `docker`, puedes lanzar un contenedor con acceso al sistema de archivos del host y ganar acceso privilegiado:


`docker run -v /:/mnt --rm -it alpine chroot /mnt sh`

Este comando monta el sistema de archivos del host y te otorga una shell con acceso a todos los archivos.

#### 2.3. **Grupo `disk`**

Si tienes acceso a `disk`, podrías leer y escribir directamente en dispositivos de almacenamiento, permitiendo modificar archivos críticos del sistema, como `/etc/shadow` para cambiar contraseñas:


`dd if=/path/to/exploit of=/dev/sda1`

#### 2.4. **Grupo `adm` o `log`**

Los usuarios en estos grupos pueden leer archivos de log del sistema. Esto puede ser útil para revisar accesos, errores y otros datos sensibles:

`cat /var/log/auth.log`

Los logs pueden revelar contraseñas en texto plano, errores de autenticación, o comandos ejecutados por otros usuarios.

### 3. **Mitigación**

- **Revisar y limitar membresías en grupos especiales**: Solo los usuarios que realmente lo necesiten deben pertenecer a grupos como `docker`, `disk` o `sudo`.
- **Auditar permisos y accesos**: Realizar auditorías regulares para verificar quién tiene acceso y qué pueden hacer.
- **Actualizar y parchar**: Mantener el sistema actualizado para prevenir vulnerabilidades conocidas que puedan ser explotadas.

### 4. **Herramientas Útiles**

- **`enum4linux` y `linpeas`**: Herramientas que pueden ayudar a enumerar configuraciones del sistema y detectar grupos que puedan ser explotados.
- **`GTFOBins`**: Un sitio que muestra comandos que pueden ser explotados si tienes acceso a ciertos binarios con privilegios.

--------

De la siguiente manera podríamos darle el grupo `docker` a un usuario:

`usermod -aG docker nombre_usuario`

### Explicación:

- **`usermod`**: Comando para modificar la información de un usuario.
- **`-aG`**: Añade el usuario a un grupo sin eliminarlo de otros grupos a los que ya pertenece.
- **`docker`**: Nombre del grupo al que se agregará el usuario.
- **`nombre_usuario`**: Reemplazar con el nombre del usuario al que se le otorgará el acceso.

![[Pasted image 20241027184932.png]]

Digamos que, como usuario no privilegiado que pertenece al grupo `docker`, podríamos ejecutar el siguiente comando para iniciar un contenedor y obtener acceso:

![[Pasted image 20241027185324.png]]

Y de la siguiente manera, a través de montajes, podríamos copiar todo el acceso del host de `root` en el directorio `/mnt` de la montura de Docker y así tener acceso total del contenedor para la edición de permisos a nuestro antojo:

![[Pasted image 20241027185433.png]]

Como por ejemplo, le podríamos asignar permisos SUID a la shell de `bash` dentro del contenedor de Docker, y una vez que salgamos, el usuario no privilegiado tendría la posibilidad de abrir una shell privilegiada. Para hacerlo, seguiríamos estos pasos:

### 1. Asignar Permisos SUID a `bash`

Una vez dentro del contenedor, ejecutaríamos el siguiente comando:


`chmod u+s /bin/bash` 

![[Pasted image 20241027185543.png]]

Otro grupo vulnerable es `adm`, que te permite leer logs del sistema. Los usuarios que pertenecen a este grupo pueden acceder a archivos de registro críticos, lo que puede revelar información sensible sobre el sistema, actividades de usuarios y posibles vulnerabilidades.

![[Pasted image 20241027185639.png]]

De la siguiente forma y en el path de `/var/log/apache2`, podríamos ver información confidencial de los logs del sistema:


![[Pasted image 20241027185755.png]]

Otro grupo vulnerable también es el de `lxd`, que otorga permisos para gestionar contenedores LXD (Linux Container Daemon). Los usuarios que pertenecen a este grupo pueden crear, administrar y acceder a contenedores, lo que puede llevar a escalación de privilegios si se utiliza de manera malintencionada.

![[Pasted image 20241027190048.png]]



