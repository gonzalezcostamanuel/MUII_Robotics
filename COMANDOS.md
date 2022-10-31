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

<details>
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

<details  >
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

<details  >
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

<details  >
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

Ahora instalamos los ciertos paquetes que necesitaremos:
- Curl: para realizar descargas de paquetes que necesitaremos
- gnupg2: Es una herramienta que permite utilizar encriptación digital y servicios de firma usando el estándar 
          OpenPGP. Lo necesitaremos para autorizar la clave GPG de ROS 
- lsb-release: Es uno de los comandos creados bajo el contexto LSB y muestra información sobre la 
                distribución GNU/Linux que se está ejecutando en el ordenador. Se usará para facilitar
                el lanzamiento de ciertos comandos.


```bash
sudo apt install curl gnupg lsb-release
```

<details  >
<summary>Output</summary>
<br>
<pre><code>Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
lsb-release is already the newest version (11.1.0ubuntu4).
lsb-release set to manually installed.
gnupg is already the newest version (2.2.27-3ubuntu2.1).
gnupg set to manually installed.
The following packages were automatically installed and are no longer required:
  libflashrom1 libftdi1-2
Use 'sudo apt autoremove' to remove them.
The following NEW packages will be installed:
  curl
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 194 kB of archives.
After this operation, 453 kB of additional disk space will be used.
Do you want to continue? [Y/n] 
Get:1 http://es.archive.ubuntu.com/ubuntu jammy-updates/main amd64 curl amd64 7.81.0-1ubuntu1.6 [194 kB]
Fetched 194 kB in 1s (267 kB/s)
Selecting previously unselected package curl.
(Reading database ... 196615 files and directories currently installed.)
Preparing to unpack .../curl_7.81.0-1ubuntu1.6_amd64.deb ...
Unpacking curl (7.81.0-1ubuntu1.6) ...
Setting up curl (7.81.0-1ubuntu1.6) ...
Processing triggers for man-db (2.10.2-1) ...
</code></pre>
</details>


Nos descargamos la clave de firma de ROS
```bash
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```
<details  >
<summary>Output</summary>
<br>
<pre><code>
Este comando no produce ningún output
</code></pre>
</details>

Añadimos la clave de firma de ROS a sources.list para que nuestro sistema se fíe de ella

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```
<details  >
<summary>Output</summary>
<br>
<pre><code>
Este comando no produce ningún output
</code></pre>
</details>

Como hemos modificado el fichero de fuentes, debemos realizar un update para que revise si tenemos nuevos
paquetes disponibles en las direcciones que hemos actualizado

```bash
sudo apt update
```
<details  >
<summary>Output</summary>
<br>
<pre><code>Hit:1 http://es.archive.ubuntu.com/ubuntu jammy InRelease
Hit:2 http://security.ubuntu.com/ubuntu jammy-security InRelease                                                                 
Hit:3 http://es.archive.ubuntu.com/ubuntu jammy-updates InRelease                                                                
Hit:4 http://es.archive.ubuntu.com/ubuntu jammy-backports InRelease                                                              
Hit:5 https://ppa.launchpadcontent.net/danielrichter2007/grub-customizer/ubuntu jammy InRelease
Get:6 http://packages.ros.org/ros2/ubuntu jammy InRelease [4,673 B]
Get:7 http://packages.ros.org/ros2/ubuntu jammy/main amd64 Packages [737 kB]
Fetched 742 kB in 2s (399 kB/s)   
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
All packages are up to date.
</code></pre>
</details>

Como ya sabemos, update no descarga nada, sino que simplemente obtiene la información acerca de las
últimas versiones que hay disponibles para tu sistema. Por ello se recomienda ejecutar un upgrade para
estar seguro de que tu sistema tiene las últimas versiones instaladas.

Como podemos ver en el output del comando upgrade, en mi caso concreto no necesitaba descargar ningún
nuevo paquete, aunque sí me recomienda desinstalar paquetes que no son necesarios. Esto lo dejaremos
para otra ocasión y haremos caso omiso de la recomendación.

```bash
sudo apt upgrade
```
<details  >
<summary>Output</summary>
<br>
<pre><code>Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages were automatically installed and are no longer required:
  libflashrom1 libftdi1-2
Use 'sudo apt autoremove' to remove them.
Try Ubuntu Pro beta with a free personal subscription on up to 5 machines.
Learn more at https://ubuntu.com/pro
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
</code></pre>
</details>


Ahora es el momento de instalar ROS2 en nuestro sistema. Para ello podemos instalar la versión 
`ros-rolling-desktop`, que es la versión más completa y recomendada (y la que vamos a utilizar en este
tutorial) o bien la versión `ros-rolling-ros-base` que es la versión más reducida, la cual sería
recomendable utilizar en entornos de producción en los que estés seguro de que no vas a necesitar
más utilidades de las que la están disponibles en la versión base.

```bash
sudo apt install ros-rolling-desktop
```

_Debido a la gran cantidad de líneas que obtenemos en este output no lo podemos mostrar en este fichero. Si quieres 
verlo lo encontrarás en el fichero_ [_output_install_ROS.md_](outputs/output_install_ROS.md)

Ahora, debemos configurar todas las variables de entorno necesarias para poder ejecutar ROS en nuestra sesión
de terminal. Eso se consigue con el comando `source /opt/ros/rolling/setup.bash` o también colocando un 
punto justo antes del path `./opt/ros/rolling/setup.bash
```bash
source /opt/ros/rolling/setup.bash
```
<details  >
<summary>Output</summary>
<br>
<pre><code>
Este comando no produce ningún output
</code></pre>
</details>

