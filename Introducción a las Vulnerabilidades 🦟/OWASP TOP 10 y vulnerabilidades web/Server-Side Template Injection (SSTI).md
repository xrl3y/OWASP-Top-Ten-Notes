
---------
#explotacion #owasp 

------------

**Server-Side Template Injection (SSTI)** es una vulnerabilidad que ocurre cuando una aplicación web incorpora entradas de usuarios de manera insegura dentro de plantillas que se interpretan en el lado del servidor. Esto permite a un atacante inyectar código malicioso en la plantilla y, potencialmente, ejecutar comandos en el servidor. La vulnerabilidad es peligrosa porque puede llevar a la ejecución remota de código (RCE) si no se trata adecuadamente.

### ¿Cómo ocurre?

Las aplicaciones modernas a menudo usan motores de plantillas para generar contenido dinámico. Los motores de plantillas permiten a los desarrolladores usar una sintaxis específica para combinar variables con texto estático y así crear páginas web personalizadas. Cuando la entrada del usuario se inserta sin la debida sanitización en la plantilla, un atacante puede inyectar instrucciones que el motor de plantillas interpretará como código a ejecutar en el servidor.

### Ejemplos de motores de plantillas comunes

- **Jinja2** (Python)
- **Thymeleaf** (Java)
- **Twig** (PHP)
- **Velocity** (Java)
- **Freemarker** (Java)

### Ejemplo de vulnerabilidad SSTI en Jinja2 (Python)

Supongamos que una aplicación usa Jinja2 para mostrar un saludo personalizado en su página web. El código del servidor podría verse así:


`from flask import Flask, request, render_template_string  app = Flask(__name__)  @app.route('/greet', methods=['GET']) def greet():     user_input = request.args.get('name')     template = f"Hello {user_input}!"     return render_template_string(template)  if __name__ == "__main__":     app.run()`

En este caso, el usuario puede ingresar su nombre como un parámetro de la URL, por ejemplo:


`http://localhost:5000/greet?name=John`

La salida sería:


`Hello John!`

Sin embargo, si el atacante inserta el siguiente payload en el parámetro `name`:

`http://localhost:5000/greet?name={{7*7}}`

La salida sería:


`Hello 49!`

El motor de plantillas de Jinja2 ha evaluado `{{7*7}}` como una expresión, revelando que la aplicación es vulnerable a SSTI. Un atacante más avanzado podría ejecutar código malicioso, como:


`http://localhost:5000/greet?name={{config.items()}}`

Esto podría devolver información confidencial sobre la configuración del servidor.

### Ejemplo de SSTI en Twig (PHP)

Twig es un motor de plantillas en PHP que también es vulnerable si la entrada de usuario se inserta de forma insegura. Supongamos que una aplicación PHP tiene el siguiente código:


`use Twig\Environment; use Twig\Loader\ArrayLoader;  $loader = new ArrayLoader([     'index' => 'Hello {{ name }}!', ]);  $twig = new Environment($loader);  $name = $_GET['name']; echo $twig->render('index', ['name' => $name]);`

Al ingresar:


`http://localhost/index.php?name={{7*7}}`

La salida sería:

`Hello 49!`

### Prevención de SSTI

1. **No usar entradas de usuario directamente en plantillas**. Siempre sanitizar y validar cualquier dato que provenga de entradas del usuario.
2. **Usar métodos seguros para la generación de plantillas**. Si se requiere insertar valores de usuario, usar funciones específicas que manejan los datos de forma segura.
3. **Configuración segura del motor de plantillas**. Algunos motores permiten configuraciones que restringen la ejecución de código o la carga de archivos maliciosos.


Siempre que haya python corriendo atras o flask probar hacer lo siguiente en imouts de texto:

![[Pasted image 20240930185418.png]]

si la operacion se hace entonces es vulnerable a la ssti , algunas de estas vulnerabilidades se encuentran en la pagina web **PayloadAllThetThings**, en la pagina web puedes ver ejemplos de las inyecciones de codigo mas frecuentes 