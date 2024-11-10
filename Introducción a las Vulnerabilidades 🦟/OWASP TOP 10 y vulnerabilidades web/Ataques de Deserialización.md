
----------

### Guía sobre Ataques de Deserialización

Los **ataques de deserialización** son un tipo de vulnerabilidad que se aprovecha de la forma en que una aplicación maneja los objetos deserializados, permitiendo a un atacante ejecutar código malicioso, comprometer datos o alterar el comportamiento de la aplicación.

#### Conceptos clave

- **Serialización**: Proceso de convertir un objeto (estructura de datos, objeto en memoria) en un formato que pueda ser almacenado o transmitido y posteriormente reconstruido (deserializado).
- **Deserialización**: El proceso inverso, donde un objeto serializado es convertido nuevamente en su forma original.
- **Deserialización insegura**: Ocurre cuando una aplicación deserializa datos sin verificar su integridad o autenticidad, permitiendo a los atacantes manipular la entrada para ejecutar código arbitrario.

#### Cómo funcionan los ataques de deserialización

Los ataques de deserialización ocurren cuando una aplicación permite la entrada de datos serializados provenientes de una fuente no confiable, como un usuario o cliente externo. Los atacantes pueden modificar estos datos antes de que sean deserializados, lo que puede permitir la ejecución de código no autorizado o la modificación del estado interno de la aplicación.

Los lenguajes que más comúnmente son afectados por estos ataques incluyen Java, PHP, Python, y .NET, ya que todos estos lenguajes tienen mecanismos integrados de serialización y deserialización.

#### Ejemplo de deserialización insegura en Java

Supongamos que una aplicación serializa objetos de usuario y los envía entre el cliente y el servidor. El código vulnerable podría ser algo así:

java

Copy code

`ObjectInputStream ois = new ObjectInputStream(new FileInputStream("user.obj")); User user = (User) ois.readObject();`

En este caso, el archivo "user.obj" es deserializado sin ninguna validación. Un atacante podría manipular este archivo para ejecutar comandos maliciosos.

#### Ejemplo de ataque en Java

El atacante puede crear un objeto malicioso que explote la deserialización insegura. En Java, las vulnerabilidades comúnmente explotan clases que tienen métodos como `readObject()`, `readResolve()` o `writeReplace()`, que permiten ejecutar código durante el proceso de deserialización.

Por ejemplo, usando bibliotecas populares con vulnerabilidades conocidas, como `Commons Collections` o `Jython`, un atacante puede generar una carga útil maliciosa.

java

Copy code

`ObjectInputStream ois = new ObjectInputStream(maliciousPayload); Object object = ois.readObject();  // Ejecuta el código del atacante`

El objeto deserializado podría ejecutar comandos en el servidor o modificar el comportamiento de la aplicación, resultando en un compromiso total del sistema.

#### Ejemplo de deserialización insegura en PHP

En PHP, la función `unserialize()` es usada para deserializar objetos. Un código vulnerable podría ser el siguiente:

php

Copy code

`$user = unserialize($_POST['user']);`

Si un atacante controla el valor de `$_POST['user']`, puede modificar el contenido serializado para ejecutar un ataque.

**Carga útil maliciosa de ejemplo**:

Si la aplicación utiliza una clase con un método mágico, como `__wakeup()` o `__destruct()`, un atacante puede inyectar código malicioso que se ejecute cuando el objeto sea deserializado.

php

Copy code

`O:8:"UserData":1:{s:4:"name";s:13:"malicious code";}`

El código inyectado puede ser ejecutado cuando el objeto se deserializa, permitiendo al atacante ejecutar código en el servidor.

#### Consecuencias de los ataques de deserialización

1. **Ejecución de código remoto (RCE)**: Un atacante puede inyectar objetos que, al deserializarse, ejecuten comandos maliciosos en el sistema afectado.
2. **Evasión de autenticación**: Los objetos deserializados pueden representar datos sensibles, como tokens de autenticación o roles de usuario, lo que permite al atacante manipular estos datos para obtener acceso no autorizado.
3. **Robo o modificación de datos**: Al modificar objetos serializados, un atacante puede alterar el comportamiento de la aplicación, cambiar estados o acceder a información privada.
4. **Denegación de servicio (DoS)**: Un atacante puede crear objetos que, al deserializarse, consuman recursos de manera descontrolada, lo que podría resultar en una sobrecarga del sistema.

#### Estrategias de mitigación

