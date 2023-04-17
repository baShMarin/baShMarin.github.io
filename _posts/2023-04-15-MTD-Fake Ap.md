---
title: MTD - Fake Ap 
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Medio]
---


¡Hola! Hoy vamos a montar un fake AP para poder obtener la contraseña WPA de la red Wireless,
puede usarse un método manual con airbase-ng, o algún script automatizado
como airgeddon.


### Técnicas Vistas: 

- **RogueApp con Airbase**
- **EvilTwin**

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
Una vez preparada nuestra interfaz USB podremos comenzar con la creación de la FakeAp para obtener las credenciales WPA, para ello crearemos una falsa red con el nombre de `Prueba de Concepto` para poder localizarla y trabajar con ella.

```bash

```

```bash

```

* * *


# -


* * * 

# -


* * *
