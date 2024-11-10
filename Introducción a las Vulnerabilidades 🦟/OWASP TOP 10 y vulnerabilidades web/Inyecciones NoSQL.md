
------------

La **inyección NoSQL** es un tipo de vulnerabilidad de seguridad que afecta a bases de datos NoSQL (como MongoDB, CouchDB, etc.), similar a la inyección SQL, pero se centra en bases de datos que no utilizan SQL como lenguaje de consulta. Los ataques de inyección NoSQL permiten a los atacantes manipular las consultas a la base de datos de una aplicación web para acceder a datos no autorizados o realizar operaciones no permitidas.

### ¿Cómo ocurre una inyección NoSQL?

La inyección NoSQL ocurre cuando una aplicación no valida o escapa adecuadamente los datos proporcionados por el usuario antes de incluirlos en una consulta de la base de datos. Si los datos de entrada no son controlados, un atacante puede inyectar código malicioso para alterar la consulta.

### Ejemplo básico de inyección NoSQL en MongoDB

Supongamos que tienes una aplicación que permite a los usuarios iniciar sesión mediante su nombre de usuario y contraseña. La aplicación podría realizar una consulta MongoDB como la siguiente:


`db.users.findOne({username: inputUsername, password: inputPassword});`

Aquí, `inputUsername` y `inputPassword` son datos que el usuario proporciona a través de un formulario de inicio de sesión.

Si el atacante introduce un valor malicioso como el siguiente:


`inputUsername = "admin"; inputPassword = { "$ne": null };`

La consulta MongoDB se transformaría en algo como:


`db.users.findOne({username: "admin", password: { "$ne": null }});`

Este tipo de consulta devolverá cualquier usuario llamado "admin" sin importar el valor real de la contraseña, ya que la condición `"$ne": null` (no es nulo) siempre será verdadera.

### Impacto

- **Acceso no autorizado**: El atacante podría iniciar sesión como otro usuario sin conocer la contraseña.
- **Exfiltración de datos sensibles**: El atacante puede modificar las consultas para obtener datos a los que no debería tener acceso.
- **Manipulación de datos**: Un atacante podría inyectar consultas maliciosas que alteren, eliminen o inserten datos de manera no autorizada.

### Medidas de prevención

1. **Validación y sanitización de entradas**: Nunca confíes en los datos proporcionados por el usuario. Asegúrate de validar y sanear todos los datos de entrada antes de usarlos en consultas.
    
2. **Uso de operadores seguros**: Utiliza operadores y funciones que traten las entradas del usuario de manera segura, como métodos de consulta que no permitan la inyección directa.
    
3. **Uso de bibliotecas ORM/ODM**: Las bibliotecas de mapeo objeto-documento (ODM), como Mongoose para MongoDB, ofrecen abstracciones seguras que ayudan a prevenir la inyección NoSQL al estructurar las consultas de manera controlada.
    

### Ejemplo seguro usando Mongoose

Con Mongoose, un código más seguro para la autenticación podría ser:


`User.findOne({ username: req.body.username, password: req.body.password })   .then(user => {     if (user) {       // iniciar sesión     } else {       // credenciales incorrectas     }   })   .catch(err => {     // manejar errores   });`

Aquí, Mongoose gestiona las consultas y sanitiza los valores de entrada, lo que ayuda a prevenir ataques de inyección NoSQL.

### Pruebas de inyección NoSQL

- **Pruebas manuales**: Intenta enviar payloads maliciosos como `"$ne": null`, `"$gt": ""`, o `"$or": [{}, {}]` en las entradas del usuario.
- **Herramientas de seguridad**: Herramientas como Burp Suite o OWASP ZAP pueden ser útiles para detectar vulnerabilidades de inyección NoSQL en aplicaciones web.

### Ejemplo adicional de payload NoSQL

Si un formulario de búsqueda en una aplicación permite a los usuarios buscar productos por nombre:


`db.products.find({ name: userInput });`

Un atacante podría enviar un payload como:


`userInput = { "$ne": "" }`

Esto devolvería todos los productos en la base de datos, eludiendo las restricciones normales.


-----------

**PAYLOAD ALL THE THINGS** -- Pagina para payloads de diferentes vulnerabilidades

![[Pasted image 20241009155646.png]]

en la imagen se explica que el usuario es admin pero la contrtaseña no es admin con el tipo de datos json 

con la funcion  rejex puedes probar usuario que empiecen con una letra y despues tirar por intruder de buirpsuite para enumerar usuarios 

![[Pasted image 20241009160026.png]]

Cantidad de caracteres se puede ver con:
![[Pasted image 20241009160346.png]]

un script de python para poder ver esto podria ser:

![[Pasted image 20241009160924.png]]
![[Pasted image 20241009161337.png]]

otra forma de explotar esta vulnerabilñidad podria ser con el siguiente comando en un biuscado en una base de datos:

![[Pasted image 20241009161908.png]]

que basicamente recurre a la [[SQLI]] de forma booleana 