1. **Validar y verificar datos deserializados**: No confíes en los datos deserializados provenientes de fuentes no confiables. Implementa validación para garantizar que solo datos válidos sean procesados.
2. **Evitar la deserialización de datos no confiables**: La forma más segura de prevenir estos ataques es no deserializar datos provenientes de usuarios no confiables o externos.
3. **Usar mecanismos de control de tipos**: Algunos lenguajes permiten restringir qué clases pueden ser deserializadas. En Java, por ejemplo, puedes usar `ObjectInputStream` con una lista permitida de clases.
4. **Firmar los objetos serializados**: Usa firmas criptográficas para verificar la autenticidad e integridad de los objetos serializados antes de deserializarlos.
5. **Deserialización segura con bibliotecas**: Usa bibliotecas que tengan mitigaciones contra deserialización insegura. Por ejemplo, en Java, puedes usar bibliotecas como `Kryo` o `Gson` en lugar de `Java Serialization`.

#### Ejemplo de mitigación en Java

Para restringir la deserialización en Java, puedes sobrescribir el método `resolveClass()` en `ObjectInputStream` para permitir solo clases específicas:

java

Copy code

`ObjectInputStream ois = new ObjectInputStream(new FileInputStream("user.obj")) {     @Override     protected Class<?> resolveClass(ObjectStreamClass desc) throws IOException, ClassNotFoundException {         if (!desc.getName().equals("com.example.User")) {             throw new InvalidClassException("Unauthorized deserialization attempt", desc.getName());         }         return super.resolveClass(desc);     } };`

Este enfoque restringe qué clases pueden ser deserializadas, previniendo la ejecución de código malicioso.

#### Herramientas para detectar deserialización insegura

1. **Burp Suite**: Herramienta que permite interceptar y modificar datos serializados en solicitudes web para intentar exploits de deserialización.
2. **ysoserial**: Herramienta que genera cargas útiles maliciosas para explotar vulnerabilidades de deserialización en aplicaciones Java.
3. **PHPGGC**: Herramienta que genera cadenas de gadgets maliciosos para explotar vulnerabilidades de deserialización en aplicaciones PHP.

### Conclusión

Los ataques de deserialización son una amenaza seria, especialmente en aplicaciones que procesan objetos serializados de usuarios externos o no confiables. Mitigar estos riesgos requiere una combinación de buenas prácticas de validación de datos, control de tipos, y la implementación de mecanismos de seguridad adicionales, como la firma de objetos o el uso de bibliotecas más seguras para la serialización/deserialización.

4o### Guía sobre Ataques de Deserialización

Los **ataques de deserialización** son un tipo de vulnerabilidad que se aprovecha de la forma en que una aplicación maneja los objetos deserializados, permitiendo a un atacante ejecutar código malicioso, comprometer datos o alterar el comportamiento de la aplicación.

#### Conceptos clave

- **Serialización**: Proceso de convertir un objeto (estructura de datos, objeto en memoria) en un formato que pueda ser almacenado o transmitido y posteriormente reconstruido (deserializado).
- **Deserialización**: El proceso inverso, donde un objeto serializado es convertido nuevamente en su forma original.
- **Deserialización insegura**: Ocurre cuando una aplicación deserializa datos sin verificar su integridad o autenticidad, permitiendo a los atacantes manipular la entrada para ejecutar código arbitrario.

#### Cómo funcionan los ataques de deserialización

Los ataques de deserialización ocurren cuando una aplicación permite la entrada de datos serializados provenientes de una fuente no confiable, como un usuario o cliente externo. Los atacantes pueden modificar estos datos antes de que sean deserializados, lo que puede permitir la ejecución de código no autorizado o la modificación del estado interno de la aplicación.

Los lenguajes que más comúnmente son afectados por estos ataques incluyen Java, PHP, Python, y .NET, ya que todos estos lenguajes tienen mecanismos integrados de serialización y deserialización.

#### Ejemplo de deserialización insegura en Java

Supongamos que una aplicación serializa objetos de usuario y los envía entre el cliente y el servidor. El código vulnerable podría ser algo así:

java

Copy code

`ObjectInputStream ois = new ObjectInputStream(new FileInputStream("user.obj")); User user = (User) ois.readObject();`

En este caso, el archivo "user.obj" es deserializado sin ninguna validación. Un atacante podría manipular este archivo para ejecutar comandos maliciosos.

#### Ejemplo de ataque en Java

El atacante puede crear un objeto malicioso que explote la deserialización insegura. En Java, las vulnerabilidades comúnmente explotan clases que tienen métodos como `readObject()`, `readResolve()` o `writeReplace()`, que permiten ejecutar código durante el proceso de deserialización.

Por ejemplo, usando bibliotecas populares con vulnerabilidades conocidas, como `Commons Collections` o `Jython`, un atacante puede generar una carga útil maliciosa.

java

Copy code

`ObjectInputStream ois = new ObjectInputStream(maliciousPayload); Object object = ois.readObject();  // Ejecuta el código del atacante`

