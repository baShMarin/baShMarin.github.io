---
title: MTD - Metasploitable2
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Fácil]
---

<img src="/assets/HTB/Metasploitable/first.jpeg">

¡Hola!
Vamos a resolver de la máquina `Metasploitable2` de dificultad "Fácil".
Metasploitable es una máquina virtual Linux intencionadamente vulnerable. Esta máquina virtual puede utilizarse para impartir formación en seguridad, probar herramientas de seguridad y practicar técnicas habituales de pruebas de penetración. 

Técnicas Vistas: 

- **VSFTP**
- **Telnet**
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
PING 192.168.1.36 (192.168.1.36) 56(84) bytes of data.
64 bytes from 192.168.1.36: icmp_seq=1 ttl=63 time=42.3 ms

--- 192.168.1.36 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 42.314/42.314/42.314/0.000 ms
```
Identificamos que es una maquina **Linux** debido a su ttl (time to live) correspondiente a 63 (Disminuye en 1 debido a que realiza un salto adicional en el entorno de HackTHeBox).

* TTL => 64 Linux
* TTL => 128 Windows

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
### Análisis de puertos y vulnerabilidades.

* * *
### Telnet
Examinamos el puerto ```23 telnet``` siendo común en conexiones vía ```telnet```. Lanzaremos una conexión hacia el puerto 23 vía telnet. Donde nos dará acceso a una ```shell``` y nos dirá las credenciales con usuario y contraseña.
Dentro de la máquina podremos encontrar varios directorios con credenciales de los servicios usados en metasploitable2.


```bash
telnet 192.168.1.36
Trying 192.168.1.36...
Connected to 192.168.1.36.
Escape character is '^]'.
              _                  _       _ _        _     _      ____  
 _ __ ___   ___| |_ __ _ ___ _ __ | | ___ (_) |_ __ _| |__ | | ___|___ \ 
| '_ ` _ \ / _ \ __/ _` / __| '_ \| |/ _ \| | __/ _` | '_ \| |/ _ \ __) |
| | | | | |  __/ || (_| \__ \ |_) | | (_) | | || (_| | |_) | |  __// __/ 
|_| |_| |_|\___|\__\__,_|___/ .__/|_|\___/|_|\__\__,_|_.__/|_|\___|_____|
                            |_|                                          


Warning: Never expose this VM to an untrusted network!

Contact: msfdev[at]metasploit.com

Login with msfadmin/msfadmin to get started


metasploitable login: 

```

* * *

### Análisis web
En el puerto 80 podremos encontrar el servicio http, accederemos en el navegador con la dirección ip http://192.168.1.36:80.
Examinamos la web donde nos da de nuevo las credenciales de usuario y contraseña y varios servicios para poder entrar.

<img src="/assets/HTB/Metasploitable/puerto80.png">


Dentro del directorio #DVWA podremos encontrar un login donde nos dan los credenciales de usuario y contraseña. Al entrar podremos encontrar bastante información sobre toda la estructura .php del sistema.

<img src="/assets/HTB/Metasploitable/phpinfo.png">

Podremos encontrar acceso al servidor tomcat 

* * *

Hemos completado la máquina **Metasploitable** de HackTheBox!! Happy Hacking!!


