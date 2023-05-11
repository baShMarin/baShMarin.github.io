---
title: MTD - Wireshark 2 
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Medio]
---

¡Hola! Hoy vamos a realizar el segundo ejercicio del módulo de envenenamiento y suplantación de identidad, en el ejercicio trabajaremos con el laboratorio de `wireshark` como ya vimos en el otro ejercicio.
Vamos a trabajar con el laboratorio de `metasploitable2` y `wireshark` para obtener las capturas resultantes.


### Técnicas Vistas: 
- **Localizar la captura resultante**
- **Indicar lo más revelante**

* * *

# Captura resultante del esnifado


En este ejercicio con la captura de pantalla resultante del esnifado de red con `WireShark`. Una vez hayada la captura, apuntaremos la información sensible.
Estamos trabajando ante el laboratorio de `metasploitable2` como ya sabemos usa unas técnicas un poco inseguras en cuanto a cifrado y seguridad.

Para la captura resultante hemos utilizado un parámetro con el filtro del `puerto 21` ; `clienteFTP`.
<img src="/assets/HTB/Wireshark/IPADDR.png">


Podemos observar varios resultados con el parámetro usado del `puerto 21`

* * * 

# Usuarios, credenciales, IPS...
Una vez hayada la captura nos dignaremos a ver un resultado en especifico, donde podemos ver que hemos capturado el paquete de acceso por FTP, filezilla en este caso que lo hemos usado desde nuestra máquina windows 10.

La máquina `metasploitable2` al no usar unos parámetros de seguridad correctos, deja visible esta información con el esnifado de red de `Wireshark`.

<img src="/assets/HTB/Wireshark/IPADDR.png">

* * *

***Esto es un ejercicio con fines didácticos realizado para MasterD.***