El objeto deserializado podría ejecutar comandos en el servidor o modificar el comportamiento de la aplicación, resultando en un compromiso total del sistema.

#### Ejemplo de deserialización insegura en PHP

En PHP, la función `unserialize()` es usada para deserializar objetos. Un código vulnerable podría ser el siguiente:

php

Copy code

`$user = unserialize($_POST['user']);`

Si un atacante controla el valor de `$_POST['user']`, puede modificar el contenido serializado para ejecutar un ataque.

**Carga útil maliciosa de ejemplo**:

Si la aplicación utiliza una clase con un método mágico, como `__wakeup()` o `__destruct()`, un atacante puede inyectar código malicioso que se ejecute cuando el objeto sea deserializado.

php

Copy code

`O:8:"UserData":1:{s:4:"name";s:13:"malicious code";}`

El código inyectado puede ser ejecutado cuando el objeto se deserializa, permitiendo al atacante ejecutar código en el servidor.

#### Consecuencias de los ataques de deserialización

1. **Ejecución de código remoto (RCE)**: Un atacante puede inyectar objetos que, al deserializarse, ejecuten comandos maliciosos en el sistema afectado.
2. **Evasión de autenticación**: Los objetos deserializados pueden representar datos sensibles, como tokens de autenticación o roles de usuario, lo que permite al atacante manipular estos datos para obtener acceso no autorizado.
3. **Robo o modificación de datos**: Al modificar objetos serializados, un atacante puede alterar el comportamiento de la aplicación, cambiar estados o acceder a información privada.
4. **Denegación de servicio (DoS)**: Un atacante puede crear objetos que, al deserializarse, consuman recursos de manera descontrolada, lo que podría resultar en una sobrecarga del sistema.

#### Estrategias de mitigación

1. **Validar y verificar datos deserializados**: No confíes en los datos deserializados provenientes de fuentes no confiables. Implementa validación para garantizar que solo datos válidos sean procesados.
2. **Evitar la deserialización de datos no confiables**: La forma más segura de prevenir estos ataques es no deserializar datos provenientes de usuarios no confiables o externos.
3. **Usar mecanismos de control de tipos**: Algunos lenguajes permiten restringir qué clases pueden ser deserializadas. En Java, por ejemplo, puedes usar `ObjectInputStream` con una lista permitida de clases.
4. **Firmar los objetos serializados**: Usa firmas criptográficas para verificar la autenticidad e integridad de los objetos serializados antes de deserializarlos.
5. **Deserialización segura con bibliotecas**: Usa bibliotecas que tengan mitigaciones contra deserialización insegura. Por ejemplo, en Java, puedes usar bibliotecas como `Kryo` o `Gson` en lugar de `Java Serialization`.

#### Ejemplo de mitigación en Java

Para restringir la deserialización en Java, puedes sobrescribir el método `resolveClass()` en `ObjectInputStream` para permitir solo clases específicas:

java

Copy code

`ObjectInputStream ois = new ObjectInputStream(new FileInputStream("user.obj")) {     @Override     protected Class<?> resolveClass(ObjectStreamClass desc) throws IOException, ClassNotFoundException {         if (!desc.getName().equals("com.example.User")) {             throw new InvalidClassException("Unauthorized deserialization attempt", desc.getName());         }         return super.resolveClass(desc);     } };`

Este enfoque restringe qué clases pueden ser deserializadas, previniendo la ejecución de código malicioso.

#### Herramientas para detectar deserialización insegura

1. **Burp Suite**: Herramienta que permite interceptar y modificar datos serializados en solicitudes web para intentar exploits de deserialización.
2. **ysoserial**: Herramienta que genera cargas útiles maliciosas para explotar vulnerabilidades de deserialización en aplicaciones Java.
3. **PHPGGC**: Herramienta que genera cadenas de gadgets maliciosos para explotar vulnerabilidades de deserialización en aplicaciones PHP.

### Conclusión

Los ataques de deserialización son una amenaza seria, especialmente en aplicaciones que procesan objetos serializados de usuarios externos o no confiables. Mitigar estos riesgos requiere una combinación de buenas prácticas de validación de datos, control de tipos, y la implementación de mecanismos de seguridad adicionales, como la firma de objetos o el uso de bibliotecas más seguras para la serialización/deserialización.

----------

transporte de objetos // entidades o atributos / Sistemas de Gestión 

Maquina cereal 1 de vulhub si quieres practicar la vulnerabilidad 

si al recibir la peticion por buprsuite esta aparece con una variable de tipo **obj** (objeto) quiere decir que posiblemente es vulnerbale a atqurs de deserializacion 

![[Pasted image 20241010224636.png]]![[Pasted image 20241010224636.png]]


![[Pasted image 20241010230133.png]]