Sin embargo, es un poco incómodo tener que acordarse de realizar el source siempre que abres una nueva 
terminal. Por eso, vamos a ejecutar los siguientes pasos para lograr que el comando source sea permanente.
Destacar que para esto nos hemos inspirado en la información proporcionada en este 
[enlace](https://askubuntu.com/questions/951000/how-to-make-the-changes-to-source-profile-permanent)


Para que se nos cargue la configuración siempre que abramos un nuevo terminal debemos modificar el fichero
de inicialización de nuestro bash. Este fichero es `$HOME/.profile`

Pero para que este "truco" funcione, debemos primero verificar que no existe ningún fichero 
`~/.bash_profile` ni `~/.bash_login` ya que estos ficheros serían más prioritarios que `$HOME/.profile` y
nos lo sobreescribirían, por lo que los cambios no surtirían efecto.
```bash
cat ~/.bash_profile
cat ~/.bash_login
```
<details  >
<summary>Output</summary>
<br>
<pre><code>cat: /home/manuel/.bash_profile: No such file or directory
cat: /home/manuel/.bash_login: No such file or directory
</code></pre>
</details>

Ahora lanzamos el siguiente comando, que lo que hará será añadir una nueva línea (que ya hemos explicado) al final
del fichero para que se ejecute cada vez que abramos una nueva consola.

```bash
echo "source /opt/ros/rolling/setup.bash" >> $HOME/.profile
```
<details  >
<summary>Output</summary>
<br>
<pre><code>
Este comando no produce ningún output
</code></pre>
</details>

Ahora debemos cerrar sesión y volver a abrirla (o si quieres curarte en salud, reiniciar) ¡y listo!
Cada vez que abramos una nueva terminal tendremos ROS2 preparado para lanzar lo que necesitemos. 

Vamos a ver un ejemplo

Lanzamos un "Publisher" en una terminal
```bash
ros2 run demo_nodes_cpp talker
```
<details  >
<summary>Output</summary>
<br>
<pre><code>[INFO] [1667239572.788741421] [talker]: Publishing: 'Hello World: 1'
[INFO] [1667239573.788697659] [talker]: Publishing: 'Hello World: 2'
[INFO] [1667239574.788704013] [talker]: Publishing: 'Hello World: 3'
[INFO] [1667239575.788705929] [talker]: Publishing: 'Hello World: 4'
[INFO] [1667239576.788721475] [talker]: Publishing: 'Hello World: 5'
[INFO] [1667239577.788703514] [talker]: Publishing: 'Hello World: 6'
[INFO] [1667239578.788721768] [talker]: Publishing: 'Hello World: 7'
[INFO] [1667239579.788709002] [talker]: Publishing: 'Hello World: 8'
</code></pre>
</details>

Abrimos una segunda consola en la que lanzamos el "Subscriber", que debe recibir los mensajes que son
publicados por el otro proceso (ten en cuenta que los mensajes no son encolados, por lo que si tardas
un cierto tiempo en ejecutar el suscriptor, el primer mensaje que recibirás tendrá un 
identificador como podría ser el 43, el 150... Es decir, que los paquetes se pierden)

```bash
ros2 run demo_nodes_py listener
```
<details  >
<summary>Output</summary>
<br>
<pre><code>[INFO] [1667239727.809670328] [listener]: I heard: [Hello World: 156]
[INFO] [1667239728.790006723] [listener]: I heard: [Hello World: 157]
[INFO] [1667239729.789519871] [listener]: I heard: [Hello World: 158]
[INFO] [1667239730.790196353] [listener]: I heard: [Hello World: 159]
[INFO] [1667239731.790102751] [listener]: I heard: [Hello World: 160]
[INFO] [1667239732.789832065] [listener]: I heard: [Hello World: 161]
[INFO] [1667239733.790101975] [listener]: I heard: [Hello World: 162]
[INFO] [1667239734.790158267] [listener]: I heard: [Hello World: 163]
[INFO] [1667239735.789868728] [listener]: I heard: [Hello World: 164]
[INFO] [1667239736.789934009] [listener]: I heard: [Hello World: 165]
[INFO] [1667239737.790111819] [listener]: I heard: [Hello World: 166]
[INFO] [1667239738.789939615] [listener]: I heard: [Hello World: 167]
[INFO] [1667239739.790137104] [listener]: I heard: [Hello World: 168]
[INFO] [1667239740.789978600] [listener]: I heard: [Hello World: 169]
[INFO] [1667239741.790009617] [listener]: I heard: [Hello World: 170]
[INFO] [1667239742.789811967] [listener]: I heard: [Hello World: 171]
[INFO] [1667239743.789986774] [listener]: I heard: [Hello World: 172]
[INFO] [1667239744.790000289] [listener]: I heard: [Hello World: 173]
[INFO] [1667239745.790013334] [listener]: I heard: [Hello World: 174]
</code></pre>
</details>

---
## RECURSOS

```bash

```
<details  >
<summary>Output</summary>
<br>
<pre><code>
a
</code></pre>
</details>