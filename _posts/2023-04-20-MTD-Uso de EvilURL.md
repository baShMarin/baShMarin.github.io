---
title: MTD - Rogue AP (Android) 
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Medio]
---


¡Hola! 


<img src="/assets/HTB/AndroidAP/INICIO.png">

### Herramientas usadas: 
- **evilURL**


* * *

# PRIMEROS PASOS


```bash 
cd github/
```
```bash
git clone https://github.com/
```

Una vez bajada la herramienta evilURL debemos entrar en ella y darle permisos de ejecución al lanzador.

```bash
cd 
```

```bash
sudo 
```

Una vez listo pasaremos a instalar los requisitos necesarios para la ejecución.
Podemos elegir entre 2 opciones para la instalación, con GUI o desde la misma terminal, en este caso usaremos GUI.

```bash
bash evilTrust.sh -m gui 
```

Para usar el modo terminal tendriamos que sustituir el parámetro `gui` por el parámetro `terminal`.
<img src="/assets/HTB/AndroidAP/.png">




* * * 

#### DEPENDENCIAS
La misma aplicación nos dirá las herramientas necesarias para la instalación una vez desplegada la interfaz de instalación:

* `python-nmap`
* `python-whois`

```bash
sudo apt-get install 
```

```bash
sudo apt-get install 
```

* * *
Una vez instalado todas las dependencias podremos seguir con el lanzamiento de `evilTrust`.
<img src="/assets/HTB/AndroidAP/.png">

Para utilizar nuestra interfaz de red, debemos de utilizarla en `mode monitor`. Y elegiremos la interfaz de red `wlan0mon`.
* * *
# USO DE LA APLICACIÓN


<img src="/assets/HTB/AndroidAP/.png">




* * *

*__Esto es un ejercicio con fines didácticos realizado para MasterD.__*



