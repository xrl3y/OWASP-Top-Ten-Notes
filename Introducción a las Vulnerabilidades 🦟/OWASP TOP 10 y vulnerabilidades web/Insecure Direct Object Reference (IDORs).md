
----------

### **Guía para Comprender y Mitigar Insecure Direct Object References (IDORs)**

#### 1. **¿Qué son los IDORs?**

Los **Insecure Direct Object References (IDORs)** son vulnerabilidades de seguridad que ocurren cuando una aplicación permite que los usuarios accedan a objetos o recursos internos (como archivos, registros de bases de datos, parámetros de URL, etc.) utilizando referencias directas sin verificar los permisos de acceso. Los atacantes pueden manipular estas referencias para acceder a datos de otros usuarios o realizar acciones no autorizadas.

#### 2. **Ejemplo de IDOR:**

Consideremos un sitio web donde un usuario puede ver los detalles de su cuenta accediendo a una URL como la siguiente:

bash

Copy code

`https://ejemplo.com/account/view?id=123`

En este caso, si el sistema no verifica que el ID `123` pertenece al usuario autenticado, un atacante podría modificar el número del ID en la URL:

bash

Copy code

`https://ejemplo.com/account/view?id=124`

Esto le permitiría acceder a la información de la cuenta de otro usuario con el ID `124`, lo que representa un claro caso de **IDOR**.

#### 3. **¿Por qué son peligrosos los IDORs?**

Los **IDORs** son especialmente peligrosos porque son fáciles de explotar y pueden dar acceso a datos sensibles o permitir la manipulación de recursos internos. Los efectos pueden incluir:

- **Robo de datos personales**: Acceso no autorizado a la información de otros usuarios.
- **Modificación de datos**: Cambiar o eliminar información crítica en el sistema.
- **Control de cuentas**: Tomar control de las cuentas de otros usuarios.

---

### **Mitigación de IDORs: Mejores Prácticas**

#### 1. **Validación de permisos**

- **Siempre valida los permisos del usuario autenticado antes de otorgar acceso** a cualquier recurso. Asegúrate de que el usuario tenga la autorización adecuada para acceder o modificar el recurso solicitado.
- Implementa verificaciones de acceso basadas en roles o listas de control de acceso (ACLs).

#### 2. **Evita referencias directas a objetos**

En lugar de exponer IDs sensibles (como números de cuenta, IDs de usuario o identificadores internos), utiliza **referencias indirectas** o **tokens** que no sean adivinables por el usuario. Por ejemplo, en lugar de:

bash

Copy code

`https://ejemplo.com/account/view?id=123`

Utiliza algo como:

arduino

Copy code

`https://ejemplo.com/account/view?token=abc123XYZ`

El token puede ser una referencia encriptada o un hash que no pueda ser manipulado fácilmente.

#### 3. **Uso de UUIDs en lugar de IDs secuenciales**

Los **UUIDs (Identificadores Universales Únicos)** son más difíciles de adivinar que los IDs secuenciales. Al usarlos, minimizas el riesgo de que un atacante pueda modificar un ID y acceder a datos sensibles. Por ejemplo, en vez de `id=123`, puedes usar algo como `id=a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11`.

#### 4. **Protege las APIs**

Los **IDORs** también son comunes en APIs REST. Asegúrate de implementar controles de acceso estrictos en todas las rutas de la API, y evita exponer referencias directas a objetos internos sin validación.

#### 5. **Implementa pruebas automatizadas de seguridad**

- **Escáneres de vulnerabilidades**: Utiliza herramientas automatizadas para identificar IDORs en tu aplicación.
- **Pruebas de penetración**: Realiza pruebas manuales que intenten identificar vulnerabilidades de IDOR en áreas clave de tu aplicación, como los sistemas de gestión de cuentas y recursos protegidos.

---

### **Recomendaciones adicionales:**

1. **Uso de ORM y frameworks seguros:** Las bibliotecas y frameworks modernos suelen incluir mecanismos para evitar IDORs. Aprovecha estas herramientas y sigue las mejores prácticas que recomiendan.
2. **Autenticación fuerte:** Asegúrate de que los sistemas de autenticación sean robustos, implementando multifactor authentication (MFA) y tokens de sesión que no puedan ser reutilizados.
3. **Registro y monitoreo:** Implementa un sistema de monitoreo que registre todos los accesos a recursos sensibles. Los logs deben alertar cuando un usuario intenta acceder a un recurso no autorizado.

---

### **Conclusión**

Los IDORs representan una amenaza significativa para la seguridad de las aplicaciones, pero con la implementación adecuada de controles de acceso, uso de referencias seguras y validaciones rigurosas, es posible mitigar eficazmente este riesgo. Aplicar estas mejores prácticas mejorará la seguridad de tu aplicación y protegerá los datos de tus usuarios.

-----

- Estas vulnerabilidades estas muy presentes en las paginas web del gobierno 

Se pueden enumerar diferentes objetos en la url que no aparecen en los aplicativos o en las opciones de diferentes elementos de una pagina web 

![[Pasted image 20241018142806.png]]

