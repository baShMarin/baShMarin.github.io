---
title: MTD - Telefonía móvil 1
published: true
categories: [Android]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Dificil]
---


¡Hola!
Vamos a realizar el primer ejercicio de Telefonía móvil. En este primer ejercicio necesitamos obtener toda la información posible del dispositivo móvil con el que vamos a trabajar. Podremos emplear cualquier recurso a nuestra disposición, siempre que sea legal.

## HERRAMIENTAS UTILIZADAS
* Root Checker Basic
* ADB
* FING
* VirusTotal
* MyPermission
* Security Advisor

* * *

## PRIMEROS PASOS
Para empezar con el ejercicio hemos comprobado nuestro dispositivo para ver la configuración de este y ver los posibles agujeros o vulnerabilidades que podemos encontrar en el.
Con una simple aplicación llamada `Root Checker Basic`descubrimos que nuestro dispositivo Android 6.0.1 instalado en un dispositivo VirtualBox ya que lo estamos usando en un entorno de pruebas para el ejercicio, está rooteado.
Aprovechando esta aplicación y viendo que nuestro dispositivo está rooteado hemos habilitado las opciones de desarrollador para poder conectar nuestro dispositivo a nuestra máquina virtual Kali Linux y comenzar con el análisis más profundo por consola.

Hemos abierto la `Terminal` dentro de nuestro dispositivo Android y he consultado la IP para poder conectarme directamente desde mi máquina Kali Linux.
Seguidamente hemos entrado como usuario root con el comando `su root` y hemos aceptado como superuser habilitando así el puerto 3535 para mi ADB connect.

```bash
setprop service.adb.tcp.port 3535
```

En esta ocasión vamos a utilizar el sistema `Android Debug Bridge (ADB)`.
`Android Debug Bridge (adb)` es una herramienta de línea de comandos versátil que te permite comunicarte con un dispositivo. El comando `ADB` facilita una variedad de acciones en dispositivos, como instalar y depurar apps. `ADB` proporciona acceso a un shell Unix que puedes usar para ejecutar una variedad de comandos en un dispositivo.

## Dentro de mi KALI LINUX
Una vez habilitado el servicio de ADB por TCP en el puerto 3535 podemos acceder desde nuestra máquina Kali Linux utilizando el comando de 

```bash
(root㉿kali)-[/home/kali]
└─# adb connect 192.168.0.116:3535
connected to 192.168.0.116:3535
                                                                             
┌──(root㉿kali)-[/home/kali] 
└─# adb devices -l                
List of devices attached
192.168.0.116:3535     device product:android_x86_64 model:VirtualBox device:x86_64 transport_id:1
```
<img src="/assets/HTB/Telefonia/image-1.png">

Para imprimir por pantalla nuestra versión, modelo y direccion IP de nuestro dispositivo utilizaremos un script básico aprovechando la documentación de ADB.
`https://ascii-abhishek.github.io/cs-handbook/bonus/adb_commands/`

```bash
┌──(root㉿kali)-[/home/kali]
└─# adb shell "getprop ro.product.manufacturer; getprop ro.product.model; getprop dhcp.eth0.ipaddress"
innotek GmbH
VirtualBox
192.168.0.116
```
-getprop ro.product.manufacturer: Nos devuelve la marca del producto en este caso es innotek GmBh que es la empresa que distribuye VirtualBox, ya que tenemos el android instalado sobre una máquina virtual
-getprop ro.product.model: Nos devuelve el modelo del producto en este caso es VirtualBox ya que está corriendo la máquina android sobre el sistema de VirtualBox.
-getprop dhcp.eth0.ipaddress: Nos devuelve la dirección IP de nuestro dispositivo, marcandole que el dispositivo está enchufado direcctamente a la red por eth0.



## EXPLOTACIÓN


1.
2.
3.
4.

* * *

*__Esto es un trabajo realizado para MasterD con fines didácticos, no está enseñado para otro de sus usos.__*
