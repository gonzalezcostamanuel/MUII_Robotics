# Listado de todos los comandos lanzados en este proyecton para UBUNTU 20.04

## Instalación de git

* sudo apt-get install git-all
* git config --global user.name "Username"
* git config --global user.email "MY_NAME@example.com"


## Instalación de docker

* Sacados de https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-es
    * sudo apt install apt-transport-https ca-certificates curl software-properties-common
    * curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    * sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
    * sudo apt update
    * apt-cache policy docker-ce
    * sudo apt install docker-ce
    * sudo systemctl status docker

    Ya tendríamos docker instalado, si queremos lanzar los comandos de docker sin sudo podemos lanzar los siguientes comandos, pero ten en cuenta que los cambios son sólo locales (si cierras y vuelves a abrir la consola tendrías que volver a ejecutar los comandos de nuevo)

    * sudo usermod -aG docker ${USER}
    * su - ${USER}
    * id -nG


   Para lanzar rviz
   * rviz2

   Para lanzar Coppelia
   * ./coppelia/CoppeliaSim_Edu_V4_4_0_rev0_Ubuntu22_04/coppeliaSim.sh 

   Para lanzar ROS2
   * . ~/ros2_humble/install/local_setup.bash

   Desconfío si el problema para lanzar el ejemplo de SLAM será por el export LDS_MODEL=1 que me he sacado de la manga

