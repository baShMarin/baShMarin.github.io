---
title: MTD - Wireshark 1 
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Medio]
---

¡Hola! Hoy vamos a realizar el primer ejercicio del módulo de envenenamiento y suplantación de identidad, en el ejercicio trabajaremos con el laboratorio de `wireshark`.
`Wireshark` es una herramienta dedicado para el esnifado del tráfico de red y además podremos usar diferentes filtros de red para tener busquedas acertadas.


### Técnicas Vistas: 
- **ESNIFADO DE RED CON WIRESHARK**
- **USO DE DIFERENTES FILTROS**


* * *

# Esnifado de red con `Wireshark`
En primer deberemos colocar las conexiones en nuestra máquina virtual para colocarnos en medio de las comunicaciones.

```bash
echo 1 > proc/sys/net/ipv4/ip_forward 
```
Una vez situados en medio de las comunicaciones podremos iniciar la herramienta `wireshark`
Y utilizaremos la interfaz de la red `et0` para comenzar nuestro esnifado de red.

<img src="/assets/HTB/Wireshark/wireshark.png">

* * * 

# Uso de diferentes filtros

En segundo lugar podremos utilizar los diferentes filtros que nos ofrece wireshark, en la barra de navegación de arriba podremos utilizar diferentes filtros tales como: 

```bash
tcp.srcport == 80
```
Donde buscariamos por el puerto 80 buscando por un servicio `http`.
Podemos usar otro puerto diferente tal y cómo 
<img src="/assets/HTB/Wireshark/srcport80.png">

```bash
tcp.dstport == 21
```
Usando el puerto `21` buscando un servicio `ftp` cuyos paquetes sean destinatarios.
Al igual que podremos buscar directamente por la `ip destinataria` si es que la tenemos 

```bash
ip.addr == 34.588.38.1
```
Y obtener directamente resultados de paquetes esnifados hacia esa dirección ip.

<img src="/assets/HTB/Wireshark/IPADDR.png">

* * *



