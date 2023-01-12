# Buscamos el fichero .py

Buscamos:

tb3_simulation_launch.py


    manuel@manuel-VivoBook-ASUSLaptop-X421EAYB-K413EA:~$ cd /opt/ros/humble/
    manuel@manuel-VivoBook-ASUSLaptop-X421EAYB-K413EA:/opt/ros/humble$ !160
    find . -name tb3_simulation_launch.py -print
    ./share/nav2_bringup/launch/tb3_simulation_launch.py

    gedit share/nav2_bringup/launch/tb3_simulation_launch.py


Parece ser que para modificar el planner tengo que cambiarlo en:

    sudo gedit /opt/ros/humble/share/nav2_bringup/params/nav2_params.yaml

    planner_server:
      ros__parameters:
        expected_planner_frequency: 20.0
        use_sim_time: True
        planner_plugins: ["GridBased"]
        GridBased:
          plugin: "nav2_navfn_planner/NavfnPlanner"
          tolerance: 0.5
          use_astar: false
          allow_unknown: true

    Opciones:

    https://navigation.ros.org/plugins/index.html#planners
        nav2_navfn_planner/NavfnPlanner
        nav2_theta_star_planner/ThetaStarPlanner
        nav2_smac_planner/SmacPlanner2D
        nav2_smac_planner/SmacPlannerHybrid
        nav2_smac_planner/SmacPlannerLattice


Parece que funciona porque:

    
    [component_container_isolated-6] [INFO] [1668705846.862447879] [planner_server]: Created global planner plugin GridBased of type nav2_theta_star_planner/ThetaStarPlanner
    [component_container_isolated-6] [INFO] [1668705846.863188336] [planner_server]: Planner Server has GridBased  planners available.

Y el robot camina como de costumbre (Creo que algo más errático que de costumbre)



Queremos sacar la información de los tópicos, por lo que primero listamos los tópicos existentes

    manuel@manuel-VivoBook-ASUSLaptop-X421EAYB-K413EA:~$ ros2 topic list
    /amcl/transition_event
    /amcl_pose
    /behavior_server/transition_event
    /bond
    /bt_navigator/transition_event
    /clicked_point
    /clock
    /cmd_vel
    /cmd_vel_nav
    /controller_server/transition_event
    /cost_cloud
    /diagnostics
    /downsampled_costmap
    /downsampled_costmap_updates
    /evaluation
    /global_costmap/costmap
    /global_costmap/costmap_raw
    /global_costmap/costmap_updates
    /global_costmap/footprint
    /global_costmap/global_costmap/transition_event
    /global_costmap/published_footprint
    /global_costmap/voxel_marked_cloud
    /goal_pose
    /imu
    /initialpose
    /intel_realsense_r200_depth/camera_info
    /intel_realsense_r200_depth/depth/camera_info
    /intel_realsense_r200_depth/depth/image_raw
    /intel_realsense_r200_depth/image_raw
    /intel_realsense_r200_depth/points
    /joint_states
    /local_costmap/clearing_endpoints
    /local_costmap/costmap
    /local_costmap/costmap_raw
    /local_costmap/costmap_updates
    /local_costmap/footprint
    /local_costmap/local_costmap/transition_event
    /local_costmap/published_footprint
    /local_costmap/voxel_grid
    /local_costmap/voxel_marked_cloud
    /local_plan
    /map
    /map_server/transition_event
    /map_updates
    /marker
    /mobile_base/sensors/bumper_pointcloud
    /odom
    /parameter_events
    /particle_cloud
    /performance_metrics
    /plan
    /plan_smoothed
    /planner_server/transition_event
    /received_global_plan
    /robot_description
    /rosout
    /scan
    /smoother_server/transition_event
    /speed_limit
    /tf
    /tf_static
    /transformed_global_plan
    /velocity_smoother/transition_event
    /waypoint_follower/transition_event
    /waypoints



manuel@manuel-VivoBook-ASUSLaptop-X421EAYB-K413EA:~$ ros2 node info /planner_server
/planner_server
  Subscribers:
    /clock: rosgraph_msgs/msg/Clock
    /parameter_events: rcl_interfaces/msg/ParameterEvent
  Publishers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /plan: nav_msgs/msg/Path
    /planner_server/transition_event: lifecycle_msgs/msg/TransitionEvent
    /rosout: rcl_interfaces/msg/Log
  Service Servers:
    /planner_server/change_state: lifecycle_msgs/srv/ChangeState
    /planner_server/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /planner_server/get_available_states: lifecycle_msgs/srv/GetAvailableStates
    /planner_server/get_available_transitions: lifecycle_msgs/srv/GetAvailableTransitions
    /planner_server/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /planner_server/get_parameters: rcl_interfaces/srv/GetParameters
    /planner_server/get_state: lifecycle_msgs/srv/GetState
    /planner_server/get_transition_graph: lifecycle_msgs/srv/GetAvailableTransitions
    /planner_server/list_parameters: rcl_interfaces/srv/ListParameters
    /planner_server/set_parameters: rcl_interfaces/srv/SetParameters
    /planner_server/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
  Service Clients:

  Action Servers:
    /compute_path_through_poses: nav2_msgs/action/ComputePathThroughPoses
    /compute_path_to_pose: nav2_msgs/action/ComputePathToPose
  Action Clients:


ros2 topic echo /plan | awk '{ print strftime("%s: "), $0; fflush(); }' | tee final_topic_plan_SmacPlannerHybrid_0.txt

 -- No me aporta nada
ros2 topic echo /planner_server/transition_event | awk '{ print strftime("%s: "), $0; fflush(); }' | tee log_topic_transition_event.txt
 -- No me aporta nada
ros2 topic echo /parameter_events | awk '{ print strftime("%s: "), $0; fflush(); }' | tee log_topic_parameter_events.txt



Tiempo que ha tardado el NavfnPlanner --> 21 segundos entre el primer y el último log


NavfnPlanner --> 96000000 - 664000000 = 568000000 = 0,568 segundos entre primer y ultimo log

ThetaPlanner --> 965000000 - 928000000 = 37000000 = 0,037 segundos entre primer y ultimo log
ThetaPlanner --> 490000000 - 67000000 = 423000000 = 0,423 segundos entre primer y ultimo log



Cálculos a hacer:

  - Media de las distancias de todos los puntos a la recta entre el punto A y B (indica cuán recto o retorcido es un camino
  - Longitud del camino, sumando todas las distancias parciales entre todos los puntos
  - Número de recálculos de rutas, cuantos más recálculos más coste de procesamiento, lo ideal sería sólo un cálculo y ya
