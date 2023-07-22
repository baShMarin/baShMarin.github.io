---
title: MTD - Rogue AP (Android) 
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Medio]
---


¡Hola! Hoy vamos a realizar el primer ejercicio del módulo de `Ingenieria Social`, esta vez usaremos un Rogue App pero en dispositivos `Android`. Estaremos usando la herramienta `EvilTrust` que nos facilita crear plantillas directamente desde la terminal y montarla en nuestra interfaz de red para lanzarla de inmediatamente.

*__Esto es un ejercicio con fines didácticos realizado para MasterD.__*

<img src="https://media.discordapp.net/attachments/1103281093643345932/1110245071028236349/INICIO.png">

### Herramientas usadas: 
- **EvilTrust**


* * *

# PRIMEROS PASOS
En primer lugar necesitaremos bajar el repositorio de <https://github.com/s4vitar/evilTrust> dentro de nuestra máquina, para ello usaremos los siguientes comandos.

Yo en particular lo instalo en un repositorio aparte llamado /github para guardar todas las herramientas en una misma carpeta.

```bash 
cd github/
```
```bash
git clone https://github.com/s4vitar/evilTrust
```

Una vez bajada la herramienta eviltrust debemos entrar en ella y darle permisos de ejecución al lanzador.

```bash
cd evilTrust/
```

```bash
sudo chmod +x evilTrust.sh
```

Una vez listo pasaremos a instalar los requisitos necesarios para la ejecución.
Podemos elegir entre 2 opciones para la instalación, con GUI o desde la misma terminal, en este caso usaremos GUI.

```bash
bash evilTrust.sh -m gui 
```

Para usar el modo terminal tendriamos que sustituir el parámetro `gui` por el parámetro `terminal`.
<img src="https://media.discordapp.net/attachments/1103281093643345932/1110245070424264755/GUI.png?width=955&height=564">




* * * 

#### DEPENDENCIAS
La misma aplicación nos dirá las herramientas necesarias para la instalación una vez desplegada la interfaz de instalación:

* `php`
* `dnsmasq`
* `hostapd`

```bash
sudo apt-get install php
```

```bash
sudo apt-get install dnsmasq
```

```bash
sudo apt-get install dnsmasq
```
* * *
Una vez instalado todas las dependencias podremos seguir con el lanzamiento de `evilTrust`.
<img src="https://media.discordapp.net/attachments/1103281093643345932/1110245070680096859/GUI2.png">

Para utilizar nuestra interfaz de red, debemos de utilizarla en `mode monitor`. Y elegiremos la interfaz de red `wlan0mon`.
* * *
# USO DE LA APLICACIÓN
Una vez lanzada nuestra aplicación nos pedirá elegir una plantilla que usaremos para nuestra RogueAP falsa, suplantando así el nombre de la wifi que seleccionemos y colocandonos en medio de las comunicaciones `man in the middle`.

Una vez lanzada la plantilla solamente deberemos esperar en consola a que nos aparezca las credenciales. Esto sucederá cuando el usuario entre en la red wifi e intente entrar en su cuenta ya sea facebook, google, staburcks, instagram... todas las plantillas que nos ofrece `evilTrust`.

<img src="https://media.discordapp.net/attachments/1103281093643345932/1110245071284076686/waiting.png">

Si desde algún dispositivo `Android` iniciamos el `WiFi` veremos la red disponible sin clave de acceso, podremos acceder ya que fuese "la red WiFi de un starbucks", y nos pedirá iniciar sesión.
Todos los datos que introduzcamos serán reportados a nuestra consola, teniendo acceso a toda la información sensible.


* * *
