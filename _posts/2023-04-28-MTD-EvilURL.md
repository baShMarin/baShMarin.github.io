---
title: MTD - EvilURL
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Medio]
---


¡Hola! 
Hoy vamos a realizar el segundo ejercicio del módulo de Ingeniería social, donde usaremos la heramienta `evilURL` para la generación de un nombre de dominio malicioso.

### Herramientas usadas: 
- **evilURL**


* * *

# PRIMEROS PASOS
Clonaremos el repositorio de github de `UndeadSec`
```bash 
cd github/
```

```bash
git clone https://github.com/UndeadSec/EvilURL.git
```

Una vez listo pasaremos a instalar los requisitos necesarios para la ejecución.

* * * 

#### DEPENDENCIAS
La misma aplicación nos dirá las herramientas necesarias para la instalación una vez desplegada la interfaz de instalación:

* `python-nmap`
* `python-whois`

```bash
pip install python-nmap python-whois
```

* * *
Una vez instalado todas las dependencias podremos seguir con el lanzamiento de `evilURL`.
<img src="https://media.discordapp.net/attachments/1103281093643345932/1110245388952289300/inicio.png?width=553&height=589">


* * *
# USO DE LA APLICACIÓN
Para hacer uso de la aplicación necesitaremos tener instalado `python3` que viene previamente instalado en nuestro máquina `Kali Linux`.


```bash
python3 evilurl.py -c facebook.com -a
```
Usaremos el comando -C para crear el dominio falso. EvilURL nos creará automáticamente varios accesos a nuestro link "facebook.com".

Por aquí dejo la captura de pantalla usando antiguamente EvilURL, ya que en la actualidad (2023) está en desuso, hay multitudes de alternativas a `evilURL`.

<img src="https://media.discordapp.net/attachments/1103281093643345932/1110245389250068651/uso.png?width=936&height=589">







* * *

*__Esto es un ejercicio con fines didácticos realizado para MasterD.__*



