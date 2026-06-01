---
title: MTD - Telefonía móvil 1
published: true
categories: [Android]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Dificil]
---


¡Hola!
Vamos a realizar el primer ejercicio de Telefonía móvil. En este primer ejercicio necesitamos obtener toda la información posible del dispositivo móvil con el que vamos a trabajar. Podremos emplear cualquier recurso a nuestra disposición, siempre que sea legal.

# HERRAMIENTAS UTILIZADAS
* Root Checker Basic
* Android Debug Bridge (ADB)
* VirusTotal
* MyPermission
* Security Advisor

* * *

## PRIMEROS PASOS
Para empezar con el ejercicio hemos realizado una primera revisión del dispositivo con el objetivo de obtener toda la información posible sobre su configuración y detectar posibles vulnerabilidades o servicios que puedan suponer un riesgo de seguridad.
Con una simple aplicación llamada `Root Checker Basic`descubrimos que nuestro dispositivo Android 6.0.1 instalado en un dispositivo VirtualBox ya que lo estamos usando en un entorno de pruebas para el ejercicio.

Una vez comprobado que el dispositivo dispone de acceso root, hemos habilitado las opciones de desarrollador para poder conectarlo a nuestra máquina Kali Linux mediante ADB y comenzar un análisis más profundo desde consola.


```bash
setprop service.adb.tcp.port 3535
```

En esta ocasión vamos a utilizar el sistema `Android Debug Bridge (ADB)`.
`Android Debug Bridge (adb)` es una herramienta de línea de comandos versátil que te permite comunicarte con un dispositivo. El comando `ADB` facilita una variedad de acciones en dispositivos, como instalar y depurar apps. `ADB` proporciona acceso a un shell Unix que puedes usar para ejecutar una variedad de comandos en un dispositivo.

# Información del dispositivo.
## Marca, modelo y dirección IP.
Una vez habilitado el acceso mediante ADB sobre TCP/IP, se estableció una conexión desde mi máquina Kali Linux utilizando el siguiente comando:

```bash
(root㉿kali)-[/home/kali]
└─# adb connect 192.168.0.116:3535
connected to 192.168.0.116:3535
                                                                             
┌──(root㉿kali)-[/home/kali] 
└─# adb devices -l                
List of devices attached
192.168.0.116:3535     device product:android_x86_64 model:VirtualBox device:x86_64 transport_id:1
```
<img src="/assets/HTB/Telefonia/image-1.png" alt="Auditoria Movil 1">

Para obtener información detallada del sistema se consultaron varias propiedades internas mediante el comando `getprop`:

`https://ascii-abhishek.github.io/cs-handbook/bonus/adb_commands/`

```bash
┌──(root㉿kali)-[/home/kali]
└─# adb shell "getprop ro.product.manufacturer; getprop ro.product.model; getprop dhcp.eth0.ipaddress"
innotek GmbH
VirtualBox
192.168.0.116
```
### ro.product.manufacturer 
Indica el fabricante registrado por el sistema Android. En este caso aparece innotek GmbH, empresa asociada al desarrollo y distribución de VirtualBox.

### getprop ro.product.model:
Muestra el modelo del dispositivo detectado por Android. Al tratarse de una máquina virtual, el sistema identifica el entorno como VirtualBox.

### getprop dhcp.eth0.ipaddress:
Permite obtener la dirección IP asignada a la interfaz de red principal. La presencia de la interfaz eth0 confirma que el sistema virtualizado dispone de conectividad mediante una interfaz Ethernet virtual proporcionada por VirtualBox.

## Puertos y servicios abiertos.
Con el objetivo de identificar los servicios expuestos por el dispositivo, se realizaron comprobaciones tanto desde el propio sistema Android mediante ADB como desde la máquina Kali Linux utilizando la herramienta Nmap.

