
--------

El secuestro de la biblioteca de objetos compartidos enlazados dinámicamente (también conocido como _Dynamic Link Library Hijacking_ o _DLL Hijacking_) es una técnica de ataque que aprovecha la manera en que los sistemas operativos cargan las bibliotecas compartidas necesarias para la ejecución de un programa. Aquí tienes una guía general sobre cómo funciona y cómo se puede realizar:

### 1. **Concepto y funcionamiento**

- Las aplicaciones suelen cargar librerías compartidas (como DLL en Windows o archivos `.so` en Linux) para ejecutar funciones específicas.
- Durante la carga, el sistema busca estas librerías en directorios específicos en un orden particular.
- Un atacante puede colocar una biblioteca maliciosa en uno de estos directorios de búsqueda, intentando que el programa cargue esta versión maliciosa en lugar de la legítima.

### 2. **Escenario de ataque**

- El atacante necesita acceso de escritura en un directorio dentro del _path_ de búsqueda de la librería.
- Cuando la aplicación intenta cargar una biblioteca que no puede encontrar, el sistema buscará en directorios alternativos en el orden establecido.
- Si el atacante coloca su versión en una de esas ubicaciones, la aplicación cargará la librería maliciosa, lo que permite ejecutar código no autorizado.

### 3. **Pasos para llevar a cabo el secuestro**

- **Identificar el programa vulnerable:** Encuentra una aplicación que intente cargar una biblioteca sin una ruta absoluta.
- **Monitorear la búsqueda de la biblioteca:** Usa herramientas como `procmon` en Windows para ver cómo el sistema busca librerías faltantes.
- **Crear la librería maliciosa:** Crea una librería compartida con las mismas funciones y nombres que la legítima, pero con código que permita al atacante ejecutar acciones no deseadas.
- **Colocar la librería en el directorio de búsqueda:** Ubica la biblioteca en un directorio que el programa revisará durante la carga, y espera a que el programa cargue tu librería en lugar de la original.

### 4. **Herramientas útiles**

- **ProcMon (Windows)** o **lsof (Linux)**: Para monitorear los intentos de carga de bibliotecas.
- **C/C++ o Python**: Para crear una biblioteca con el código malicioso.
- **Ghidra o IDA Pro**: Para analizar cómo se cargan las bibliotecas en el binario objetivo.

### 5. **Mitigación y protección**

- **Fijación de rutas absolutas:** Los desarrolladores pueden cargar bibliotecas mediante rutas absolutas para evitar que el sistema busque en directorios alternativos.
- **Bloqueo de permisos en directorios sensibles:** Limitar quién puede escribir en los directorios de búsqueda.
- **Uso de listas blancas:** Configurar aplicaciones para que solo carguen bibliotecas autorizadas.


------
El laboratorio comienza con el siguiente script en C, diseñado para generar y ejecutar un número aleatorio:


![[Pasted image 20241030152414.png]]

La siguiente herramienta a utilizar es la siguiente:

![[Pasted image 20241030152527.png]]


De la siguiente manera podemos ver las bibliotecas que maneja un script 

![[Pasted image 20241030152700.png]]

En el manual de C, la función `rand` está caracterizada de la siguiente manera:En el manual de C, la función `rand` está caracterizada de la siguiente manera:

![[Pasted image 20241030152822.png]]

Donde dice el nombre de la función, el retorno y el argumento que recibe 

![[Pasted image 20241030152845.png]]

Podemos crear un programa diferente utilizando la misma biblioteca que se emplea en el script a secuestrar de la siguiente manera:

![[Pasted image 20241030153011.png]]
Ahora bien, el enlazador dinámico funciona de una manera peculiar: ejecuta la función del script en la ubicación más cercana en la que encuentre la biblioteca, de acuerdo con el argumento proporcionado.


![[Pasted image 20241030153120.png]]


De la siguiente manera, lo comprimimos para que funcione correctamente como un buen archivo ejecutable:


![[Pasted image 20241030153204.png]]

Y de la siguiente manera, creo una biblioteca compartida que incluye tanto mi script como el script con la vulnerabilidad de permisos:

![[Pasted image 20241030153245.png]]

Otro vector de ataque sino deja cargar recursos de biblioteca como anteriormente es mirara si el siguiente recurso tiene capacidad de escritura en la maquina 


![[Pasted image 20241030153657.png]]

Y, leyendo hacia dónde apunta la máquina, podemos crear un directorio en el cual colocar el siguiente archivo en C para llevar a cabo una escalada de privilegios:


`#include <stdio.h> #include <stdlib.h>  void main() {     // Código para escalar privilegios     setuid(0);  // Cambia el ID de usuario a root     system("/bin/sh");  // Abre un shell con privilegios elevados }`

### Pasos:

1. **Compila el archivo** para crear una biblioteca compartida o un ejecutable:
  
    `gcc -o exploit -shared -fPIC exploit.c`
    
2. **Ubica el archivo en el directorio** donde el sistema o el enlazador dinámico pueda cargarlo en lugar de la biblioteca legítima.

![[Pasted image 20241030154033.png]]

Una vez realizado esto, se fusiona la biblioteca personalizada con el script en un archivo comprimido. Luego, se cambia la ruta del apuntador hacia la nueva biblioteca personalizada y se nombra la función de manera que coincida con la que el programa espera recibir. 

