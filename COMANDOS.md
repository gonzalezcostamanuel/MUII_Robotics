# Listado de todos los comandos lanzados en este proyecto para UBUNTU 22.04

## Instalamos ROS2 siguiendo el tutorial oficial

El tutorial lo podemos encontrar en https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html

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
sudo apt install ros-humble-desktop
```

 <span style="font-size: xx-small; ">
Debido a la gran cantidad de líneas que obtenemos en este output no lo podemos mostrar en este fichero. Si quieres 
verlo lo encontrarás en el fichero [_output_install_ROS.md_](outputs/output_install_ROS.md)
</span>  

Ahora, debemos configurar todas las variables de entorno necesarias para poder ejecutar ROS en nuestra sesión
de terminal. Eso se consigue con el comando `source /opt/ros/rolling/setup.bash` o también colocando un 
punto justo antes del path `./opt/ros/rolling/setup.bash
```bash
source /opt/ros/humble/setup.bash
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
echo "source /opt/ros/humble/setup.bash" >> $HOME/.profile
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




## Instalamos NAV2 Tools

Seguimos las indicaciones mostradas en https://navigation.ros.org/build_instructions/index.html

Se instalan 3 paquetes diferentes en una misma línea:
* **ros-humble-navigation2**: Nos surte de las herramientas que usará el robot para moverse de un punto "A" a 
un punto "B" de la mejor manera posible. 
* **ros-humble-turtlebot3**: Paquete que contiene todo lo necesario para modelar el robot turtlebot3 en nuestro ordenador.
    Podéis ver una imagen del robot real a continuación: 
<p align="center">
    <img src="https://www.roscomponents.com/1326-home_default_2x/turtlebot3-burger.jpg" alt="turtlebot3" width="200"/>
</p>
    
* **ros-humble-nav2-bringup**: Paquete que contiene todo un sistema de software completo para aplicaciones basadas en
    Nav2. Los autores recomiendan utilizar este paquete como base para que tú realices tu sistema simplemente modificando
    lo que sea necesario de este paquete bringup. Otro dato interesante es que, por defecto, nav2-bringup agrupa todas
    sus funcionalidades en un único nodo de ROS para mejorar su eficiencia, pero tenemos la opción de lanzarlo con
    las funcionalidades divididas en nodos con la opción `use_composition:=False`. Más información [aquí](https://github.com/ros-planning/navigation2/blob/humble/nav2_bringup/README.md).





```bash
sudo apt install ros-humble-navigation2 ros-humble-nav2-bringup ros-humble-turtlebot3*
```

 <span style="font-size: xx-small; ">
Debido a la gran cantidad de líneas que obtenemos en este output no lo podemos mostrar en este fichero. Si quieres 
verlo lo encontrarás en el fichero [_output_install_NAV2.md_](outputs/output_install_NAV2.md)
  </span>  
Ahora, si queremos preparar el entorno para una prueba rápida que nos muestre que hemos instalado correctamente 
NAV2 seteamos las siguientes variables de entorno:
* **TURTLEBOT3_MODEL**: Indicamos qué tipo de modelo queremos utilizar, es decir, el mapa. Algunas opciones son 
WAFFLE, BURGER, etc.
* **GAZEBO_MODEL_PATH**: Aquí le indicamos la ruta donde están almacenados los modelos. Si queremos asegurarnos
de que esa es la ruta correcta, lanza un `ls -la` y te tendrá que salir algo similar a lo mostrado en este desplegable.
<details  >
<summary>Contenido de la carpeta /opt/ros/humble/share/turtlebot3_gazebo/models</summary>
<br>
<pre><code>total 36
drwxr-xr-x 9 root root 4096 Nov  2 19:16 .
drwxr-xr-x 9 root root 4096 Nov  2 19:16 ..
drwxr-xr-x 2 root root 4096 Nov  2 19:16 turtlebot3_burger
drwxr-xr-x 3 root root 4096 Nov  2 19:16 turtlebot3_common
drwxr-xr-x 8 root root 4096 Nov  2 19:16 turtlebot3_dqn_world
drwxr-xr-x 2 root root 4096 Nov  2 19:16 turtlebot3_house
drwxr-xr-x 2 root root 4096 Nov  2 19:16 turtlebot3_waffle
drwxr-xr-x 2 root root 4096 Nov  2 19:16 turtlebot3_waffle_pi
drwxr-xr-x 3 root root 4096 Nov  2 19:16 turtlebot3_world

</code></pre>
</details>

Los comandos que debemos lanzar para configurar las variables de entorno que acabamos de explicar son:

```bash
export TURTLEBOT3_MODEL=waffle
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/humble/share/turtlebot3_gazebo/models
```
<details  >
<summary>Output</summary>
<br>
<pre><code>
Este comando no produce ningún output
</code></pre>
</details>

Y una vez configuradas las variables de entorno lanzamos el siguiente comando, el cual te debe arrojar
un output similar al mostrado en la correspondiente sección de output a la vez que se te abre el simulador
Gazebo y una ventana de RVIZ (puedes ver un ejemplo [aquí](https://navigation.ros.org/getting_started/index.html))

```bash
ros2 launch nav2_bringup tb3_simulation_launch.py headless:=False
```
<details  >
<summary>Output</summary>
<br>
<pre><code>[INFO] [launch]: All log files can be found below /home/manuel/.ros/log/2022-11-04-16-43-39-236745-manuel-VivoBook-ASUSLaptop-X421EAYB-K413EA-7713
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [gzserver-1]: process started with pid [7727]
[INFO] [gzclient-2]: process started with pid [7729]
[INFO] [spawn_entity.py-3]: process started with pid [7731]
[INFO] [robot_state_publisher-4]: process started with pid [7733]
[INFO] [rviz2-5]: process started with pid [7735]
[INFO] [component_container_isolated-6]: process started with pid [7737]
[rviz2-5] Warning: Ignoring XDG_SESSION_TYPE=wayland on Gnome. Use QT_QPA_PLATFORM=wayland to run on Wayland anyway.
[robot_state_publisher-4] Link base_link had 7 children
[robot_state_publisher-4] Link camera_link had 2 children
[robot_state_publisher-4] Link camera_depth_frame had 1 children
[robot_state_publisher-4] Link camera_depth_optical_frame had 0 children
[robot_state_publisher-4] Link camera_rgb_frame had 1 children
[robot_state_publisher-4] Link camera_rgb_optical_frame had 0 children
[robot_state_publisher-4] Link caster_back_left_link had 0 children
[robot_state_publisher-4] Link caster_back_right_link had 0 children
[robot_state_publisher-4] Link imu_link had 0 children
[robot_state_publisher-4] Link base_scan had 0 children
[robot_state_publisher-4] Link wheel_left_link had 0 children
[robot_state_publisher-4] Link wheel_right_link had 0 children
[robot_state_publisher-4] [INFO] [1667576620.029065054] [robot_state_publisher]: got segment base_footprint
[robot_state_publisher-4] [INFO] [1667576620.029162903] [robot_state_publisher]: got segment base_link
[robot_state_publisher-4] [INFO] [1667576620.029171560] [robot_state_publisher]: got segment base_scan
[robot_state_publisher-4] [INFO] [1667576620.029177459] [robot_state_publisher]: got segment camera_depth_frame
[robot_state_publisher-4] [INFO] [1667576620.029183334] [robot_state_publisher]: got segment camera_depth_optical_frame
[robot_state_publisher-4] [INFO] [1667576620.029189102] [robot_state_publisher]: got segment camera_link
[robot_state_publisher-4] [INFO] [1667576620.029194265] [robot_state_publisher]: got segment camera_rgb_frame
[robot_state_publisher-4] [INFO] [1667576620.029199636] [robot_state_publisher]: got segment camera_rgb_optical_frame
[robot_state_publisher-4] [INFO] [1667576620.029205170] [robot_state_publisher]: got segment caster_back_left_link
[robot_state_publisher-4] [INFO] [1667576620.029210543] [robot_state_publisher]: got segment caster_back_right_link
[robot_state_publisher-4] [INFO] [1667576620.029215797] [robot_state_publisher]: got segment imu_link
[robot_state_publisher-4] [INFO] [1667576620.029221397] [robot_state_publisher]: got segment wheel_left_link
[robot_state_publisher-4] [INFO] [1667576620.029227218] [robot_state_publisher]: got segment wheel_right_link
[component_container_isolated-6] [INFO] [1667576620.169177476] [nav2_container]: Load Library: /opt/ros/humble/lib/libmap_server_core.so
[component_container_isolated-6] [INFO] [1667576620.179329612] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_map_server::MapSaver>
[component_container_isolated-6] [INFO] [1667576620.179392564] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_map_server::MapServer>
[component_container_isolated-6] [INFO] [1667576620.179400137] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_map_server::MapServer>
[component_container_isolated-6] [INFO] [1667576620.187972165] [map_server]: 
[component_container_isolated-6] 	map_server lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.188020696] [map_server]: Creating
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/map_server' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.191130201] [nav2_container]: Load Library: /opt/ros/humble/lib/libamcl_core.so
[component_container_isolated-6] [INFO] [1667576620.196780682] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_amcl::AmclNode>
[component_container_isolated-6] [INFO] [1667576620.196822090] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_amcl::AmclNode>
[component_container_isolated-6] [INFO] [1667576620.205684271] [amcl]: 
[component_container_isolated-6] 	amcl lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.206418899] [amcl]: Creating
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/amcl' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.209400146] [nav2_container]: Load Library: /opt/ros/humble/lib/libnav2_lifecycle_manager_core.so
[component_container_isolated-6] [INFO] [1667576620.209991578] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_lifecycle_manager::LifecycleManager>
[component_container_isolated-6] [INFO] [1667576620.210015093] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_lifecycle_manager::LifecycleManager>
[component_container_isolated-6] [INFO] [1667576620.215319921] [lifecycle_manager_localization]: Creating
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/lifecycle_manager_localization' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.216905534] [lifecycle_manager_localization]: Creating and initializing lifecycle service clients
[component_container_isolated-6] [INFO] [1667576620.218801484] [lifecycle_manager_localization]: Starting managed nodes bringup...
[component_container_isolated-6] [INFO] [1667576620.218834344] [lifecycle_manager_localization]: Configuring map_server
[component_container_isolated-6] [INFO] [1667576620.218946097] [map_server]: Configuring
[component_container_isolated-6] [INFO] [map_io]: Loading yaml file: /opt/ros/humble/share/nav2_bringup/maps/turtlebot3_world.yaml
[component_container_isolated-6] [DEBUG] [map_io]: resolution: 0.05
[component_container_isolated-6] [DEBUG] [map_io]: origin[0]: -10
[component_container_isolated-6] [DEBUG] [map_io]: origin[1]: -10
[component_container_isolated-6] [DEBUG] [map_io]: origin[2]: 0
[component_container_isolated-6] [DEBUG] [map_io]: free_thresh: 0.196
[component_container_isolated-6] [DEBUG] [map_io]: occupied_thresh: 0.65
[component_container_isolated-6] [DEBUG] [map_io]: mode: trinary
[component_container_isolated-6] [DEBUG] [map_io]: negate: 0
[rviz2-5] [INFO] [1667576620.219496531] [rviz2]: Stereo is NOT SUPPORTED
[rviz2-5] [INFO] [1667576620.219571784] [rviz2]: OpenGl version: 4.6 (GLSL 4.6)
[component_container_isolated-6] [INFO] [map_io]: Loading image_file: /opt/ros/humble/share/nav2_bringup/maps/turtlebot3_world.pgm
[component_container_isolated-6] [DEBUG] [map_io]: Read map /opt/ros/humble/share/nav2_bringup/maps/turtlebot3_world.pgm: 384 X 384 map @ 0.05 m/cell
[rviz2-5] [INFO] [1667576620.235912711] [rviz2]: Stereo is NOT SUPPORTED
[component_container_isolated-6] [INFO] [1667576620.236787100] [lifecycle_manager_localization]: Configuring amcl
[component_container_isolated-6] [INFO] [1667576620.236888981] [amcl]: Configuring
[component_container_isolated-6] [INFO] [1667576620.237039158] [amcl]: initTransforms
[component_container_isolated-6] [INFO] [1667576620.242446798] [amcl]: initPubSub
[component_container_isolated-6] [INFO] [1667576620.244126073] [amcl]: Subscribed to map topic.
[component_container_isolated-6] [INFO] [1667576620.245464380] [lifecycle_manager_localization]: Activating map_server
[component_container_isolated-6] [INFO] [1667576620.245526403] [map_server]: Activating
[component_container_isolated-6] [INFO] [1667576620.245682887] [map_server]: Creating bond (map_server) to lifecycle manager.
[component_container_isolated-6] [INFO] [1667576620.246010634] [amcl]: Received a 384 X 384 map @ 0.050 m/pix
[component_container_isolated-6] [INFO] [1667576620.253041982] [nav2_container]: Load Library: /opt/ros/humble/lib/libcontroller_server_core.so
[component_container_isolated-6] [INFO] [1667576620.255065101] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_controller::ControllerServer>
[component_container_isolated-6] [INFO] [1667576620.255093060] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_controller::ControllerServer>
[component_container_isolated-6] [INFO] [1667576620.259767824] [controller_server]: 
[component_container_isolated-6] 	controller_server lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.261535365] [controller_server]: Creating controller server
[component_container_isolated-6] [INFO] [1667576620.268928158] [local_costmap.local_costmap]: 
[component_container_isolated-6] 	local_costmap lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.269305130] [local_costmap.local_costmap]: Creating Costmap
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/controller_server' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.272278229] [nav2_container]: Load Library: /opt/ros/humble/lib/libsmoother_server_core.so
[component_container_isolated-6] [INFO] [1667576620.273737069] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_smoother::SmootherServer>
[component_container_isolated-6] [INFO] [1667576620.273788018] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_smoother::SmootherServer>
[component_container_isolated-6] [INFO] [1667576620.279582708] [smoother_server]: 
[component_container_isolated-6] 	smoother_server lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.280309115] [smoother_server]: Creating smoother server
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/smoother_server' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.282591109] [nav2_container]: Load Library: /opt/ros/humble/lib/libplanner_server_core.so
[component_container_isolated-6] [INFO] [1667576620.283518394] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_planner::PlannerServer>
[component_container_isolated-6] [INFO] [1667576620.283545907] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_planner::PlannerServer>
[component_container_isolated-6] [INFO] [1667576620.291717470] [planner_server]: 
[component_container_isolated-6] 	planner_server lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.292665423] [planner_server]: Creating
[component_container_isolated-6] [INFO] [1667576620.304009505] [global_costmap.global_costmap]: 
[component_container_isolated-6] 	global_costmap lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.304277667] [global_costmap.global_costmap]: Creating Costmap
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/planner_server' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.307200565] [nav2_container]: Load Library: /opt/ros/humble/lib/libbehavior_server_core.so
[component_container_isolated-6] [INFO] [1667576620.309332765] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<behavior_server::BehaviorServer>
[component_container_isolated-6] [INFO] [1667576620.309366274] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<behavior_server::BehaviorServer>
[component_container_isolated-6] [INFO] [1667576620.316472923] [behavior_server]: 
[component_container_isolated-6] 	behavior_server lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/behavior_server' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.319535716] [nav2_container]: Load Library: /opt/ros/humble/lib/libbt_navigator_core.so
[component_container_isolated-6] [INFO] [1667576620.320972646] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_bt_navigator::BtNavigator>
[component_container_isolated-6] [INFO] [1667576620.320999193] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_bt_navigator::BtNavigator>
[component_container_isolated-6] [WARN] [1667576620.322196701] [amcl]: New subscription discovered on topic '/particle_cloud', requesting incompatible QoS. No messages will be sent to it. Last incompatible policy: RELIABILITY_QOS_POLICY
[component_container_isolated-6] [INFO] [1667576620.328462673] [bt_navigator]: 
[component_container_isolated-6] 	bt_navigator lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.328529777] [bt_navigator]: Creating
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/bt_navigator' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.330742456] [nav2_container]: Load Library: /opt/ros/humble/lib/libwaypoint_follower_core.so
[component_container_isolated-6] [INFO] [1667576620.331269371] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_waypoint_follower::WaypointFollower>
[component_container_isolated-6] [INFO] [1667576620.331295322] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_waypoint_follower::WaypointFollower>
[component_container_isolated-6] [INFO] [1667576620.342368830] [waypoint_follower]: 
[component_container_isolated-6] 	waypoint_follower lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.342619672] [waypoint_follower]: Creating
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/waypoint_follower' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.344824568] [nav2_container]: Load Library: /opt/ros/humble/lib/libvelocity_smoother_core.so
[component_container_isolated-6] [INFO] [1667576620.345736890] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_velocity_smoother::VelocitySmoother>
[component_container_isolated-6] [INFO] [1667576620.345805577] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_velocity_smoother::VelocitySmoother>
[component_container_isolated-6] [INFO] [1667576620.348363152] [lifecycle_manager_localization]: Server map_server connected with bond.
[component_container_isolated-6] [INFO] [1667576620.348421543] [lifecycle_manager_localization]: Activating amcl
[component_container_isolated-6] [INFO] [1667576620.348531725] [amcl]: Activating
[component_container_isolated-6] [INFO] [1667576620.348568160] [amcl]: Creating bond (amcl) to lifecycle manager.
[component_container_isolated-6] [INFO] [1667576620.357282465] [velocity_smoother]: 
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/velocity_smoother' in container 'nav2_container'
[component_container_isolated-6] 	velocity_smoother lifecycle node launched. 
[component_container_isolated-6] 	Waiting on external lifecycle transitions to activate
[component_container_isolated-6] 	See https://design.ros2.org/articles/node_lifecycle.html for more information.
[component_container_isolated-6] [INFO] [1667576620.359649355] [nav2_container]: Found class: rclcpp_components::NodeFactoryTemplate<nav2_lifecycle_manager::LifecycleManager>
[component_container_isolated-6] [INFO] [1667576620.359694749] [nav2_container]: Instantiate class: rclcpp_components::NodeFactoryTemplate<nav2_lifecycle_manager::LifecycleManager>
[component_container_isolated-6] [INFO] [1667576620.370467413] [lifecycle_manager_navigation]: Creating
[spawn_entity.py-3] [INFO] [1667576620.372905953] [spawn_entity]: Spawn Entity started
[spawn_entity.py-3] [INFO] [1667576620.373218905] [spawn_entity]: Loading entity XML from file /opt/ros/humble/share/nav2_bringup/worlds/waffle.model
[spawn_entity.py-3] [INFO] [1667576620.374009528] [spawn_entity]: Waiting for service /spawn_entity, timeout = 30
[spawn_entity.py-3] [INFO] [1667576620.374263826] [spawn_entity]: Waiting for service /spawn_entity
[INFO] [launch_ros.actions.load_composable_nodes]: Loaded node '/lifecycle_manager_navigation' in container 'nav2_container'
[component_container_isolated-6] [INFO] [1667576620.380869518] [lifecycle_manager_navigation]: Creating and initializing lifecycle service clients
[component_container_isolated-6] [INFO] [1667576620.389023211] [lifecycle_manager_navigation]: Starting managed nodes bringup...
[component_container_isolated-6] [INFO] [1667576620.389065828] [lifecycle_manager_navigation]: Configuring controller_server
[component_container_isolated-6] [INFO] [1667576620.389168117] [controller_server]: Configuring controller interface
[component_container_isolated-6] [INFO] [1667576620.389439116] [controller_server]: getting goal checker plugins..
[component_container_isolated-6] [INFO] [1667576620.389580003] [controller_server]: Controller frequency set to 20.0000Hz
[component_container_isolated-6] [INFO] [1667576620.389622515] [local_costmap.local_costmap]: Configuring
[component_container_isolated-6] [INFO] [1667576620.393554184] [local_costmap.local_costmap]: Using plugin "voxel_layer"
[component_container_isolated-6] [INFO] [1667576620.396318106] [local_costmap.local_costmap]: Subscribed to Topics: scan
[component_container_isolated-6] [INFO] [1667576620.401824982] [local_costmap.local_costmap]: Initialized plugin "voxel_layer"
[component_container_isolated-6] [INFO] [1667576620.401867079] [local_costmap.local_costmap]: Using plugin "inflation_layer"
[component_container_isolated-6] [INFO] [1667576620.402867710] [local_costmap.local_costmap]: Initialized plugin "inflation_layer"
[component_container_isolated-6] [INFO] [1667576620.410311420] [controller_server]: Created progress_checker : progress_checker of type nav2_controller::SimpleProgressChecker
[component_container_isolated-6] [INFO] [1667576620.414531942] [controller_server]: Created goal checker : general_goal_checker of type nav2_controller::SimpleGoalChecker
[component_container_isolated-6] [INFO] [1667576620.415977130] [controller_server]: Controller Server has general_goal_checker  goal checkers available.
[component_container_isolated-6] [INFO] [1667576620.417437324] [controller_server]: Created controller : FollowPath of type dwb_core::DWBLocalPlanner
[component_container_isolated-6] [INFO] [1667576620.419838332] [controller_server]: Setting transform_tolerance to 0.200000
[rviz2-5] [INFO] [1667576620.429380032] [rviz2]: Trying to create a map of size 384 x 384 using 1 swatches
[rviz2-5] [ERROR] [1667576620.432989169] [rviz2]: Vertex Program:rviz/glsl120/indexed_8bit_image.vert Fragment Program:rviz/glsl120/indexed_8bit_image.frag GLSL link result : 
[rviz2-5] active samplers with a different type refer to the same texture image unit
[component_container_isolated-6] [INFO] [1667576620.433123317] [controller_server]: Using critic "RotateToGoal" (dwb_critics::RotateToGoalCritic)
[component_container_isolated-6] [INFO] [1667576620.433939558] [controller_server]: Critic plugin initialized
[component_container_isolated-6] [INFO] [1667576620.434134421] [controller_server]: Using critic "Oscillation" (dwb_critics::OscillationCritic)
[component_container_isolated-6] [INFO] [1667576620.435433244] [controller_server]: Critic plugin initialized
[component_container_isolated-6] [INFO] [1667576620.435624057] [controller_server]: Using critic "BaseObstacle" (dwb_critics::BaseObstacleCritic)
[component_container_isolated-6] [INFO] [1667576620.435924918] [controller_server]: Critic plugin initialized
[component_container_isolated-6] [INFO] [1667576620.436117284] [controller_server]: Using critic "GoalAlign" (dwb_critics::GoalAlignCritic)
[component_container_isolated-6] [INFO] [1667576620.436809708] [controller_server]: Critic plugin initialized
[component_container_isolated-6] [INFO] [1667576620.437018848] [controller_server]: Using critic "PathAlign" (dwb_critics::PathAlignCritic)
[component_container_isolated-6] [INFO] [1667576620.437651259] [controller_server]: Critic plugin initialized
[component_container_isolated-6] [INFO] [1667576620.438020772] [controller_server]: Using critic "PathDist" (dwb_critics::PathDistCritic)
[component_container_isolated-6] [INFO] [1667576620.438503370] [controller_server]: Critic plugin initialized
[component_container_isolated-6] [INFO] [1667576620.438762643] [controller_server]: Using critic "GoalDist" (dwb_critics::GoalDistCritic)
[component_container_isolated-6] [INFO] [1667576620.439157712] [controller_server]: Critic plugin initialized
[component_container_isolated-6] [INFO] [1667576620.439186998] [controller_server]: Controller Server has FollowPath  controllers available.
[component_container_isolated-6] [INFO] [1667576620.446261026] [lifecycle_manager_navigation]: Configuring smoother_server
[component_container_isolated-6] [INFO] [1667576620.446394698] [smoother_server]: Configuring smoother server
[component_container_isolated-6] [INFO] [1667576620.450861702] [lifecycle_manager_localization]: Server amcl connected with bond.
[component_container_isolated-6] [INFO] [1667576620.450954174] [lifecycle_manager_localization]: Managed nodes are active
[component_container_isolated-6] [INFO] [1667576620.450982557] [lifecycle_manager_localization]: Creating bond timer...
[component_container_isolated-6] [INFO] [1667576620.453317840] [smoother_server]: Created smoother : simple_smoother of type nav2_smoother::SimpleSmoother
[component_container_isolated-6] [INFO] [1667576620.454543743] [smoother_server]: Smoother Server has simple_smoother  smoothers available.
[component_container_isolated-6] [INFO] [1667576620.459718509] [lifecycle_manager_navigation]: Configuring planner_server
[component_container_isolated-6] [INFO] [1667576620.459826101] [planner_server]: Configuring
[component_container_isolated-6] [INFO] [1667576620.459869216] [global_costmap.global_costmap]: Configuring
[component_container_isolated-6] [INFO] [1667576620.463613619] [global_costmap.global_costmap]: Using plugin "static_layer"
[component_container_isolated-6] [INFO] [1667576620.464409724] [global_costmap.global_costmap]: Subscribing to the map topic (/map) with transient local durability
[component_container_isolated-6] [INFO] [1667576620.464903862] [global_costmap.global_costmap]: Initialized plugin "static_layer"
[component_container_isolated-6] [INFO] [1667576620.464924836] [global_costmap.global_costmap]: Using plugin "obstacle_layer"
[component_container_isolated-6] [INFO] [1667576620.465885508] [global_costmap.global_costmap]: Subscribed to Topics: scan
[component_container_isolated-6] [INFO] [1667576620.469812705] [global_costmap.global_costmap]: Initialized plugin "obstacle_layer"
[component_container_isolated-6] [INFO] [1667576620.469853154] [global_costmap.global_costmap]: Using plugin "inflation_layer"
[component_container_isolated-6] [INFO] [1667576620.470758432] [global_costmap.global_costmap]: Initialized plugin "inflation_layer"
[component_container_isolated-6] [INFO] [1667576620.475114399] [planner_server]: Created global planner plugin GridBased of type nav2_navfn_planner/NavfnPlanner
[component_container_isolated-6] [INFO] [1667576620.475145993] [planner_server]: Configuring plugin GridBased of type NavfnPlanner
[component_container_isolated-6] [INFO] [1667576620.475800446] [planner_server]: Planner Server has GridBased  planners available.
[component_container_isolated-6] [INFO] [1667576620.482637611] [lifecycle_manager_navigation]: Configuring behavior_server
[component_container_isolated-6] [INFO] [1667576620.482843222] [behavior_server]: Configuring
[component_container_isolated-6] [INFO] [1667576620.486784673] [behavior_server]: Creating behavior plugin spin of type nav2_behaviors/Spin
[component_container_isolated-6] [INFO] [1667576620.487688507] [behavior_server]: Configuring spin
[component_container_isolated-6] [INFO] [1667576620.493437259] [behavior_server]: Creating behavior plugin backup of type nav2_behaviors/BackUp
[component_container_isolated-6] [INFO] [1667576620.494160285] [behavior_server]: Configuring backup
[component_container_isolated-6] [INFO] [1667576620.500114334] [behavior_server]: Creating behavior plugin drive_on_heading of type nav2_behaviors/DriveOnHeading
[component_container_isolated-6] [INFO] [1667576620.500800793] [behavior_server]: Configuring drive_on_heading
[component_container_isolated-6] [INFO] [1667576620.504395427] [behavior_server]: Creating behavior plugin wait of type nav2_behaviors/Wait
[component_container_isolated-6] [INFO] [1667576620.504881147] [behavior_server]: Configuring wait
[component_container_isolated-6] [INFO] [1667576620.507687966] [lifecycle_manager_navigation]: Configuring bt_navigator
[component_container_isolated-6] [INFO] [1667576620.507769096] [bt_navigator]: Configuring
[component_container_isolated-6] [INFO] [1667576620.535033046] [global_costmap.global_costmap]: StaticLayer: Resizing costmap to 384 X 384 at 0.050000 m/pix
[component_container_isolated-6] [INFO] [1667576620.559299673] [lifecycle_manager_navigation]: Configuring waypoint_follower
[component_container_isolated-6] [INFO] [1667576620.559423490] [waypoint_follower]: Configuring
[component_container_isolated-6] [INFO] [1667576620.569030550] [waypoint_follower]: Created waypoint_task_executor : wait_at_waypoint of type nav2_waypoint_follower::WaitAtWaypoint
[component_container_isolated-6] [INFO] [1667576620.570078245] [lifecycle_manager_navigation]: Configuring velocity_smoother
[component_container_isolated-6] [INFO] [1667576620.570235337] [velocity_smoother]: Configuring velocity smoother
[component_container_isolated-6] [INFO] [1667576620.573773907] [lifecycle_manager_navigation]: Activating controller_server
[component_container_isolated-6] [INFO] [1667576620.573869236] [controller_server]: Activating
[component_container_isolated-6] [INFO] [1667576620.573898513] [local_costmap.local_costmap]: Activating
[component_container_isolated-6] [INFO] [1667576620.573910386] [local_costmap.local_costmap]: Checking transform
[component_container_isolated-6] [INFO] [1667576620.573928177] [local_costmap.local_costmap]: Timed out waiting for transform from base_link to odom to become available, tf error: Invalid frame ID "odom" passed to canTransform argument target_frame - frame does not exist
[component_container_isolated-6] [INFO] [1667576621.074025720] [local_costmap.local_costmap]: Timed out waiting for transform from base_link to odom to become available, tf error: Invalid frame ID "odom" passed to canTransform argument target_frame - frame does not exist
[spawn_entity.py-3] [INFO] [1667576621.128662944] [spawn_entity]: Calling service /spawn_entity
[spawn_entity.py-3] [INFO] [1667576621.505913651] [spawn_entity]: Spawn status: SpawnEntity: Successfully spawned entity [turtlebot3_waffle]
[gzserver-1] ../src/intel/isl/isl.c:2220: FINISHME: ../src/intel/isl/isl.c:isl_surf_supports_ccs: CCS for 3D textures is disabled, but a workaround is available.
[component_container_isolated-6] [INFO] [1667576621.574063063] [local_costmap.local_costmap]: Timed out waiting for transform from base_link to odom to become available, tf error: Invalid frame ID "odom" passed to canTransform argument target_frame - frame does not exist
[INFO] [spawn_entity.py-3]: process has finished cleanly [pid 7731]
[gzserver-1] [INFO] [1667576621.738333293] [intel_realsense_r200_depth_driver]: Publishing camera info to [/intel_realsense_r200_depth/camera_info]
[gzserver-1] [INFO] [1667576621.739096666] [intel_realsense_r200_depth_driver]: Publishing depth camera info to [/intel_realsense_r200_depth/depth/camera_info]
[gzserver-1] [INFO] [1667576621.739605250] [intel_realsense_r200_depth_driver]: Publishing pointcloud to [/intel_realsense_r200_depth/points]
[gzclient-2] ../src/intel/isl/isl.c:2220: FINISHME: ../src/intel/isl/isl.c:isl_surf_supports_ccs: CCS for 3D textures is disabled, but a workaround is available.
[gzserver-1] [INFO] [1667576621.822437822] [turtlebot3_diff_drive]: Wheel pair 1 separation set to [0.287000m]
[gzserver-1] [INFO] [1667576621.822496572] [turtlebot3_diff_drive]: Wheel pair 1 diameter set to [0.066000m]
[gzserver-1] [INFO] [1667576621.824032985] [turtlebot3_diff_drive]: Subscribed to [/cmd_vel]
[gzserver-1] [INFO] [1667576621.826149732] [turtlebot3_diff_drive]: Advertise odometry on [/odom]
[gzserver-1] [INFO] [1667576621.829119622] [turtlebot3_diff_drive]: Publishing odom transforms between [odom] and [base_footprint]
[gzserver-1] [INFO] [1667576621.841072781] [turtlebot3_joint_state]: Going to publish joint [wheel_left_joint]
[gzserver-1] [INFO] [1667576621.841124056] [turtlebot3_joint_state]: Going to publish joint [wheel_right_joint]
[component_container_isolated-6] [INFO] [1667576622.044695421] [amcl]: createLaserObject
[component_container_isolated-6] [INFO] [1667576622.074067927] [local_costmap.local_costmap]: start
[component_container_isolated-6] [INFO] [1667576622.075126154] [controller_server]: Creating bond (controller_server) to lifecycle manager.
[component_container_isolated-6] [INFO] [1667576622.178151220] [lifecycle_manager_navigation]: Server controller_server connected with bond.
[component_container_isolated-6] [INFO] [1667576622.178210221] [lifecycle_manager_navigation]: Activating smoother_server
[component_container_isolated-6] [INFO] [1667576622.178539738] [smoother_server]: Activating
[component_container_isolated-6] [INFO] [1667576622.178585946] [smoother_server]: Creating bond (smoother_server) to lifecycle manager.
[component_container_isolated-6] [INFO] [1667576622.282470794] [lifecycle_manager_navigation]: Server smoother_server connected with bond.
[component_container_isolated-6] [INFO] [1667576622.282520046] [lifecycle_manager_navigation]: Activating planner_server
[component_container_isolated-6] [INFO] [1667576622.282753411] [planner_server]: Activating
[component_container_isolated-6] [INFO] [1667576622.282791526] [global_costmap.global_costmap]: Activating
[component_container_isolated-6] [INFO] [1667576622.282799631] [global_costmap.global_costmap]: Checking transform
[component_container_isolated-6] [INFO] [1667576622.282808296] [global_costmap.global_costmap]: Timed out waiting for transform from base_link to map to become available, tf error: Invalid frame ID "map" passed to canTransform argument target_frame - frame does not exist
[component_container_isolated-6] [INFO] [1667576622.782900232] [global_costmap.global_costmap]: Timed out waiting for transform from base_link to map to become available, tf error: Invalid frame ID "map" passed to canTransform argument target_frame - frame does not exist
[component_container_isolated-6] [INFO] [1667576622.845412244] [amcl]: Message Filter dropping message: frame 'base_scan' at time 0.426 for reason 'the timestamp on the message is earlier than all the data in the transform cache'
[component_container_isolated-6] [INFO] [1667576623.282866049] [global_costmap.global_costmap]: Timed out waiting for transform from base_link to map to become available, tf error: Invalid frame ID "map" passed to canTransform argument target_frame - frame does not exist
[component_container_isolated-6] [WARN] [1667576623.475882921] [amcl]: AMCL cannot publish a pose or update the transform. Please set the initial pose...
[component_container_isolated-6] [INFO] [1667576623.748303215] [amcl]: Message Filter dropping message: frame 'base_scan' at time 1.426 for reason 'the timestamp on the message is earlier than all the data in the transform cache'
[component_container_isolated-6] [INFO] [1667576623.782869683] [global_costmap.global_costmap]: Timed out waiting for transform from base_link to map to become available, tf error: Invalid frame ID "map" passed to canTransform argument target_frame - frame does not exist
[rviz2-5] [INFO] [1667576623.856196938] [rviz]: Message Filter dropping message: frame 'base_scan' at time 0.426 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576624.048136289] [rviz]: Message Filter dropping message: frame 'base_scan' at time 0.627 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576624.079991290] [rviz]: Message Filter dropping message: frame 'odom' at time 0.630 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576624.272684793] [rviz]: Message Filter dropping message: frame 'base_scan' at time 0.826 for reason 'discarding message because the queue is full'
[component_container_isolated-6] [INFO] [1667576624.282877420] [global_costmap.global_costmap]: Timed out waiting for transform from base_link to map to become available, tf error: Invalid frame ID "map" passed to canTransform argument target_frame - frame does not exist
[rviz2-5] [INFO] [1667576624.304707388] [rviz]: Message Filter dropping message: frame 'odom' at time 0.834 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576624.464248178] [rviz]: Message Filter dropping message: frame 'base_scan' at time 1.026 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576624.496500631] [rviz]: Message Filter dropping message: frame 'odom' at time 1.038 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576624.656359076] [rviz]: Message Filter dropping message: frame 'base_scan' at time 1.226 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576624.688794261] [rviz]: Message Filter dropping message: frame 'odom' at time 1.242 for reason 'discarding message because the queue is full'
[component_container_isolated-6] [INFO] [1667576624.782879344] [global_costmap.global_costmap]: Timed out waiting for transform from base_link to map to become available, tf error: Invalid frame ID "map" passed to canTransform argument target_frame - frame does not exist
[rviz2-5] [INFO] [1667576624.848670474] [rviz]: Message Filter dropping message: frame 'base_scan' at time 1.426 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576624.880836799] [rviz]: Message Filter dropping message: frame 'odom' at time 1.446 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576625.072789968] [rviz]: Message Filter dropping message: frame 'base_scan' at time 1.627 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576625.104731315] [rviz]: Message Filter dropping message: frame 'odom' at time 1.650 for reason 'discarding message because the queue is full'
[rviz2-5] [INFO] [1667576625.264225721] [rviz]: Message Filter dropping message: frame 'base_scan' at time 1.826 for reason 'discarding message because the queue is full'
.
.
.
Y continúa...
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