```bash
┌──(kali㉿kali)-[~]
└─$ nmap -sV 192.168.0.116
Starting Nmap 7.98 ( https://nmap.org ) at 2026-06-01 09:47 -0400
Nmap scan report for 192.168.0.116
Host is up (0.0010s latency).
Not shown: 999 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
5555/tcp open  adb     Android Debug Bridge device (name: android_x86_64; model: VirtualBox; device: x86_64)
MAC Address: 08:00:27:2C:58:2F (Oracle VirtualBox virtual NIC)
Service Info: OS: Android; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.22 seconds
```

<img src="/assets/HTB/Telefonia/image-3.png" alt="Auditoria Movil 3">

* Capturas de pantallas de mi reporte de adb shell.
Inicialmente se ejecutó el comando netstat con permisos limitados, observándose que Android restringe la visualización de determinados procesos para usuarios sin privilegios elevados.

Tras obtener acceso root, fue posible consultar la información completa de los servicios en escucha:
```bash
┌──(kali㉿kali)-[~]
└─$ sudo su                     
[sudo] password for kali: 
┌──(root㉿kali)-[/home/kali]
└─# adb connect 192.168.0.116:3535 
* daemon not running; starting now at tcp:5037
* daemon started successfully
connected to 192.168.0.116:3535
                                                                                                                                          
┌──(root㉿kali)-[/home/kali]
└─# adb shell                     
shell@x86_64:/ $ netstat -tulnp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program Name
tcp        0      0 0.0.0.0:3535            0.0.0.0:*               LISTEN      -
tcp        0      0 ::ffff:127.0.0.1:41168  :::*                    LISTEN      -
udp        0      0 :::5228                 :::*                                -
shell@x86_64:/ $ 
130|shell@x86_64:/ $ whoami
shell
shell@x86_64:/ $ su root
root@x86_64:/ # netstat -tulnp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program Name
tcp        0      0 0.0.0.0:3535            0.0.0.0:*               LISTEN      4188/adbd
tcp        0      0 ::ffff:127.0.0.1:41168  :::*                    LISTEN      3720/com.google.andr
udp        0      0 :::5228                 :::*                                2109/com.google.andr
```
<img src="/assets/HTB/Telefonia/image-2.png" alt="Auditoria Movil 2">

## Listado de aplicaciones
```bash
┌──(root㉿kali)-[/home/kali]
└─# adb shell pm list packages    
package:com.google.android.youtube
package:com.example.android.rssreader
package:com.android.providers.telephony
package:org.android_x86.analytics
package:com.google.android.googlequicksearchbox
package:com.android.providers.calendar
package:com.android.providers.media
package:com.google.android.onetimeinitializer
package:com.android.wallpapercropper
package:com.android.documentsui
package:com.android.galaxy4
package:com.mypermissions.mypermissions
package:com.android.externalstorage
package:com.android.htmlviewer
package:com.android.mms.service
package:com.android.providers.downloads
package:com.android.browser
package:com.google.android.configupdater
package:com.android.soundrecorder
package:com.android.defcontainer
package:com.android.providers.downloads.ui
package:com.android.vending
package:com.android.pacprocessor
package:com.joeykrim.rootcheck
package:com.android.certinstaller
package:com.android.carrierconfig
package:com.belarc.securityadvisor
package:android
package:com.android.contacts
package:com.android.camera2
package:com.android.launcher3
package:com.android.backupconfirm
package:com.android.statementservice
package:com.google.android.gm
package:com.overlook.android.fing
package:com.android.wallpaper.holospiral
package:com.android.calendar
package:com.android.phasebeam
package:com.google.android.setupwizard
package:com.android.providers.settings
package:com.android.sharedstoragebackup
package:com.android.printspooler
package:com.android.dreams.basic
package:com.android.webview
package:com.android.inputdevices
package:com.android.server.telecom
package:com.google.android.syncadapters.contacts
package:com.example.android.notepad
package:com.android.keychain
package:com.android.chrome
package:com.android.dialer
package:com.android.gallery3d
package:com.google.android.gms
package:com.google.android.gsf
package:com.android.calllogbackup
package:com.google.android.partnersetup
package:com.android.packageinstaller
package:com.android.basicsmsreceiver
package:com.svox.pico
package:com.android.proxyhandler
package:com.cyanogenmod.filemanager
package:com.android.inputmethod.latin
package:com.google.android.feedback
package:com.google.android.syncadapters.calendar
package:com.android.managedprovisioning
package:com.google.android.gsf.login
package:com.android.wallpaper.livepicker
package:jackpal.androidterm
package:com.google.android.backuptransport
package:com.android.settings
package:com.cyanogenmod.eleven
package:com.android.calculator2
package:org.android_x86.hardwarecollector
package:com.android.wallpaper
package:com.android.vpndialogs
package:com.android.email
package:com.android.phone
package:com.android.shell
package:com.android.providers.userdictionary
package:com.android.location.fused
package:com.android.deskclock
package:com.android.systemui
package:com.android.bluetoothmidiservice
package:com.funnycat.virustotal
package:com.android.bluetooth
package:com.android.development
package:com.android.providers.contacts
package:com.android.captiveportallogin
```

