
--------


Las condiciones de carrera (Race Condition) son un fenómeno en la programación concurrente que ocurre cuando dos o más procesos o hilos intentan acceder y manipular recursos compartidos al mismo tiempo, sin la debida sincronización. Este tipo de problema puede llevar a resultados inesperados o incorrectos, ya que el orden en que se ejecutan los procesos puede influir en el estado final del recurso compartido.

Las condiciones de carrera son especialmente comunes en sistemas operativos y aplicaciones que utilizan múltiples hilos de ejecución, donde las operaciones críticas no están protegidas adecuadamente. Identificar y mitigar estas condiciones es crucial para garantizar la integridad de los datos y la estabilidad de las aplicaciones. Las técnicas para prevenir condiciones de carrera incluyen el uso de mecanismos de sincronización, como mutexes, semáforos y monitores, que aseguran que solo un hilo pueda acceder al recurso compartido en un momento dado.

El estudio de las condiciones de carrera no solo es fundamental para los desarrolladores de software, sino también para los ingenieros de sistemas y arquitectos de software, quienes deben diseñar sistemas robustos y confiables en entornos concurrentes.

------

Debemos mirar la url para ver como se comporta apartir de las variables

![[Pasted image 20241025205401.png]]

Posteriormente en los paneles de texto para ingresare un usuario si vemos que por parte del codigo crea un archivo bash de las siguiente forma:

![[Pasted image 20241025211203.png]]

Entonces podremos en el apartado de ingreso probar ejecución remota de comandos de la siguiente forma: 


![[Pasted image 20241025211302.png]]

Pero como no permite esta ejecucion remota de comandos entonces se puede probar una condicion de carrera que basicamente establece que antes de que borre un arvhivo que no es aceptable, leerlo

![[Pasted image 20241025212422.png]]

