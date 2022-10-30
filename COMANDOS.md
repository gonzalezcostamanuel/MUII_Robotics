# Listado de todos los comandos lanzados en este proyecto para UBUNTU 22.04


## Instalamos ROS2 siguiendo el tutorial oficial

El tutorial lo podemos encontrar en https://docs.ros.org/en/rolling/Installation/Ubuntu-Install-Debians.html

---
En primer lugar, nos aseguramos de que el idioma del sistema es el compatible con ROS2 mediante el comando locale:
Una localización (locale) es un conjunto de reglas culturales e idiomáticas que abarcan
     aspectos tales como el idioma usado para mensajes, diferentes juegos de caracteres,
     convenios lexicográficos, etc.
```bash
locale
```

<details open>
<summary>Output</summary>
<br>
<pre><code>LANG=en_US.UTF-8
LANGUAGE=
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC=es_ES.UTF-8
LC_TIME=es_ES.UTF-8
LC_COLLATE="en_US.UTF-8"
LC_MONETARY=es_ES.UTF-8
LC_MESSAGES="en_US.UTF-8"
LC_PAPER=es_ES.UTF-8
LC_NAME=es_ES.UTF-8
LC_ADDRESS=es_ES.UTF-8
LC_TELEPHONE=es_ES.UTF-8
LC_MEASUREMENT=es_ES.UTF-8
LC_IDENTIFICATION=es_ES.UTF-8
LC_ALL=
</code></pre>
</details>

En nuestro caso ya tenemos UTF-8 configurado. Aun así, ejecutamos los comandos para que veáis el output esperado: 
primero ejecutamos un update para descargarnos todos los paquetes disponibles y luego 
nos instalamos el paquete "locales" que nos permita configurar los tomas de localización

```bash
sudo apt update && sudo apt install locales
```
<details open>
<summary>Output</summary>
<br>
<pre><code>[sudo] password for manuel: 
Hit:1 http://es.archive.ubuntu.com/ubuntu jammy InRelease
Get:2 http://es.archive.ubuntu.com/ubuntu jammy-updates InRelease [114 kB]                          
Get:3 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]                           
Hit:4 https://ppa.launchpadcontent.net/danielrichter2007/grub-customizer/ubuntu jammy InRelease
Get:5 http://es.archive.ubuntu.com/ubuntu jammy-backports InRelease [99,8 kB]            
Get:6 http://es.archive.ubuntu.com/ubuntu jammy-updates/main amd64 DEP-11 Metadata [94,8 kB]
Get:7 http://es.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 DEP-11 Metadata [255 kB]
Get:8 http://es.archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 DEP-11 Metadata [940 B]
Get:9 http://es.archive.ubuntu.com/ubuntu jammy-backports/universe amd64 DEP-11 Metadata [12,6 kB]
Get:10 http://security.ubuntu.com/ubuntu jammy-security/main amd64 DEP-11 Metadata [20,1 kB]
Get:11 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 DEP-11 Metadata [13,3 kB]
Fetched 721 kB in 1s (733 kB/s)              
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
locales is already the newest version (2.35-0ubuntu3.1).
locales set to manually installed.
The following packages were automatically installed and are no longer required:
  libflashrom1 libftdi1-2
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
</code></pre>
</details>

En nuestro caso se puede comprobar en el output que ya teníamos instalado correctamente el paquete "locales".

Ahora lanzamos todos los comandos de configuración de "locales":
```bash
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
```

<details open>
<summary>Output</summary>
<br>
<pre><code>manuel@manuel-VivoBook-ASUSLaptop-X421EAYB-K413EA:~$ sudo locale-gen en_US en_US.UTF-8
Generating locales (this might take a while)...
  en_US.ISO-8859-1... done
  en_US.UTF-8... done
Generation complete.
manuel@manuel-VivoBook-ASUSLaptop-X421EAYB-K413EA:~$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
manuel@manuel-VivoBook-ASUSLaptop-X421EAYB-K413EA:~$ export LANG=en_US.UTF-8
</code></pre>
</details>

Ahora si volvemos a ejecutar el comando "locale" tenemos que ver las terminaciones UTF-8 (si es que no lo teníamos ya)



También necesitamos configurar el repositorio de ROS2 para que se pueda instalar. Y antes de ello, debemos 
verificar que tenemos activo el repositorio "universe" de Ubuntu:

```bash
apt-cache policy | grep universe
```
<details open>
<summary>Output</summary>
<br>
<pre><code> 500 http://security.ubuntu.com/ubuntu jammy-security/universe i386 Packages
     release v=22.04,o=Ubuntu,a=jammy-security,n=jammy,l=Ubuntu,c=universe,b=i386
 500 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages
     release v=22.04,o=Ubuntu,a=jammy-security,n=jammy,l=Ubuntu,c=universe,b=amd64
 100 http://es.archive.ubuntu.com/ubuntu jammy-backports/universe i386 Packages
     release v=22.04,o=Ubuntu,a=jammy-backports,n=jammy,l=Ubuntu,c=universe,b=i386
 100 http://es.archive.ubuntu.com/ubuntu jammy-backports/universe amd64 Packages
     release v=22.04,o=Ubuntu,a=jammy-backports,n=jammy,l=Ubuntu,c=universe,b=amd64
 500 http://es.archive.ubuntu.com/ubuntu jammy-updates/universe i386 Packages
     release v=22.04,o=Ubuntu,a=jammy-updates,n=jammy,l=Ubuntu,c=universe,b=i386
 500 http://es.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages
     release v=22.04,o=Ubuntu,a=jammy-updates,n=jammy,l=Ubuntu,c=universe,b=amd64
 500 http://es.archive.ubuntu.com/ubuntu jammy/universe i386 Packages
     release v=22.04,o=Ubuntu,a=jammy,n=jammy,l=Ubuntu,c=universe,b=i386
 500 http://es.archive.ubuntu.com/ubuntu jammy/universe amd64 Packages
     release v=22.04,o=Ubuntu,a=jammy,n=jammy,l=Ubuntu,c=universe,b=amd64
</code></pre>
</details>

Efectivamente, vemos que al ejecutar el comando anteriormente citado obtenemos una salida distinta de vacío, 
por lo que tenemos correctamente habilitado el repositorio Universe.

Si no vemos el anterior output prueba a lanzar los siguientes comandos:

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

---

RECURSOS

```bash

```
<details open>
<summary>Output</summary>
<br>
<pre><code>
a
</code></pre>
</details>
