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

   
   * source /opt/ros/humble/setup.bash
   * export TURTLEBOT3_MODEL=waffle
   * export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/humble/share/turtlebot3_gazebo/models
   * export LDS_MODEL=1 # Esto usarlo como último recurso
   * ros2 launch nav2_bringup tb3_simulation_launch.py slam:=True



# Listado en crudo de todos los comandos
    1  sudo apt update
    2  sudo apt update && sudo apt install curl gnupg2 lsb-release
    3  sudo apt update && sudo apt install curl gnupg2 lsb-release
    4  curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
    5  sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'
    6  mkdir -p ~/ros2_crystal
    7  cd ~/ros2_crystal
    8  tar xf ~/Downloads/ros2-crystal-linux-x86_64.tar.bz2
    9  tar xf ~/Downloads/ros2-foxy-20220928-linux-focal-amd64.tar.bz2 
    10  sudo apt update
    11  sudo apt install -y python-rosdep
    12  sudo apt update && sudo apt install curl gnupg2 lsb-release
    13  curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
    14  sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'
    15  sudo apt update
    16  sudo apt install -y python-rosdep
    17  python3 --version
    18  sudo apt install -y python3-rosdep
    19  sudo rosdep init
    20  rosdep update
    21  ros2
    22  CHOOSE_ROS_DISTRO=humble
    23  rosdep install --from-paths ros2-linux/share --ignore-src --rosdistro $CHOOSE_ROS_DISTRO -y --skip-keys "console_bridge fastcdr fastrtps libopensplice67 libopensplice69 osrf_testing_tools_cpp poco_vendor rmw_connext_cpp rosidl_typesupport_connext_c rosidl_typesupport_connext_cpp rti-connext-dds-5.3.1 tinyxml_vendor tinyxml2_vendor urdfdom urdfdom_headers"
    24  rosdep install --from-paths ros2-linux/share --ignore-src --rosdistro crystal -y --skip-keys "console_bridge fastcdr fastrtps libopensplice67 libopensplice69 osrf_testing_tools_cpp poco_vendor rmw_connext_cpp rosidl_typesupport_connext_c rosidl_typesupport_connext_cpp rti-connext-dds-5.3.1 tinyxml_vendor tinyxml2_vendor urdfdom urdfdom_headers"
    25  $ rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y --os=ubuntu:jammy
    26  rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y --os=ubuntu:jammy
    27  gksudo gedit /etc/default/grub
    28  sudo apt install gksudo
    29  sudo gedit /etc/default/grub
    30  sudo update-grub
    31  sudo gedit /etc/default/grub
    32  sudo update-grub
    33  coppelia
    34  ros2
    35  locale  # check for UTF-8
    36  sudo apt update && sudo apt install locales
    37  sudo apt update && sudo apt install locales~
    38  sudo locale-gen en_US en_US.UTF-8
    39  sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
    40  export LANG=en_US.UTF-8
    41  locale
    42  apt-cache policy | grep universe
    43  sudo apt install software-properties-common
    44  sudo add-apt-repository universe
    45  sudo apt update && sudo apt install curl gnupg2 lsb-release
    46  sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg
    47  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
    48  sudo apt update && sudo apt install -y   build-essential   cmake   git   libbullet-dev   python3-colcon-common-extensions   python3-flake8   python3-pip   python3-pytest-cov   python3-rosdep   python3-setuptools   python3-vcstool   wget
    49  python3 -m pip install -U   argcomplete   flake8-blind-except   flake8-builtins   flake8-class-newline   flake8-comprehensions   flake8-deprecated   flake8-docstrings   flake8-import-order   flake8-quotes   pytest-repeat   pytest-rerunfailures   pytest
    50  export PATH=$PATH:/home/manuel/.local/bin
    51  python3 -m pip install -U   argcomplete   flake8-blind-except   flake8-builtins   flake8-class-newline   flake8-comprehensions   flake8-deprecated   flake8-docstrings   flake8-import-order   flake8-quotes   pytest-repeat   pytest-rerunfailures   pytest
    52  sudo apt install --no-install-recommends -y   libasio-dev   libtinyxml2-dev
    53  sudo apt install --no-install-recommends -y   libcunit1-dev
    54  mkdir -p ~/ros2_foxy/src
    55  cd ~/ros2_foxy
    56  vcs import --input https://raw.githubusercontent.com/ros2/ros2/foxy/ros2.repos src
    57  sudo apt upgrade
    58  sudo rosdep init
    59  rm /etc/ros/rosdep/sources.list.d/20-default.list
    60  sudo rosdep init
    61  sudo rm /etc/ros/rosdep/sources.list.d/20-default.list
    62  sudo rosdep init
    63  rosdep update
    64  rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-5.3.1 urdfdom_headers"
    65  apt-get update & apt-get install net-tools
    66  sudo apt-get update && apt-get install net-tools
    67  ps aux | grep -i apt
    68  cd ~
    69  ls
    70  cd ros2_foxy/
    71  rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-5.3.1 urdfdom_headers"
    72  export ROS_DISTRO=jammy
    73  export ROS_PYTHON_VERSION=3
    74  rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-5.3.1 urdfdom_headers"
    75  python3 --version
    76  cd /root/ros_catkin_ws/build_isolated/gl_dependency && /root/ros_catkin_ws/install_isolated/env.sh cmake
    77  /root/ros_catkin_ws/src/gl_dependency
    78  -DCATKIN_DEVEL_PREFIX=/root/ros_catkin_ws/devel_isolated/gl_dependency
    79  -DCMAKE_INSTALL_PREFIX=/root/ros_catkin_ws/install_isolated
    80  -DPYTHON_EXECUTABLE=/usr/local/bin/anaconda3/bin/python
    81  -DPYTHON_INCLUDE_DIR=/usr/local/bin/anaconda3/include/python3.7m
    82  -DCMAKE_BUILD_TYPE=Release -G 'Unix Makefiles'
    83  cd /root/ros_catkin_ws/build_isolated/gl_dependency
    84  sudo cd /root/ros_catkin_ws/build_isolated/gl_dependency
    85  ls -la /root/
    86  sudo ls -la /root/
    87  rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-5.3.1 urdfdom_headers"
    88  . ~/ros2_humble/install/local_setup.bash
    89  ros2 run turtlesim turtle_teleop_key
    90  sudo apt update && sudo apt install curl gnupg lsb-release
    91  sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
    92  echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
    93  sudo apt update && sudo apt install -y   build-essential   cmake   git   python3-colcon-common-extensions   python3-flake8   python3-flake8-docstrings   python3-pip   python3-pytest   python3-pytest-cov   python3-rosdep   python3-setuptools   python3-vcstool   wget
    94  sudo apt install -y    python3-flake8-blind-except
    95  mkdir -p ~/ros2_humble/src
    96  rm -r ~/ros2_*
    97  sudo rm -r ~/ros2_*
    98  mkdir -p ~/ros2_humble/src
    99  cd ~/ros2_humble
    100  vcs import --input https://raw.githubusercontent.com/ros2/ros2/humble/ros2.repos src
    101  sudo apt upgrade
    102  sudo rosdep init
    103  sudo rm /etc/ros/rosdep/sources.list.d/20-default.list
    104  sudo rosdep init
    105  rosdep update
    106  rosdep install --from-paths src --ignore-src -y --skip-keys "fastcdr rti-connext-dds-6.0.1 urdfdom_headers"
    107  cd ~/ros2_humble/
    108  colcon build --symlink-install
    109  . ~/ros2_humble/install/local_setup.bash
    110  ros2 run demo_nodes_cpp talker
    111  cd ..
    112  mkdir Coppelia
    113  mv ~/Downloads/CoppeliaSim_Edu_V4_4_0_rev0_Ubuntu22_04 Coppelia/
    114  cd coppelia/CoppeliaSim_Edu_V4_4_0_rev0_Ubuntu22_04/
    115  cd ../..
    116  ls -la coppelia/CoppeliaSim_Edu_V4_4_0_rev0_Ubuntu22_04/ | grep sh
    117  ./coppelia/CoppeliaSim_Edu_V4_4_0_rev0_Ubuntu22_04/coppeliaSim.sh 
    118  mkdir -p ~/rviz2_ws/src
    119  cd ~/rviz2_ws/src
    120  git clone https://github.com/ros2/rviz.git
    121  colcon build --merge-install
    122  roscore
    123  sudo apt install python3-roslaunch
    124  roscore
    125  ros2 run demo_nodes_cpp talker
    126  rosrun rviz rviz
    127  sudo apt install rosbash
    128  apt-cache depends --recurse rosbash
    129  echo $ROS_DISTRO
    130  sudo apt install rosbash
    131  sudo aptitude install rosbash
    132  apt-get install aptitude
    133  sudo apt-get install aptitude
    134  sudo aptitude install rosbash
    135  sudo roscore
    136  roscore
    137  rviz2
    138  history | grep co
    139  sudo apt update
    140  sudo apt install ros-humble-turtlesim
    141  ros2 pkg executables turtlesim
    142  ros2 run turtlesim turtlesim_node
    143  udo apt install ros-humble-slam-toolbox
    144  . ~/ros2_humble/install/local_setup.bash
    145  ros2 run turtlesim turtle_teleop_key
    146  sudo apt install ros-<ros2-distro>-slam-toolbox
    147  sudo apt install ros-humble-slam-toolbox
    148  export TURTLEBOT3_MODEL=waffle
    149  ros2 launch turtlebot3_bringup robot.launch.py
    150  sudo apt get install turtlebot3_bringup
    151  sudo apt install turtlebot3_bringup
    152  sudo apt-get install ros-kinetic-turtlebot3-bringup
    153  mkdir -p ~/turtlebot3_ws/src
    154  cd ~/turtlebot3_ws
    155  wget https://raw.githubusercontent.com/ROBOTIS-GIT/turtlebot3/ros2/turtlebot3.repos
    156  vcs import src < turtlebot3.repos
    157  colcon build --symlink-install
    158  sudo apt install ros-humble-navigation2
    159  sudo apt install ros-humble-nav2-bringup
    160  sudo apt install ros-humble-turtlebot3*
    161  source /opt/ros/<ros2-distro>/setup.bash
    162  export TURTLEBOT3_MODEL=waffle
    163  export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/<ros2-distro>/share/turtlebot3_gazebo/models
    164  export TURTLEBOT3_MODEL=waffle
    165  export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/<ros2-distro>/share/turtlebot3_gazebo/models
    166  export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/humble/share/turtlebot3_gazebo/models
    167  ros2 launch nav2_bringup tb3_simulation_launch.py headless:=False
    168  sudo apt install ros-humble-navigation2
    169  sudo apt install ros-humble-nav2-bringup
    170  ros2 launch nav2_bringup tb3_simulation_launch.py headless:=False
    171  ros2 launch turtlebot3_bringup robot.launch.py
    172  source /opt/ros/humble/setup.bash
    173  export TURTLEBOT3_MODEL=waffle
    174  ros2 launch turtlebot3_bringup robot.launch.py
    175  gedit /home/manuel/.ros/log/2022-10-17-20-55-31-384200-manuel-VivoBook-ASUSLaptop-X421EAYB-K413EA-243696/launch.log 
    176  ros2 launch --log-level debug turtlebot3_bringup robot.launch.py
    177  ros2 -h
    178  ros2 launch turtlebot3_bringup robot.launch.py --log-level debug
    179  ros2 launch turtlebot3_bringup robot.launch.py --ros-args --log-level DEBUG
    180  cd ..
    181  ls -la
    182  cd ros2_humble/
    183  ros2 launch turtlebot3_bringup robot.launch.py
    184  echo 'export LDS_MODEL=LDS-02' >> ~/.bashrc
    185  ros2 launch turtlebot3_bringup robot.launch.py
    186  echo $LDS_MODEL
    187  export LDS_MODEL=LDS-02
    188  echo $LDS_MODEL
    189  ros2 launch turtlebot3_bringup robot.launch.py
    190  export LDS_MODEL=0
    191  ros2 launch turtlebot3_bringup robot.launch.py
    192  sudo ros2 launch turtlebot3_bringup robot.launch.py
    193  ros2 launch turtlebot3_bringup robot.launch.py
    194  export LDS_MODEL=1
    195  ros2 launch turtlebot3_bringup robot.launch.py
    196  export LDS_MODEL=A1
    197  ros2 launch turtlebot3_bringup robot.launch.py
    198  ros2 launch nav2_bringup tb3_simulation_launch.py headless:=False
    199  . ~/ros2_humble/install/local_setup.bash
    200  source /opt/ros/humble/setup.bash
    201  export TURTLEBOT3_MODEL=waffle
    202  export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/humble/share/turtlebot3_gazebo/models
    203  ros2 launch nav2_bringup navigation_launch.py
    204  . ~/ros2_humble/install/local_setup.bash
    205  sudo apt update
    206  sudo apt install ~nros-humble-rqt*
    207  rqt
    208  cd ../..
    209  ./coppelia/CoppeliaSim_Edu_V4_4_0_rev0_Ubuntu22_04/coppeliaSim.sh
    210  source /opt/ros/humble/setup.bash
    211  export TURTLEBOT3_MODEL=waffle
    212  export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/humble/share/turtlebot3_gazebo/models
    213  LDS_MODEL=1
    214  ros2 launch slam_toolbox online_async_launch.py
    215  ros2 launch nav2_bringup tb3_simulation_launch.py slam:=True
    216  history 

