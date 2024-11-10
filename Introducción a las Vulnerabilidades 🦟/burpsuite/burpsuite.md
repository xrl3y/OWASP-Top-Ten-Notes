
-----------
#burpsuite #escucha #programa

Para inicializar el burpsuite en segundo plano y que así no dependa de un ciclo padre de ejecución se ejecuta el siguiente comando:

```bash 
burpsuite &> /dev/null/ & disown
```

**Burp Suite** es una herramienta popular utilizada para realizar pruebas de penetración en aplicaciones web, especialmente útil para identificar y explotar vulnerabilidades de seguridad. Esta guía rápida te ofrece una introducción para empezar a usarla:

1. **Configuración de proxy**: Burp Suite actúa como un intermediario entre el navegador y el servidor de la aplicación web. Primero, debes configurar el proxy de tu navegador (como Firefox) para que redirija el tráfico a través de Burp. Puedes usar extensiones como FoxyProxy para facilitar este proceso.
    
2. **Interceptar solicitudes**: Con Burp, puedes capturar, modificar y analizar las solicitudes y respuestas entre el navegador y el servidor. Para ello, navega a la pestaña "Proxy" y activa la opción "Interceptar", permitiéndote inspeccionar y manipular el tráfico antes de que llegue al servidor.
    
3. **Definir el alcance**: Es importante delimitar el ámbito de tus pruebas para evitar dañar accidentalmente otras aplicaciones. En la pestaña "Target", puedes añadir URLs específicas que deseas analizar, asegurando que solo realices pruebas en los sistemas para los que tienes autorización.
    
4. **Explorar el sitio**: Burp Suite incluye herramientas como el "Spider", que rastrea el sitio web automáticamente, recopilando todas las páginas y enlaces disponibles. Esta información aparece en el mapa del sitio ("Sitemap"), lo que te da una visión general de la estructura del sitio.
    
5. **Repetidor e intruso**: El "Repeater" te permite modificar manualmente las solicitudes HTTP y ver cómo responde el servidor, ideal para analizar comportamientos específicos. El "Intruder", por su parte, es útil para automatizar ataques como fuzzing, enviando múltiples cargas útiles para descubrir vulnerabilidades en los parámetros de la solicitud.
    
6. **Análisis y escaneo automatizado**: Las versiones Pro y Enterprise incluyen funciones avanzadas de escaneo automático, útiles para realizar pruebas de seguridad a gran escala sin intervención manual, ahorrando tiempo en pruebas repetitivas.
    

