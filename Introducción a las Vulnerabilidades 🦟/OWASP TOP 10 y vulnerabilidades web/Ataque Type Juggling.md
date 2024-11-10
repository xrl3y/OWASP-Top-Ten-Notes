
---------

Un **ataque de Type Juggling** es una vulnerabilidad en la cual un atacante explota la manera en que un lenguaje de programación maneja o compara diferentes tipos de datos. Este tipo de ataque es común en lenguajes de programación como PHP, que pueden realizar conversiones automáticas entre tipos de datos como cadenas (strings), números enteros (integers), booleanos (boolean), entre otros.

El ataque ocurre porque el programa no distingue correctamente entre tipos de datos, lo que permite que un atacante manipule variables en un contexto específico para obtener un resultado no deseado, como acceder a partes no autorizadas del sistema o evadir medidas de seguridad.

### Ejemplo básico:

En PHP, si no se especifica correctamente el tipo de comparación (estricta vs. no estricta), pueden ocurrir estos problemas. Por ejemplo, al comparar una cadena `"0"` con un valor booleano `false` en una comparación no estricta (`==`), el resultado sería `true`, lo cual puede permitir un comportamiento no deseado.

`// Ejemplo de vulnerabilidad Type Juggling $password = "12345"; if ($_POST['password'] == $password) {     // acceso concedido }`

Un atacante podría enviar un valor como `"0"` en `$_POST['password']`, y debido a que PHP convierte tipos, la comparación sería verdadera porque `"0"` se evalúa como `false` y `false == false`.

### Guía para evitar un ataque Type Juggling:

1. **Usa comparaciones estrictas**: En lenguajes como PHP, usa la triple igualdad (`===`) para comparar valores. La triple igualdad compara tanto el valor como el tipo de los datos.
    

    `if ($_POST['password'] === $password) {     // acceso concedido solo si los valores y tipos coinciden }`
    
2. **Validación de entradas**: Asegúrate de validar y sanitizar adecuadamente las entradas de los usuarios. Por ejemplo, si esperas que un valor sea numérico, valida que realmente lo sea.

    
    `if (is_numeric($_POST['user_id'])) {     // procesa el ID del usuario }`
    
3. **Configuración estricta**: Configura tu entorno de desarrollo para forzar un manejo más estricto de los tipos de datos. En PHP, por ejemplo, puedes configurar el archivo `php.ini` para que lance advertencias si se realiza un Type Juggling accidental.
    
4. **Usa funciones de hashing seguras**: Si estás trabajando con contraseñas u otros datos sensibles, usa funciones de hashing como `password_hash()` y `password_verify()` en lugar de comparar cadenas de texto directamente.
    

    
    `$hash = password_hash("12345", PASSWORD_DEFAULT); if (password_verify($_POST['password'], $hash)) {     // acceso concedido }`
    
5. **Manejo adecuado de tipos**: Siempre especifica y controla los tipos de datos en las operaciones que realices, para evitar que el lenguaje haga conversiones implícitas no deseadas.


---------

basicamente si exite una mala configutacion en las variables de la contraseña, la puedes pasar por tipo de array en burpsuite asi como **password[ ]** , y de esa manera con la peticion bajo burpsuite te permitira ingresar al usuario 

de la sioguiente manera en desaroollo es mejor jugar con triple igual "= = =" al hacer la co,mprativa entew dos cariablesx o piede pasar ko sifguiebnte:

![[Pasted image 20241005204257.png]]

