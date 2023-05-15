---
title: MTD - EvilURL
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
<img src="/assets/HTB/EvilURL/inicio.png">


* * *
# USO DE LA APLICACIÓN
Para hacer uso de la aplicación necesitaremos tener instalado `python3` que viene previamente instalado en nuestro máquina `Kali Linux`.


```bash
python3 evilurl.py -c facebook.com -a
```
Usaremos el comando -C para crear el dominio falso. EvilURL nos creará automáticamente varios accesos a nuestro link "facebook.com".

Por aquí dejo la captura de pantalla usando antiguamente EvilURL, ya que en la actualidad (2023) está en desuso, hay multitudes de alternativas a `evilURL`.

<img src="/assets/HTB/EvilURL/uso.png">







* * *

*__Esto es un ejercicio con fines didácticos realizado para MasterD.__*