Para más detalles y configuraciones específicas, puedes consultar la [documentación oficial de Burp Suite](https://portswigger.net/burp/documentation)​([PortSwigger](https://portswigger.net/burp/documentation/desktop/getting-started))​([PortSwigger](https://portswigger.net/burp/documentation))​([Ceos3c](https://www.ceos3c.com/security/burp-suite-tutorial-made-easy/)).

---------------
Primero, abre Burp Suite y selecciona la opción de **Intercept**. Cuando estés listo para interceptar, activa Burp y configura el proxy en la máquina o la web que quieras analizar. Para interceptar las peticiones, sigue estos pasos:

1. Configura el navegador o la aplicación para que use el proxy de Burp.
2. En Burp Suite, verifica que el **Intercept** esté habilitado.
3. Realiza la acción en la web o aplicación que deseas interceptar (por ejemplo, cargar una página o enviar un formulario).
4. Burp Suite capturará las peticiones, y podrás revisarlas en detalle para analizarlas o modificarlas según sea necesario.

Recuerda desactivar el proxy en tu navegador o aplicación cuando termines de interceptar las peticiones para volver a la navegación normal.

![[Pasted image 20240918183007.png]]

Así mismo, al presionar **CTRL+R** o hacer clic en **Send to Repeater**, entrarás en modo traza. En este modo, puedes manejar múltiples peticiones y visualizar sus respuestas de manera detallada. El proceso se realiza de la siguiente forma:

1. Una vez que hayas capturado una petición, presiona **CTRL+R** o selecciona **Send to Repeater** en el menú.
2. La petición se enviará al **Repeater**, donde podrás modificar parámetros o reenvíarla tantas veces como necesites.
3. Cada vez que envíes la petición, Burp Suite te mostrará la respuesta de la web o aplicación, permitiéndote analizar cómo reacciona el servidor ante diferentes entradas.
4. Puedes revisar cada interacción y ajustar el comportamiento de la petición según los resultados obtenidos.

Este método es útil para pruebas manuales o para realizar análisis más profundos de las solicitudes y respuestas.

![[Pasted image 20240918183130.png]]

Además, debes definir un **scope** o alcance para evitar que Burp Suite detecte múltiples objetivos y así concentrarte únicamente en el que te interesa. Para hacerlo, sigue estos pasos:

1. Dirígete a la pestaña **Target** y selecciona **Scope**.
2. En el menú de **Scope**, añade el dominio o la URL que deseas interceptar. Puedes especificar direcciones IP, subdominios o rutas particulares para limitar el análisis.
3. Una vez configurado el scope, marca la opción para restringir la actividad de Burp Suite solo a los objetivos dentro del alcance que definiste.
4. Esto evitará que Burp Suite capture peticiones no relacionadas con tu target, facilitando el enfoque en lo que realmente te interesa analizar.

De esta forma, podrás optimizar el proceso de interceptación y análisis, sin interferencias de otros sitios o servicios.

![[Pasted image 20240918183457.png]]

De esta forma, también puedes acceder a la respuesta de la petición sin necesidad de pasar por el **Repeater** al hacer lo siguiente:

1. Una vez que Burp Suite haya interceptado la petición, en lugar de enviarla al **Repeater**, simplemente presiona el botón de **Forward**.
2. Al hacerlo, Burp Suite permitirá que la petición siga su curso hacia el servidor y recibirás la respuesta directamente en el flujo de tráfico habitual.
3. Puedes continuar interceptando más peticiones o deshabilitar la interceptación si no deseas detener otras solicitudes.

Este método es más directo y útil cuando solo necesitas ver cómo reacciona el servidor a la petición sin realizar modificaciones adicionales en el **Repeater**.


![[Pasted image 20240918184236.png]]
En la siguiente sección de la configuración del proxy, puedes añadir sustituciones en todo el código de respuesta o de solicitud de la siguiente manera:

1. Ve a la pestaña de **Proxy** y selecciona **Options**.
2. Desplázate hasta el apartado de **Match and Replace**.
3. Haz clic en **Add** para añadir una nueva regla de sustitución.
4. En el campo de **Match**, escribe el texto o patrón que deseas encontrar en las solicitudes o respuestas.
5. En el campo de **Replace**, introduce el valor con el que deseas sustituir el texto coincidente.
6. Especifica si esta regla debe aplicarse a las peticiones (requests) o respuestas (responses) seleccionando la opción adecuada en el menú desplegable.
7. Finalmente, guarda la configuración.

De esta forma, Burp Suite automáticamente realizará las sustituciones en cada petición o respuesta interceptada según las reglas que hayas definido, facilitando la manipulación de datos de manera más eficiente.

![[Pasted image 20240918184755.png]]

Además, una vez interceptada una petición, si deseas ejecutar ataques de fuerza bruta, puedes enviarla al **Intruder** con **CTRL+I**. Luego, sigue estos pasos para configurar el ataque:

1. **Envía la petición al Intruder** presionando **CTRL+I** o haciendo clic en el botón **Send to Intruder** en Burp Suite.
2. Dirígete a la pestaña **Intruder** y selecciona la solicitud que enviaste.
3. En el **Intruder**, elige la pestaña **Positions**. Aquí verás la petición con todos los parámetros resaltados.
4. Ajusta los parámetros que deseas usar como **payloads** (cargando el diccionario). Puedes seleccionar la palabra o parámetro que quieres que sea reemplazado por los valores del diccionario.
5. Una vez seleccionado el parámetro, haz clic en **Add** para añadirlo como posición a atacar.
6. Luego, ve a la pestaña **Payloads**. Aquí puedes cargar un diccionario con los posibles valores que deseas probar.
7. Haz clic en **Load** y selecciona el archivo de diccionario que deseas usar para el ataque.

Con esto configurado, puedes iniciar el ataque y Burp Suite probará todas las combinaciones posibles del diccionario contra el parámetro seleccionado en la solicitud.

![[Pasted image 20240918185619.png]]

Una vez seleccionada la palabra que deseas usar como fuente de payload, puedes cargar un diccionario de contraseñas, como los de SecLists, siguiendo estos pasos:

1. **En la pestaña de Payloads** del **Intruder**, asegúrate de haber seleccionado la palabra o parámetro que quieres atacar.
2. **Carga el diccionario de contraseñas** haciendo clic en el botón **Load** en la sección **Payloads**.
3. **Selecciona el archivo** del diccionario que quieres usar. Si estás usando un archivo de SecLists, busca el diccionario adecuado en tu sistema de archivos. Algunos ejemplos de diccionarios de SecLists que podrías utilizar son `common-passwords.txt` o `passwords-top1000.txt`.
4. **Carga el diccionario** y verifica que los valores aparezcan en la lista de **Payloads**.

Es una buena práctica utilizar diccionarios más específicos o reducidos en tamaño si el de `rockyou` resulta ser demasiado grande para el objetivo en cuestión, ya que esto puede acelerar el proceso de ataque y hacer que sea más manejable.


![[Pasted image 20240918185833.png]]

1. **Para iniciar el ataque**: Después de cargar el diccionario de contraseñas y configurar los parámetros, simplemente haz clic en **Start Attack**. Este es un ataque de tipo **Sniper**.
    
2. **Payload Encoding**: Asegúrate de **quitar el chulito** en **Payload Encoding** si no deseas que Burp Suite aplique codificación a los payloads.
    
3. **Decoder**: Burp Suite también tiene una pestaña llamada **Decoder**, que puedes usar para decodificar hashes o valores en diferentes formatos.
    
4. **Tipos de ataques**:
    
    - **Cluster Bomb**: Útil para usar dos diccionarios a la vez, por ejemplo, uno para usuarios y otro para contraseñas. Este tipo de ataque combina cada valor del primer diccionario con cada valor del segundo.
    - **Battering Ram**: Itera un mismo diccionario en varias posiciones seleccionadas, útil para pruebas en múltiples parámetros a la vez.
    - **Pitchfork**: Itera en paralelo dos diccionarios, combinando el primer ítem del primer diccionario con el primer ítem del segundo diccionario, el segundo ítem del primer diccionario con el segundo ítem del segundo diccionario, y así sucesivamente.
    -
1. **Uso de proxy**: La mayoría de las herramientas de seguridad, como **sqlmap** para [[SQLI]] y **gobuster**, tienen opciones para configurar un proxy. Esto te permite monitorear su actividad a través de Burp Suite, lo que te ayuda a ver qué están haciendo detrás del código.
    

Esto debería cubrir lo esencial para realizar ataques y analizar la actividad en Burp Suite.