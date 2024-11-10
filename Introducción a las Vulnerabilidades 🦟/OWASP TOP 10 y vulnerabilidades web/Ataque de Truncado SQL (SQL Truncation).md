
------

Un **ataque de truncado** es un tipo de vulnerabilidad que se aprovecha de cómo las aplicaciones manejan y almacenan cadenas de texto, especialmente en bases de datos. Sucede cuando se permite que datos más largos de lo esperado sean ingresados, y al ser truncados (cortados) al tamaño máximo permitido, esto provoca comportamientos inesperados o peligrosos. Aquí tienes una guía para entender este tipo de ataque:

### 1. ¿Qué es un ataque de truncado?

Un ataque de truncado ocurre cuando un sistema o base de datos acepta datos que son más largos de lo que debería, y los corta (trunca) para ajustarse a la longitud máxima permitida sin validar adecuadamente el contenido. Esto puede ser aprovechado por un atacante para manipular datos de forma malintencionada.

Por ejemplo, si una aplicación permite ingresar un nombre de usuario, y el campo tiene un límite de 10 caracteres, pero alguien intenta ingresar un nombre de 20 caracteres, los últimos 10 caracteres serán eliminados si no se maneja adecuadamente.

### 2. Ejemplo de un ataque de truncado

Supongamos que un sistema tiene un campo para nombres de usuario con un límite de 10 caracteres. Si un atacante intenta registrar el nombre `administradorX` (12 caracteres), y la base de datos lo trunca a `administrad`, podría causar problemas como:

- **Impersonación**: El atacante registra un nombre similar a un nombre de usuario legítimo para hacerse pasar por ese usuario. Por ejemplo, si `administrad` ya existe como nombre de usuario válido, la aplicación puede no manejar correctamente los duplicados y permitir la creación del nombre truncado.
- **Manipulación de datos**: Los campos truncados pueden permitir insertar comandos maliciosos o generar resultados inesperados si no se manejan adecuadamente.

### 3. ¿Dónde se producen ataques de truncado?

Los ataques de truncado suelen ocurrir en sistemas que:

- **No validan la longitud de entrada** antes de almacenar datos en la base de datos.
- **No verifican duplicados** después de truncar los datos.
- **No proporcionan mensajes de error adecuados** para entradas que superan la longitud permitida.

Pueden afectar a cualquier tipo de campo que tenga una longitud limitada, como nombres de usuario, correos electrónicos, contraseñas, entre otros.

### 4. Ejemplo de ataque práctico

Imagina una aplicación que usa una tabla de base de datos con un campo `username` limitado a 10 caracteres:

- Usuario real: `administrator` (11 caracteres, debería ser truncado a `administrato`).
- Si la aplicación no valida, el atacante registra `administratorX` (13 caracteres), que se trunca a `administrato`. Ahora, hay dos usuarios diferentes con el mismo nombre de usuario almacenado en la base de datos.

### 5. Cómo prevenir ataques de truncado

1. **Validar la longitud de las entradas**: Asegúrate de que la longitud de las entradas sea válida antes de intentar almacenarlas. No permitas que los datos se trunquen sin un manejo adecuado.
    
2. **Rechazar entradas demasiado largas**: En lugar de truncar, rechaza entradas que superen la longitud permitida e informa al usuario de la razón.
    
3. **Asegurar unicidad antes del truncado**: Si el campo necesita ser único (por ejemplo, un nombre de usuario), verifica esta unicidad antes de aceptar la entrada.
    
4. **Implementar límites de longitud consistentes**: Asegúrate de que los límites de longitud sean consistentes entre el frontend, el backend y la base de datos para evitar inconsistencias que puedan ser explotadas.
    
5. **Sanitización y escape de entradas**: Siempre es una buena práctica sanitizar y escapar las entradas para evitar inyecciones de código u otros ataques maliciosos que podrían combinarse con el truncado.
    

### 6. Conclusión

Los ataques de truncado pueden parecer menores, pero pueden tener graves implicaciones de seguridad. Evitarlos requiere validar y gestionar adecuadamente la longitud de las entradas de usuario y ser consciente de cómo los datos son manejados y almacenados por las aplicaciones.

¿Hay algo más específico sobre los ataques de truncado que te gustaría saber?

--------
Si al revisar el inspeccionar de la pagina vemos que en el formulario de entrada de un login php tiene esta variable adjunta que maxlength  podemos suponer un ataque de truncado:

![[Pasted image 20241019225742.png]]

podemos hacer lo siguiente para redirigirnos a un usuario en especifico con lo siguiente: _![[Pasted image 20241019230310.png]]_

Si podemos editar la varibale de lingitud por medio del codigo html, podremos hacere lo siguiente:
![[Pasted image 20241019230501.png]]

Podremos añadir mas caracteres y aca emepzaria el ataquye de truncado 

![[Pasted image 20241019230533.png]]

Asi pues la base de datos leeria los espacios despues de los validos uinsertados y los eliminaria dejando un el viejo usuario reegistrado con la clave del usuario nuevo ingresado en el apartado de register

Una vez dentro podemos configurar de la siguiente forma la terminal explotada 

![[Pasted image 20241019231140.png]]

![[Pasted image 20241019231159.png]]

![[Pasted image 20241019231213.png]]

