
----------

### 1. **Entender Python Library Hijacking**

El Python Library Hijacking ocurre cuando un atacante reemplaza una biblioteca de Python legítima con una versión maliciosa que ejecuta código no autorizado. Esto puede permitir al atacante ejecutar comandos con los privilegios del usuario que ejecuta el script de Python.

### 2. **Detección de Oportunidades para Library Hijacking**

- **Identificar la Ruta de Búsqueda de Módulos**: Puedes verificar dónde Python busca los módulos utilizando el siguiente comando en Python:
    
    `import sys print(sys.path)`
    
- **Buscar Directorios Escribibles**: Revisa cada directorio en `sys.path` para encontrar aquellos donde tienes permisos de escritura. Por ejemplo:
    
    `for dir in $(python3 -c "import sys; print('\n'.join(sys.path))"); do     if [ -w "$dir" ]; then         echo "$dir es escribible"     fi done`
    

### 3. **Explotación de Library Hijacking**

- **Crear un Módulo Malicioso**: Si encuentras un directorio escribible, crea un módulo de Python que tenga el mismo nombre que una biblioteca que deseas reemplazar. Por ejemplo, si quieres reemplazar `os`, crea un archivo llamado `os.py`:
    
    
    `# os.py import os import subprocess  # Ejemplo de código malicioso que ejecuta un comando subprocess.call(["/bin/bash", "-i"])`
    
- **Reemplazar la Biblioteca Original**: Coloca este archivo `os.py` en el directorio escribible que se encuentra en `sys.path`.
    

### 4. **Ejecutar un Script de Python**

- **Probar el Comportamiento**: Ejecuta un script de Python que importe la biblioteca que has reemplazado. Por ejemplo:
    
    `import os`
    
- **Ejecutar el Comando Malicioso**: Al importar `os`, tu módulo malicioso se ejecutará en lugar de la biblioteca original, proporcionando acceso a una shell o ejecutando cualquier otro comando malicioso.
    

### 5. **Verificación de la Explotación**

- **Comprobar la Ejecución**: Verifica que el módulo malicioso se haya ejecutado correctamente al observar el comportamiento del script y los resultados de cualquier comando ejecutado.

### 6. **Mitigación**

- **Revisar y Limitar el PYTHONPATH**: Asegúrate de que el `PYTHONPATH` no incluya directorios inseguros o escribibles.
- **Uso de Entornos Virtuales**: Implementa entornos virtuales para mantener dependencias y bibliotecas separadas y seguras.
- **Auditar Bibliotecas**: Monitorea y audita regularmente las bibliotecas de Python en los sistemas para detectar cambios no autorizados.


------

Primeramente:

![[Pasted image 20241026202532.png]]

A nivel de `sudoers`, podemos poner la siguiente política para que el usuario `savitar` pueda ejecutar un script de Python al nivel de `manolito`:


![[Pasted image 20241026202743.png]]

Si digamos somos el usuario `manolito` y tenemos el siguiente script de Python configurado, podremos ver una cadena de MD5:

![[Pasted image 20241026203013.png]]

Entonces, como el usuario `s4vitar`, de la siguiente forma podríamos ver la ruta de importación de `PATH` que sigue Python:

![[Pasted image 20241026203316.png]]

Entonces, una vez viendo la ruta de `PATH` y confirmando que en las librerías Python primero busca en el directorio actual de trabajo, podremos secuestrar la biblioteca `hashlib` con nuestro propio código, indicándole que haga lo que nosotros queramos de la siguiente forma:

![[Pasted image 20241026203530.png]]

En el hashlib.py 

![[Pasted image 20241026203542.png]]

Posteriormente al ejecutarlo tendríamos acceso al usuario manolito 