Y estas las aplicaciones instaladas por el usuario.
```bash
┌──(root㉿kali)-[/home/kali]
└─# adb shell pm list packages -3
package:com.mypermissions.mypermissions
package:com.joeykrim.rootcheck
package:com.belarc.securityadvisor
package:com.overlook.android.fing
package:com.android.chrome
package:com.funnycat.virustotal
```
### Uso de VirusTotal
Desde VirusTotal podemos ver todas las aplicaciones instaladas por el usuario desde una interfaz gráfica.


## Permisos potenciales de aplicaciones
Hemos hecho un análisis para ver los permisos "peligrosos" otorgados a aplicaciones y al sistema.

He filtrado para que los muestre en grupos todos los "permisos peligrosos".

```bash
──(root㉿kali)-[/home/kali]
└─# adb shell pm list permissions -g -d
Dangerous Permissions:

group:com.google.android.gms.permission.CAR_INFORMATION
  permission:com.google.android.gms.permission.CAR_VENDOR_EXTENSION
  permission:com.google.android.gms.permission.CAR_MILEAGE
  permission:com.google.android.gms.permission.CAR_FUEL

group:android.permission-group.CONTACTS
  permission:android.permission.WRITE_CONTACTS
  permission:android.permission.GET_ACCOUNTS
  permission:android.permission.READ_CONTACTS

group:android.permission-group.PHONE
  permission:android.permission.READ_CALL_LOG
  permission:android.permission.READ_PHONE_STATE
  permission:android.permission.CALL_PHONE
  permission:android.permission.WRITE_CALL_LOG
  permission:android.permission.USE_SIP
  permission:android.permission.PROCESS_OUTGOING_CALLS
  permission:com.android.voicemail.permission.ADD_VOICEMAIL

group:android.permission-group.CALENDAR
  permission:android.permission.READ_CALENDAR
  permission:android.permission.WRITE_CALENDAR

group:android.permission-group.CAMERA
  permission:android.permission.CAMERA

group:android.permission-group.SENSORS
  permission:android.permission.BODY_SENSORS

group:android.permission-group.SUPERUSER
  permission:android.permission.ACCESS_SUPERUSER

group:android.permission-group.LOCATION
  permission:android.permission.ACCESS_FINE_LOCATION
  permission:com.google.android.gms.permission.CAR_SPEED
  permission:android.permission.ACCESS_COARSE_LOCATION

group:android.permission-group.STORAGE
  permission:android.permission.READ_EXTERNAL_STORAGE
  permission:android.permission.WRITE_EXTERNAL_STORAGE

group:android.permission-group.MICROPHONE
  permission:android.permission.RECORD_AUDIO

group:android.permission-group.SMS
  permission:android.permission.READ_SMS
  permission:android.permission.RECEIVE_WAP_PUSH
  permission:android.permission.RECEIVE_MMS
  permission:android.permission.RECEIVE_SMS
  permission:android.permission.SEND_SMS
  permission:android.permission.READ_CELL_BROADCASTS
```
Todos los comandos usados por adb los he sacado directamente desde la documentación del propio ADB
* * *

*__Esto es un trabajo realizado para MasterD con fines didácticos, no está enseñado para otro de sus usos.__*
