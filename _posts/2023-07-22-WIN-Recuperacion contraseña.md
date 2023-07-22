---
title: HTB - File Inclusion
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

<img src="/assets/HTB/Windows11/inicio.png"">

Para iniciar nuestro windows 11 en modo seguro deberemos de presionar la `tecla SHIFT` y seguido sin soltarla presionaremos en el icono de apagado (abajo a la derecha) y reiniciaremos el ordenador. Cuando se esté reiniciando podremos soltar la `tecla SHIFT`. Nos debería aparecer esta pantalla al reiniciarse el ordenador: 

<img src="assets/HTB/Windows11/eligeunaopcion.png">

Seguido de ello tenemos que clickar en `elegir una opción` - `solucionar un problema` - `opciones avanzadas` - `símbolo del sistema`.

<img src="assets/HTB/Windows11/eligeunaopcion.png">
<img src="assets/HTB/Windows11/solucionarunproblema.png">
<img src="assets/HTB/Windows11/opcionesavanzadas.png">

* Si te fijas el cursor está encima de dónde hay que clicar.
Una vez en la terminal de nuestro sistema operativo necesitaremos hacer los siguientes pasos...

* * * 

# DENTRO DE LA TERMINAL
En primer lugar deberemos de hallar en que partición se encuentra nuestro windows instalado. Para ello ejecutaremos los siguientes comandos.

```bash 
X:\Windows\System32> diskpart
```

Una vez dentro de `DISKPART`.

```bash
DISKPART> list volume
```
<img src="assets/HTB/Windows11/listadodevolumenes.png">

* Nos mostrará una lista con los discos y el tamaño de ellos, en mi caso el `Volume 1` el disco `" C "` contiene el sistema operativo Windows.
Una vez hallado el disco debemos de salirnos del `DISKPART` usaremos el comando `exit`

```bash
DISKPART> exit
```

Y pasaremos a escribir los siguientes comandos: 

```bash
X:\Windows\System32> c:
```


```bash
C:\Windows> cd System32
```
Una vez dentro de nuestro disco C en el system32 de nuestro windows debemos ejecutar los siguientes comandos
```bash
C:\Windows\System32> move utilman.exe utilman.exe.bak
```

Seguido de mover `utilman.exe` deberemos de copiar el `CMD` en `accesibilidad` para ello usaremos el comando:
```bash
C:\Windows\System32> copy cmd.exe utilman.exe
```

Nos debe de salir en la consola algo así:
<img src="">

Seguido de ello escribiremos el comando 

```bash
C:\Windows\System32> Wpeutil reboot
```

Ya queda realmente poco... un par de comandos más y listo! Si te has perdido hasta aquí recuerda que puedes consultar conmigo cualquier duda, en el perfil tienes mi contacto.
Sigamos...

Después del reinicio podremos observar que si pulsamos en el botón de `ACCESIBILIDAD` se nos abrirá la `TERMINAL`
<img src="assets/HTB/Windows11/accesibilidad.png">

Una vez abierta nuestra `terminal` deberemos de ejecutar los comandos para crear nuestro usuario y poder entrar a nuestro sistema operativo, para ello debemos de escribir lo siguiente:

```bash
C:\Windows\System32> net user "Nombre de usuario que quieras" /add
```
* En el nombre de usuario podrás colocar el que quieras, recuerda que no tienes que poner las comillas en tu nombre de usuario, quedaría algo así: `net user Marin /add`

Ahora necesitaremos saber el nombre de nuestro grupo de administradores, para ello usaremos el comando

```bash
C:\Windows\System32> net localgroup
```
Nos hará una lista de los grupos de usuarios de nuestro ordenador, debemos de fijarnos en el que sea "Admin", pueden aparecer de varias formas depende del idioma de tu sistema operativo, en mi caso es "Administrators" pero en tu caso si es el windows-ES será "Administradores", una vez localizado el nombre usaremos el comando siguiente:

```bash
C:\Windows\System32> net localgroup "Tu grupo Administrador" "Nombre de usuario elegido" /add
```
* Recuerda colocar el nombre del grupo hallado en el paso anterior donde pone "Tu grupo Administrador" y tu el nombre de usuario que elegistes en el paso anterior, IMPORTANTE no usar las comillas y dejar los espacios, el comando quedaría algo así:

`net localgroup Administradores Marin /add`

Ya puedes cerrar la pestaña de la terminal y reiniciar el ordenador, al reiniciar el ordenador podrás ver que tienes un nuevo usuario con el nombre que le dimos y podrás entrar a tu sistema operativo.

# DENTRO DE NUESTRO SISTEMA OPERATIVO
Una vez dentro de nuestro sistema operativo podremos ver que ya tenemos acceso a todo y que nuestro usuario contiene los poderes de administrador.

En este caso queremos recuperar nuestra antigua cuenta para recuperar la información que teníamos en el usuario, para ello haremos los siguientes pasos:

1. Clic derecho en el icono de `Windows`
2. Clicaremos en `Administración de equipos.``
3. Clicaremos seguidamente en `Usuarios y grupos locales`
4. Nos aparecera el nombre de `Usuarios` dónde haremos clic derecho y `establecer contraseña`

Una vez asignada la contraseña podremos reiniciar o cerrar sesión de nuestro windows y entrar en nuestro usuario perdido.



* * *

*__Esto es un trabajo con fines didácticos, no está enseñado para otro de sus usos.__*
