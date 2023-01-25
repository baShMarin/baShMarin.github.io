---
title: MTD - Metasploitable2
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Fácil]
---

<img src="/assets/HTB/Metasploitable/login.png">

¡Hola!
Vamos a resolver de la máquina `Metasploitable2` de dificultad "Fácil".
Metasploitable es una máquina virtual Linux intencionadamente vulnerable. Esta máquina virtual puede utilizarse para impartir formación en seguridad, probar herramientas de seguridad y practicar técnicas habituales de pruebas de penetración. 

Técnicas Vistas: 

- **VSFTP**
- **Telnet**
- **SMB/SAMBA**
- **Ataques de diccionario por fuerza bruta**
- **Metasploit-Framework**

### Preparación Entorno

* * *

Antes de iniciar la fase de enumeración y reconocimiento procederemos a crear un directorio de trabajo con el nombre `Metasploitable`. Una vez creado accedemos al directorio y con la ayuda de la función que tenemos definida en la zshrc `mkt` crearemos cuatro directorios de trabajo `nmap, content, exploits y scripts` donde almacenaremos de una manera ordenada toda la información que vayamos recopilando de la máquina en función de su naturaleza.

```bash
function mkt(){
    mkdir {nmap,content,exploits,scripts}
}
```

### Reconocimiento

* * *

Accedemos al directorio de trabajo `nmap` e iniciamos nuestra fase de reconocimiento realizando un `ping` a la IP de la máquina para comprobar que esté activa y detectamos su sistema operativo basándonos en el `ttl` de una traza **ICMP**.

```bash
❯ ping -c 1 
PING 192.168.1.193 (192.168.1.193) 56(84) bytes of data.
64 bytes from 192.168.1.193: icmp_seq=1 ttl=63 time=42.3 ms

--- 192.168.1.193 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 42.314/42.314/42.314/0.000 ms
```
Identificamos que es una maquina **Linux** debido a su ttl (time to live) correspondiente a 63 (Disminuye en 1 debido a que realiza un salto adicional en el entorno de HackTHeBox).

* TTL => 64 Linux
* TTL => 128 Windows

Continuamos con la enumeración de los **65535** puertos en la máquina.

```bash
nmap -p- --open --min-rate 5000 -vvv -n -Pn 192.168.1.193 -oG allPorts
```
```bash
cat allPorts


```
### Análisis de puertos y vulnerabilidades.

* * *

Examinamos en primer lugar el puerto ```21 ftp```, intentaremos conectarnos vía FTP a la máquina.
Nos pide unas credenciales, por lo que usaremos ```hydra``` con el repositorio de ```Seclist``` o cualquier otro previamente descargado.

```bash
 hydra -

```



### Escalada De Privilegios 

* * *

Hemos completado la máquina **GoodGames** de HackTheBox!! Happy Hacking!!

<img src="/assets/HTB/GoodGames/pwned.png">
