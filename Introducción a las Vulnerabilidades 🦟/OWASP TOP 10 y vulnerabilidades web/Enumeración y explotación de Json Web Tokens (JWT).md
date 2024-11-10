
-------

### Guía de Introducción a la Enumeración y Explotación de JSON Web Tokens (JWT)

#### 1. **¿Qué es un JWT?**

Un **JSON Web Token (JWT)** es un estándar abierto (RFC 7519) que define una forma compacta y autónoma de transmitir información entre dos partes como un objeto JSON. Este objeto se firma digitalmente para garantizar la autenticidad e integridad de los datos.

Un JWT típico tiene tres partes codificadas en Base64 separadas por puntos (`.`):

- **Header**: contiene el tipo de token y el algoritmo de firma.
- **Payload**: contiene las reclamaciones (claims), es decir, la información que se quiere transmitir.
- **Signature**: se utiliza para verificar la autenticidad del token.

Formato de un JWT:

css

Copy code

`Header.Payload.Signature`

#### 2. **Estructura de un JWT**

- **Header**: Generalmente incluye el tipo de token y el algoritmo de firma.

json

Copy code

`{   "alg": "HS256",   "typ": "JWT" }`

- **Payload**: Contiene las **claims**. Algunas pueden ser predefinidas (como `iss`, `exp`, `sub`) o personalizadas.

json

Copy code

`{   "sub": "1234567890",   "name": "John Doe",   "admin": true }`

- **Signature**: Se genera combinando el header y payload codificados en base64 con una clave secreta utilizando el algoritmo especificado en el header.

#### 3. **Flujo básico de un JWT**

1. **Creación**: El servidor genera un JWT y lo firma.
2. **Transmisión**: El JWT se envía al cliente (por ejemplo, en la cabecera de autorización de una petición HTTP).
3. **Verificación**: Cuando el cliente envía el JWT de vuelta, el servidor verifica su autenticidad usando la firma.

#### 4. **Enumeración de JWTs**

La **enumeración** consiste en identificar y analizar los JWTs que se utilizan en una aplicación. En esta fase, los objetivos clave son:

- **Inspeccionar el contenido del JWT**: Decodifica el header y payload para analizar su estructura y el tipo de información que contiene. Herramientas como [jwt.io](https://jwt.io/) te permiten decodificar los tokens sin necesidad de claves.
    
    bash
    
    Copy code
    
    `echo '<JWT>' | cut -d "." -f1,2 | base64 -d`
    
- **Identificar el algoritmo de firma**: El algoritmo especificado en el header (`alg`) puede proporcionar pistas sobre posibles vulnerabilidades.
    

#### 5. **Vulnerabilidades comunes en JWTs**

1. **Uso del algoritmo `none`**: Este es uno de los ataques más comunes. Si el algoritmo es `none`, el token no se firma. Esto permite que un atacante modifique el payload del token sin necesidad de una clave secreta.
    
    - **Exploit**: Cambia el algoritmo a `none` y elimina la firma.
    
    json
    
    Copy code
    
    `{   "alg": "none",   "typ": "JWT" }`
    
    Luego, simplemente envías el JWT sin firma.
    
2. **Falsa firma con clave pública**: Si el servidor usa un algoritmo como `RS256` (firma asimétrica) pero acepta la clave pública como clave secreta, un atacante puede firmar el token con la clave pública.
    
    - **Exploit**: Generar un nuevo JWT firmado con la clave pública en lugar de la clave privada.
3. **Fuerza bruta de la clave secreta**: Si el JWT utiliza un algoritmo simétrico como `HS256`, puedes intentar realizar fuerza bruta para descubrir la clave secreta utilizada para firmar el token.
    
    - Herramientas como **`jwt-cracker`** o **John the Ripper** permiten forzar el valor de la clave secreta.
4. **Reutilización de tokens caducados**: Si el servidor no verifica correctamente la fecha de expiración (`exp`), un atacante podría reutilizar tokens viejos.
    

#### 6. **Explotación de JWT**

##### a) **Manipulación del Payload**

- Puedes manipular el contenido del JWT para intentar elevar privilegios (ej. cambiar el valor de `admin: false` a `admin: true`).
    
    Por ejemplo, si tienes un JWT válido con esta carga:
    
    json
    
    Copy code
    
    `{   "sub": "1234567890",   "name": "John Doe",   "admin": false }`
    
    Puedes cambiar `admin: false` a `admin: true`, volver a firmar el token con la misma clave secreta y enviarlo al servidor.
    

##### b) **Cambiar el algoritmo a `none`**

- Si la aplicación no maneja adecuadamente la verificación de la firma y permite el uso del algoritmo `none`, puedes cambiar el JWT de la siguiente forma:
    
    Header original:
    
    json
    
    Copy code
    
    `{   "alg": "HS256",   "typ": "JWT" }`
    
    Cambiado a:
    
    json
    
    Copy code
    
    `{   "alg": "none",   "typ": "JWT" }`
    
    Luego, elimina la firma y envía el token:
    
    bash
    
    Copy code
    
    `eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.`
    

##### c) **Cracking de JWT con `HS256`**

- Para crackear un JWT firmado con HS256, puedes usar una lista de palabras para intentar adivinar la clave secreta:
    
    bash
    
    Copy code
    
    `jwtcrack <JWT>`
    

#### 7. **Herramientas útiles**

- **jwt.io**: Una página web interactiva para decodificar, inspeccionar y firmar JWTs.
- **jwt-cracker**: Herramienta para crackear JWTs con claves débiles.
- **jwt_tool.py**: Una herramienta versátil en Python para pruebas de seguridad de JWTs.

#### 8. **Mitigaciones**

- **Usar algoritmos seguros**: Prefiere algoritmos asimétricos como `RS256` en lugar de simétricos como `HS256`.
- **Verificar siempre la firma**: Asegúrate de que el servidor siempre verifique la firma del token.
- **No aceptar el algoritmo `none`**.
- **Rotación de claves**: Implementa un sistema para rotar claves regularmente.
- **Verificación de la expiración**: Asegúrate de validar correctamente las fechas de expiración (`exp`).

### Conclusión

La enumeración y explotación de JWT puede ser una poderosa técnica en pruebas de seguridad. Al identificar vulnerabilidades comunes como el uso indebido de algoritmos de firma o la implementación débil de verificaciones, un atacante podría comprometer la seguridad del sistema. Sin embargo, con las medidas adecuadas, puedes fortalecer la seguridad de los sistemas que emplean JWTs.

-------

Cookie de sesion en formato de jason web token , encodeados en base 64 

![[Pasted image 20241024160040.png]]

El header son usuarios
payload : propiedades
signature: contraseña o firma digital 

![[Pasted image 20241024160218.png]]

Podemos ir creando un json web token de la siguiente forma:

## HEADER

Lo que se hace es decirle que no maneje ningun algoritmo de cifrado 

![[Pasted image 20241024160626.png]]

## Payload

Buscamos hacer el jwt para el id 2 

![[Pasted image 20241024160837.png]]

Juntando ambos sin el signature unidos por punto 

```
header.payload.
```

y lo pegamos y la pagina acepta el algoritmo none entonces podriamos entrar a la sesion del usuario 2

Si no acepta la peticion sin signature

![[Pasted image 20241024161310.png]]

Podriamos hacer lo siguiente:

![[Pasted image 20241024161506.png]]

Establecer que el secreto usado es *secret* que es una tipica vulnerabilidad tambien 

Pagina recomendada: *JWT.io* 

