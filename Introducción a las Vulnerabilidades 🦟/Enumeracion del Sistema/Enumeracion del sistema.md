
---------
Para realizar la enumeración del sistema en Linux, puedes utilizar la herramienta **LSE (Linux System Enumeration)**, disponible en el repositorio de GitHub: [LSE en GitHub](https://github.com/diego-treitos/linux-smart-enumeration).

### Pasos para usar LSE:

1. Dirígete al repositorio y haz clic en el script `.sh`.
2. Cambia el formato a **Raw**.
3. Descárgalo usando el siguiente comando en la consola:
    
    `wget <URL_del_script_raw>`
    
4. Asigna permisos de ejecución al archivo descargado con:
    
    `chmod +x <nombre_del_script.sh>`
    
5. Para usarlo como un comando integrado en el sistema, puedes moverlo a `/usr/bin`:

    `sudo mv <nombre_del_script.sh> /usr/bin/lse`
    
### Uso de LSE:

Para acceder al panel de ayuda y ver las opciones disponibles, puedes ejecutar:

`lse -h`

### Funcionalidades clave:

- Detectar **vías potenciales de elevación de privilegios**.
- Analizar **tareas cron** y tareas recurrentes que podrían ser vulnerables.
- Jugar con los niveles de análisis para obtener mayor profundidad, ajustando el nivel con `-l` para niveles más avanzados.

Este script es útil para evaluar posibles vectores de escalada de privilegios y analizar la configuración del sistema a diferentes profundidades.

```bash 
lse -l 2 
```

Otra herramienta que también puedes utilizar para la enumeración de sistemas en Linux es **LinEnum**. Esta herramienta realiza un análisis automático de posibles vectores de escalada de privilegios y configuraciones de seguridad.

### Uso de LinEnum:

Para emplear **LinEnum**, solo tienes que descargar el script desde su repositorio en GitHub. Después de darle permisos de ejecución, puedes ejecutarlo directamente para obtener información relevante sobre el sistema.

### Enumeración manual:

Si prefieres un enfoque manual, puedes investigar binarios como **python3.9** para comprobar que no tengan permisos **SUID** ni **capabilities**, los cuales podrían ser aprovechados para ejecutar consolas interactivas y escalar privilegios mediante la modificación del **UID** del usuario. Este es un método efectivo para obtener acceso privilegiado.

### Abuso de tareas cron:

Las **tareas cron** también pueden ser un vector de ataque. Si encuentras scripts o comandos ejecutados a intervalos regulares con permisos elevados, podrías modificarlos o inyectar código malicioso para aprovechar la escalada de privilegios.

### Monitoreo en tiempo real:

Si ya tienes acceso al sistema y puedes subir archivos, usar **pspy** es una excelente opción. Esta herramienta te permite monitorear en tiempo real los procesos que se están ejecutando en la máquina, lo que puede ayudarte a identificar actividades sospechosas o vulnerabilidades que puedas explotar.

### Creación de un script personalizado:

Puedes construir un script que te identifique en un ciclo todos los nuevos comandos ejecutados en el sistema utilizando herramientas como `inotifywait` o analizando el historial de comandos de los usuarios. Este enfoque te dará un control detallado de la actividad del sistema en tiempo real.

Estas técnicas te ayudarán a obtener una visión más completa de las posibles vulnerabilidades de un sistema Linux.

```bash
ps -eo user, command
```

De la siguiente forma:

![[Pasted image 20240918181424.png]]

Para encontrar todas las formas de explotación relacionadas con **SUID**, **capabilities**, **cron**, y otros vectores en los binarios de una máquina, puedes seguir los siguientes enfoques:

1. **Identificar archivos con el bit SUID**: Ejecuta este comando para listar todos los archivos que tienen el bit SUID activado, lo que podría permitir la escalada de privilegios:
    
    `find / -perm -4000 2>/dev/null`
    
2. **Ver capacidades de los binarios**: Las capacidades en binarios pueden proporcionar privilegios específicos que normalmente no tendrían. Puedes listar todas las capacidades asignadas con:
    
    `getcap -r / 2>/dev/null`
    
3. **Buscar tareas cron con privilegios elevados**: Para identificar tareas cron que se ejecutan con privilegios elevados, puedes inspeccionar los archivos cron y los **cron jobs** de usuarios y del sistema:
    
    `cat /etc/crontab ls -la /etc/cron.*`
    
4. **Buscar comandos recientemente ejecutados**: Un método para encontrar comandos ejecutados recientemente o cualquier actividad de cron o procesos es revisar los logs o usar herramientas como `pspy`:

    
    `tail -f /var/log/syslog`
    

Estas herramientas y técnicas te ayudarán a identificar potenciales vulnerabilidades en el sistema, como archivos con permisos inseguros, binarios con capacidades inapropiadas o tareas cron mal configuradas que podrían explotarse. Además existen también métodos de explotación en paginas como:

![[Pasted image 20240918181740.png]]