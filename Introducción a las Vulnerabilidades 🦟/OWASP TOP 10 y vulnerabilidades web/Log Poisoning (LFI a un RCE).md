
----------

Para explicar este concepto, utilizaremos cualquier contenedor de Docker que contenga una versión de Ubuntu disponible para la ejecución del siguiente script. Abrimos un servidor por el puerto 80:

![[Pasted image 20240924212718.png]]

Con este script podemos inyectar comandos de CMD directamente en la URL. El log poisoning debe interpretarse como un enfoque que apunta a diferentes rutas, en este caso a la `/var/log`, donde se encuentran diversos tipos de logs del sistema, como los servicios de Apache2.

Por ejemplo, en la ruta `/var/log/apache2` se encuentran los recursos de access.log, que al revisarlos mostrarían los registros de peticiones al servidor, incluyendo códigos de éxito como el 200 y códigos de error como el 400. Esto puede resultar en el envenenamiento de logs.

![[Pasted image 20240924213236.png]]

Si en el log se está dentro del grupo con ID = (adm), se podría acceder a la ruta de directorios deseada dentro de cualquier log del sistema. Para mostrar los resultados de un log poisoning, se cambian los permisos correspondientes.


![[Pasted image 20240924213744.png]]

Ahora que se sabe que se tiene acceso a los logs, si, por ejemplo, mediante una petición GET envías un comando interpretado por PHP, el log lo interpretará y tendrás una vía vulnerable para la ejecución de comandos.

![[Pasted image 20240924213851.png]]

El siguiente log te lista información del sistema mediante el servicio de PHP. Lo que quieres ver es la parte de **Disabled Functions**, ya que es donde te indicará de qué manera y qué comandos de ejecución los desarrolladores deshabilitaron para el sistema en cuestión.

![[Pasted image 20240924214212.png]]

Y el comando para activar la ejecución de comandos mediante POST con CMD sería el siguiente:

![[Pasted image 20240924214526.png]]--

--------

## SSH Log Poisoning

También se puede hacer a través de las peticiones SSH mediante el log de btmp, donde se almacena el registro de SSH. Si en el usuario de entrada inyectas código PHP, se podría ver referenciado en el log como una ejecución de comandos de la siguiente forma:

![[Pasted image 20240924214945.png]]

