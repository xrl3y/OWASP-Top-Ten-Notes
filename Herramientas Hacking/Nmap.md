
------

### Comandos Básicos de Nmap

1. **Escaneo Básico de Puertos**
    
    
    `nmap <dirección IP o dominio>`
    
    Realiza un escaneo básico de puertos en un host, usando TCP connect o SYN.
    
2. **Escaneo de Rango de IP**
    
    
    `nmap <rango de IP>`
    
    Escanea un rango de direcciones IP, útil para ver qué hosts están activos en una red.
    
3. **Escaneo de Puertos Específicos**
    
    
    `nmap -p <puerto1,puerto2,...> <IP>`
    
    Escanea puertos específicos en el host especificado.
    
4. **Escaneo de Todos los Puertos**
    
    
    `nmap -p- <IP>`
    
    Escanea todos los puertos posibles (1-65535).
    
5. **Escaneo de Puertos TCP y UDP**
    
    
    `nmap -sS -sU <IP>`
    
    Realiza un escaneo TCP SYN y UDP en el host especificado.
    
6. **Escaneo de Red Completo**
    
    
    `nmap -A <IP>`
    
    Ejecuta un escaneo exhaustivo que incluye información del sistema operativo, servicios, versión de software, y scripts NSE.
    

### Modos de Escaneo Comunes

1. **Escaneo SYN (`-sS`)**  
    Este es uno de los escaneos más rápidos y sigilosos, también conocido como "escaneo stealth". No completa la conexión TCP, lo que puede evitar detección en algunos firewalls.
    
2. **Escaneo de Puertos UDP (`-sU`)**  
    Escanea los puertos UDP en lugar de TCP. Puede ser más lento y consume más recursos, pero es esencial para encontrar servicios como DNS o DHCP.
    
3. **Escaneo Completo de TCP (`-sT`)**  
    Este escaneo utiliza una conexión TCP completa, útil si no se tienen permisos de superusuario para hacer un escaneo SYN.
    
4. **Escaneo de Detección de Sistema Operativo (`-O`)**  
    Identifica el sistema operativo del host escaneado. Esto es útil para conocer posibles vulnerabilidades o servicios asociados con el sistema operativo.
    
5. **Escaneo de Versiones (`-sV`)**  
    Detecta la versión de los servicios en los puertos abiertos, lo que ayuda a evaluar posibles vulnerabilidades basadas en versiones específicas.
    
6. **Escaneo Rápido (`-F`)**  
    Escanea solo los puertos más comunes en lugar de todos. Esto es útil para un análisis preliminar rápido.
    

### Ejemplos de Comandos Útiles

- **Escaneo TCP SYN Rápido en una Red**
    
    
    `nmap -sS -F <rango de IP>`
    
- **Detección de Sistema Operativo y Servicios**
    
    `nmap -O -sV <IP>`
    
- **Escaneo Profundo con Scripts NSE**
    
    
    `nmap -A <IP>`
    

### Notas Adicionales

- **Permisos**: Algunos escaneos (como SYN) requieren permisos de superusuario.
- **Scripts NSE**: Nmap tiene un conjunto de scripts avanzados para análisis de vulnerabilidades, detección de malware, y mucho más (`nmap --script <script_name>`).

-------

Escaneo básico para CTF

```
namp -p- --open -sS --min-rate 5000 -Pn -n -vvv <IP> -oG AllPorts
```

```
nmap -p<Puertos Abiertos> -sCV <IP> -oN Targeted
```

![[Pasted image 20241108173830.png]]

