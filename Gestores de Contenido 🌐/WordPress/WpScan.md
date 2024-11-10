---

---
-----------

#web #wordpress #reconocimiento #herramientas

----------------

La herramienta **WpScan**[^1] es una herramienta utilizada para enumerar los gestores de contenidos de [[WordPress]]

-------------

Su modo de uso es muy sencillo, podríamos realizar un escaneo a modo de traza con el siguiente código en bash:

```bash
wpscan --url https://tinder.com
```

Si quisiéramos enumerar usuarios validos potenciales existentes podríamos usar el siguiente comando: 

```bash
wpscan --url https://tinder.com --enumerate u
```

Otra de las utilidades de esta app es su capacidad de realizar ataques de fuerza bruta se ejecuta con el siguiente comando:

```bash
wpscan --url https://tinder.com -U xrl3y -P /usr/share/wordlists/rockyou.txt
```

Cabe destacar que este procedimiento también se puede ejecutar de forma manual abusando de la ruta **xmrlpc.php**[^2] , siendo necesario para ello crear un script en bash o en python. 


-----------

## Referencias 

---------

[^1]: Enlace al proyecto de GitHub [Enlace](https://github.com/wpscanteam/wpscan)
[^2]: Enlace al proyecto de GitHub [[xmrlpc]]

