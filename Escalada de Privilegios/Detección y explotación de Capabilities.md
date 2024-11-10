
-------

Las **Linux Capabilities** permiten asignar privilegios específicos a procesos sin otorgarles permisos de superusuario completo. Esto puede ser explotado si las capacidades están mal configuradas. Aquí tienes una guía para detectar y explotar vulnerabilidades relacionadas con ellas:

### 1. **Detección de Capabilities**

- **Listar capacidades en archivos ejecutables:** Usa el comando para buscar binarios que tengan capacidades especiales asignadas:
   
    `getcap -r / 2>/dev/null`
    
- **Ver las capacidades de un archivo específico:**

    `getcap /ruta/al/archivo`
    

### 2. **Tipos comunes de Capabilities peligrosas**

- `CAP_SETUID`: Permite cambiar la identidad del usuario.
- `CAP_NET_ADMIN`: Permite configurar la red, modificar rutas y dispositivos.
- `CAP_SYS_MODULE`: Permite cargar y descargar módulos del kernel.
- `CAP_DAC_OVERRIDE`: Bypassea los permisos de archivos, otorgando acceso que normalmente estaría restringido.

### 3. **Explotación de Capabilities**

- **Escalación de privilegios con `CAP_SETUID`:** Si un binario tiene esta capacidad, puedes intentar ejecutarlo para cambiar tu identidad a `root`:
    
    
    `./binario_con_setuid_capability`
    
- **Usar `CAP_DAC_READ_SEARCH` para leer archivos restringidos:** Si encuentras esta capacidad, puedes acceder a archivos que normalmente no podrías leer, como `/etc/shadow`.
- **Ejemplos con `CAP_NET_RAW`:** Esta capacidad permite capturar y manipular paquetes de red, lo que puede ser aprovechado para ataques de red.

### 4. **Mitigación**

- **Eliminar capacidades innecesarias:**
    
    `setcap -r /ruta/al/archivo`
    
- **Revisar y auditar capacidades:** Mantén un control regular de las capacidades asignadas a binarios para evitar configuraciones peligrosas.

### 5. **Herramientas útiles**

- **`getcap` y `setcap`**: Para listar y modificar capacidades.
- **`pwnkit`**: Usar si encuentras binarios con capacidades que permiten escalar privilegios (e.g., explotación conocida de `pkexec`).


--------

En este caso, vamos a jugar primeramente con las capabilities del primer binario, que sería **tcpdump**.

Por ejemplo, de la siguiente manera podríamos listar diferentes capabilities en el sistema según los directorios del sistema:

`getcap -r / 2>/dev/null`

Este comando nos permitirá identificar qué binarios tienen asignadas capabilities y, potencialmente, encontrar aquellos que podamos explotar para realizar acciones privilegiadas sin necesidad de ser `root`.

![[Pasted image 20241027174012.png]]

Y de la siguiente manera podríamos decodificar la capability para entender sus permisos específicos:



`getcap /ruta/al/binario`

Esto mostrará las capabilities asignadas al binario específico, permitiéndonos saber qué permisos adicionales tiene. Por ejemplo, si usamos:


`getcap /usr/sbin/tcpdump`

Podríamos ver una salida como:


`/usr/sbin/tcpdump = cap_net_raw,cap_net_admin+ep`

Esto indica que `tcpdump` tiene permisos para manejar operaciones de red (`cap_net_raw`, `cap_net_admin`), lo cual podría explotarse para realizar actividades de red privilegiadas.

![[Pasted image 20241027174108.png]]

Y de la siguiente manera podremos establecer capabilities en el binario `tcpdump`:

![[Pasted image 20241027174217.png]]

Y de la siguiente manera podremos ver desde la raíz qué capabilities se están ejecutando en el sistema:

![[Pasted image 20241027174321.png]]

Y una vez añadida la capability, ya nos dejaría ejecutar el comando `tcpdump` como un usuario no privilegiado.

Con el comando `ps -faux` en Docker, podremos ver el PID de un servicio ejecutándose, en este caso, `tcpdump`. Para hacerlo, usaríamos el siguiente comando:

`ps -faux | grep tcpdump`

### Explicación:

- `ps -faux`: Muestra todos los procesos en el sistema con información detallada.
- `grep tcpdump`: Filtra la salida para mostrar solo las líneas que contienen `tcpdump`, permitiéndonos identificar el PID y otros detalles del proceso.

Esto es útil para verificar que `tcpdump` se está ejecutando correctamente y para monitorear su actividad en el sistema.


![[Pasted image 20241027174558.png]]

Y con ese PID, podremos ver las capabilities que tiene montadas de la siguiente manera:

![[Pasted image 20241027174551.png]]

Otra capability vulnerable puede ser

![[Pasted image 20241027174857.png]]

![[Pasted image 20241027174923.png]]

TODAS LAS CAPABILITIES SE ENCUENTRAN REGISTRADAS EN LA SIGUIENTE PAGINA:

![[Pasted image 20241027175026.png]]


