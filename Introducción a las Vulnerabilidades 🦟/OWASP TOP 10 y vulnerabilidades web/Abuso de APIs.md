
-----
Una **API (Interfaz de Programación de Aplicaciones)** es un conjunto de reglas y protocolos que permite que diferentes aplicaciones se comuniquen entre sí. Las APIs definen cómo se deben hacer las solicitudes y cómo deben responder las aplicaciones, facilitando la integración de funcionalidades entre sistemas. Esto es esencial para que las aplicaciones puedan intercambiar datos o usar características de otras sin necesidad de conocer detalles complejos del código interno.

**Ejemplos de APIs:**

1. **API de Google Maps**: Permite integrar mapas, ubicaciones y direcciones en aplicaciones móviles o sitios web.
2. **API de PayPal**: Facilita la integración de pagos en línea, permitiendo que sitios web procesen pagos a través de PayPal.
3. **API de Twitter**: Permite publicar tweets, leer información de perfiles y buscar contenidos directamente desde una aplicación externa.
4. **API de OpenWeather**: Ofrece datos meteorológicos en tiempo real para ser utilizados en apps de clima.

Estas APIs permiten que los desarrolladores construyan aplicaciones más completas al aprovechar funcionalidades ya existentes.

-----
El laboratorio que se establece es el de el github de **Crapi**

al CTRL + SHIFT + C o al mayormente llamado inspeccionar, lo que haremos es ir a network y dirigirnos a la opcion de XHR para que las peticiones no muestren tanto ruido, de la siguiente manera:

![[Pasted image 20241012212115.png]]

si espichamos doble click en cualquiera de las peticiones podremos ver tanto la peticion como la respuyesta como el path de la api a la que se esta gestionando. Muchas cveces como una estructura de puntoas puede ser un **json** La estructura del api token puede ser de la sifuiente manerta

![[Pasted image 20241012212413.png]]

## POSTMAN




![[Pasted image 20241012212924.png]]

posteriormente realizar la ejecuccion de los siguiente comandos:

```bash 
wget https://dl.pstmn.io/download/latest/linux64 -O postman-linux-x64.tar.gz
```

```bash
sudo tar -xzf postman-linux-x64.tar.gz -C /opt
```

```bash
sudo ln -s /opt/Postman/Postman /usr/bin/postman
```

Iniciar sesión en postman con el correo de Gmail 

Una vez estando en postman lo que hacemoes es registrar todos los end point que queremos catalogados con el protocolo que queremos de la sifg uiente forma¿:

![[Pasted image 20241012214648.png]]

Luego procedemos al registro de credenciales en formato raw de tipo json 


![[Pasted image 20241012214823.png]]

Poateriormente jugar con variables por ejemplo en el token dado 

![[Pasted image 20241012215141.png]]

**{{** <- Sirve para mencionar las variables registradad, modificar la autoriuzacion 

![[Pasted image 20241012215303.png]]

con los siguiete, sabiuendo el metodo de empleo de cambio de conteraseña de y de los factores de solo 4 digitos podemos realizar un ataque de fuerza bruta de la siguiente manera
![[Pasted image 20241012220748.png]]

se emplea ffuf porque es menos AGRESIVO para que no se rompa el servidor y ademas se emplea el comando -p para indicarle el tiempo que queremos que se demore entre cada peticcion para no saturar el servidor

![[Pasted image 20241012221415.png]]

en postman puedes cambiar el metodo y puedes fuzzear para descubrir que metodos estan disponibles , se puede probar que 
![[Pasted image 20241012222231.png]]
