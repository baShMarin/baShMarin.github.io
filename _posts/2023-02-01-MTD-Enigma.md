---
title: MTD - Enigma 
published: true
categories: [Windows]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Fácil]
---

<img src="/assets/HTB/Enigma/enigma.png">

¡Hola!
Vamos a hacer un análisis de las vulnerabilidades más críticas en  `Enigma` de dificultad "Fácil".
`Enigma` es una máquina virtual Windows Server 2012 intencionadamente vulnerable. Esta máquina virtual puede utilizarse para impartir formación en seguridad, probar herramientas de seguridad y practicar técnicas habituales de pruebas de penetración. 

### Técnicas Vistas: 

- **FTP**
- **NETBIOS**
- **SMBCLIENT**
- **METERPRETER**
- **ATAQUES Dos**

# Preparación Entorno

* * *

Antes de iniciar la fase de enumeración y reconocimiento procederemos a crear un directorio de trabajo con el nombre ``. Una vez creado accedemos al directorio y con la ayuda de la función que tenemos definida en la zshrc `mkt` crearemos cuatro directorios de trabajo `nmap, content, exploits y scripts` donde almacenaremos de una manera ordenada toda la información que vayamos recopilando de la máquina en función de su naturaleza.

```bash
function mkt(){
    mkdir {nmap,content,exploits,scripts}
}
```

# Reconocimiento

* * *

Accedemos al directorio de trabajo `nmap` e iniciamos nuestra fase de reconocimiento realizando un `ping` a la IP de la máquina para comprobar que esté activa y detectamos su sistema operativo basándonos en el `ttl` de una traza **ICMP**.

```bash
❯ ping -c 1 
PING 192.168.1.36 (192.168.1.36) 56(84) bytes of data.
64 bytes from 192.168.1.36: icmp_seq=1 ttl=63 time=42.3 ms

--- 192.168.1.36 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 42.314/42.314/42.314/0.000 ms
```
Identificamos que es una maquina **Windows** debido a su ttl (time to live) correspondiente a 127 (Disminuye en 1 debido a que realiza un salto adicional en el entorno de HackTHeBox).

* `TTL => 64 Linux`
* `TTL => 128 Windows`

Continuamos con la enumeración de los **65535** puertos en la máquina.

```bash
nmap -p- --open --min-rate 5000 -vvv -n -Pn 192.168.1.36 -oG allPorts
```

```bash
cat allPorts

    Discovered open port 139/tcp on 192.168.1.36
    Discovered open port 111/tcp on 192.168.1.36
    Discovered open port 22/tcp on 192.168.1.36
    Discovered open port 5900/tcp on 192.168.1.36
    Discovered open port 3306/tcp on 192.168.1.36
    Discovered open port 80/tcp on 192.168.1.36
    Discovered open port 21/tcp on 192.168.1.36
    Discovered open port 23/tcp on 192.168.1.36
    Discovered open port 445/tcp on 192.168.1.36
    Discovered open port 25/tcp on 192.168.1.36
    Discovered open port 53/tcp on 192.168.1.36
    Discovered open port 513/tcp on 192.168.1.36
    Discovered open port 1524/tcp on 192.168.1.36
    Discovered open port 512/tcp on 192.168.1.36
    Discovered open port 1099/tcp on 192.168.1.36
    Discovered open port 2049/tcp on 192.168.1.36
    Discovered open port 2121/tcp on 192.168.1.36
    Discovered open port 8009/tcp on 192.168.1.36
    Discovered open port 5432/tcp on 192.168.1.36
    Discovered open port 8180/tcp on 192.168.1.36
    Discovered open port 6667/tcp on 192.168.1.36
    Discovered open port 6000/tcp on 192.168.1.36
    Discovered open port 514/tcp on 192.168.1.36
    Completed Connect Scan at 21:21, 4.62s elapsed (1000 total ports)
```

Con nuestra herramienta `extractPorts` previamente definida en nuestra `.zshrc` podremos extraer los puertos más relevantes y hacerle un escaneo a dichos puertos, para ver el servicio usado en ellos.

```bash 


```



* * *
# Exploits 

#### FTP 
#### NETBIOS
#### SMBCLIENT + REVERSE_TCP
#### ATAQUE DOS

* * *



* * *

# Escalada de privilegios

Y con estas vulnerabilidades encontradas vemos lo fácil que ha sido conseguir el control remoto completo de la máquina `metasploitable2`

Un saludo!
* * *




