---
title: MTD - bWAPP file inclusion
published: false
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Medio]
---


¡Hola! 
Hoy vamos a realizar el primer ejercicio del módulo de Hacking de aplicaciones Web, trabajando sobre la máquina `beebox`.
En este ejercicio realizaremos un `File inclusion`.

### Herramientas usadas: 
- **OWAS-ZAP Proxy**
- **Fuzz**
- **Scripting**


* * *

# PRIMEROS PASOS
En primer lugar usaremos `OWAS-ZAP` como proxy para indexar las páginas web y poder realizar un análisis más detallado.
La herramienta viene preinstalada en nuestro `kali linux` por lo tanto solamente tendriamos que abrirlo desde el gestor de aplicaciones y acto seguido configurar el `proxy` en nuestro navegador.

<img src="assets/HTB/bWAPP/proxy.png">

Acto seguido pasamos entrar a la web de nuestro máquina `bee-box`, en nuestro caso tendríamos la IP 10.0.2.5.


Como podemos ver en este `login` tenemos las credenciales ya dadas, en este caso `bee` de usuario y `bug` como contraseña.
Dentro de la aplicación web podemos encontrar en el apartado `A7` el ejercicio de `Remote & Local File Inclusion (RFI/LFI)`.


* * * 

# EN LA APLICACIÓN WEB
Una vez dentro de la aplicación web podemos ver un selector de lenguajes, a simple vista parece algo simple.

<img src="assets/HTB/bWAPP/proxy.png">

Cuando seleccionamos un lenguaje, podemos ver que la URL de nuestra aplicación web cambia. 
Esto sería un vacío donde podriamos realizar nuestras técnicas de intrusión.

Observamos que dentro del link añade una particularidad, `?language=lang_en.php` donde podemos comenzar a intentar escalar privilegios usando `../../etc/passw` para subir entre carpetas dentro del servidor web y podriamos ver si nos devuelve el fichero sensible.

<img src="assets/HTB/bWAPP/etc_pass.png">

Si analizamos el código fuente de la aplicación web, podemos ver que nos ha devuelto el fichero completo con sus directorios y nombres de usuario

<img src="assets/HTB/bWAPP/etc_pass.png">

Esta información podemos usarla para aplicarle ténicas de `fuzzing` con `OWAS-ZAP`. Eso lo veremos en el siguiente paso.

* * *

# USO DE LA APLICACIÓN
La aplicación que vamos a usar es `OWAS-ZAP` con técnicas de `fuzzing`. Usaremos diccionarios como `SecList` y `RockForYou`

```bash

```

<img src="assets/HTB/">







* * *

*__Esto es un ejercicio con fines didácticos realizado para MasterD.__*



