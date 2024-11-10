
------

**LDAP (Lightweight Directory Access Protocol)** es un protocolo utilizado para acceder y gestionar servicios de directorios en redes IP. Las inyecciones LDAP son un tipo de vulnerabilidad en aplicaciones web que permite a un atacante manipular consultas LDAP a través de entradas maliciosas.

#### Conceptos clave

- **LDAP Query**: Una consulta LDAP es usada para buscar, leer o modificar información en un servidor LDAP.
- **Inyección LDAP**: Se produce cuando los datos proporcionados por el usuario son incorporados directamente en una consulta LDAP sin la debida validación o escape, permitiendo que el atacante modifique la consulta para obtener acceso no autorizado a los datos o eludir controles de seguridad.

#### Funcionamiento de una inyección LDAP

Al igual que la inyección SQL, la inyección LDAP ocurre cuando una aplicación web inserta directamente datos proporcionados por el usuario en una consulta LDAP sin la validación adecuada. Esto permite al atacante modificar la estructura de la consulta LDAP para acceder o modificar información confidencial.

#### Ejemplo básico de código vulnerable

Un ejemplo típico de inyección LDAP ocurre cuando la aplicación acepta un nombre de usuario para autenticar a un usuario en el sistema:


`String filter = "(&(uid=" + user + ")(password=" + password + "))"; DirContext context = new InitialDirContext(env); context.search("ou=users,dc=example,dc=com", filter, controls);`

Aquí, los valores del `user` y `password` provienen de la entrada del usuario y se concatenan directamente en el filtro LDAP sin escape ni validación.

#### Ejemplo de ataque

Si un atacante ingresa en el campo de usuario el siguiente valor:


`*)(uid=*)`

La consulta LDAP se modificaría a:


`(&(uid=*)(uid=*)(password=password))`

Esto podría permitir eludir la autenticación porque `uid=*` coincide con cualquier usuario.

#### Mitigación de inyecciones LDAP

1. **Escape de caracteres especiales**: Asegúrate de escapar caracteres especiales que puedan tener significados en consultas LDAP, tales como `*`, `)`, `(`, `\`, y `/`.
    
2. **Validación de entrada**: Valida y sanitiza todos los datos proporcionados por el usuario antes de incorporarlos en la consulta.
    
3. **Usar consultas parametrizadas**: En lugar de construir dinámicamente las consultas LDAP, usa API que admitan consultas parametrizadas para evitar la manipulación de las entradas.
    
4. **Principio de mínimo privilegio**: Configura los servidores LDAP y las cuentas de acceso con los privilegios mínimos necesarios para reducir el impacto en caso de que ocurra una inyección.
    

#### Ejemplo seguro de construcción de consultas

`String filter = "(&(uid={0})(password={1}))"; SearchControls controls = new SearchControls(); String[] searchArgs = new String[]{user, password}; DirContext context = new InitialDirContext(env); context.search("ou=users,dc=example,dc=com", filter, searchArgs, controls);`

En este caso, la consulta es parametrizada y evita la concatenación directa de las entradas del usuario, protegiendo contra inyecciones LDAP.

#### Herramientas para detectar inyecciones LDAP

1. **Burp Suite**: Excelente herramienta para interceptar y modificar las solicitudes web para intentar diferentes vectores de inyección.
2. **OWASP ZAP**: Otra herramienta muy útil para analizar vulnerabilidades web, incluidas inyecciones LDAP.
3. **W3AF**: Framework de pruebas de seguridad que puede detectar inyecciones LDAP.

#### Consecuencias de las inyecciones LDAP

Una inyección LDAP exitosa podría permitir a un atacante:

- Eludir controles de autenticación.
- Acceder o modificar información en un servidor LDAP.
- Realizar búsquedas arbitrarias en el directorio LDAP.

Estas vulnerabilidades son particularmente peligrosas cuando los servidores LDAP almacenan información sensible, como credenciales de usuario o datos confidenciales de la organización.

### Ejemplos de inyecciones LDAP

#### Inyección de autenticación

Imaginemos que un campo de autenticación utiliza la siguiente consulta LDAP:


`String filter = "(&(uid=" + user + ")(password=" + password + "))";`

Si el atacante introduce:

- Usuario: `admin`
- Contraseña: `) (`

El filtro resultante sería:

`(&(uid=admin)(password=)(uid=))`

Esto puede permitir que el atacante acceda sin proporcionar una contraseña válida.

#### Inyección de búsqueda

Si la aplicación realiza búsquedas con un filtro como:


`String filter = "(cn=" + user + ")";`

Y el atacante introduce `*` en el campo de búsqueda, el filtro se convierte en:


`(cn=*)`

Esto devolvería todos los registros de la base de datos LDAP.

### Conclusión

Las inyecciones LDAP son un riesgo grave en aplicaciones web que no validan adecuadamente las entradas del usuario. Implementar prácticas seguras de programación, como el uso de consultas parametrizadas y la validación/escape de entradas, es esencial para proteger contra este tipo de vulnerabilidades.


---------

Scripts para enumerar Ldap

![[Pasted image 20241010215643.png]]

También se puede hacer lectura de archivos poniéndose en escucha en el puerto 443 de la siguiente forma:

![[Pasted image 20241010220306.png]]

