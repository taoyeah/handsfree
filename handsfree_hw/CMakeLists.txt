cmake_minimum_required(VERSION 2.8.3)
project(handsfree_hw)

find_package(catkin REQUIRED COMPONENTS
  controller_manager
  geometry_msgs
  hardware_interface
  realtime_tools
  roscpp
  std_msgs
  handsfree_msgs
)

set(config_path \"${CMAKE_CURRENT_SOURCE_DIR}\")
add_definitions(-DCONFIG_PATH=${config_path})
catkin_package(   
  INCLUDE_DIRS include
#  LIBRARIES handsfree_hw
#  CATKIN_DEPENDS controller_manager geometry_msgs hardware_interface realtime_tools roscpp std_msgs
#  DEPENDS system_lib
)

execute_process(COMMAND sh ${CMAKE_CURRENT_SOURCE_DIR}/env.sh)

include_directories(
  include 
  ${catkin_INCLUDE_DIRS}
  include/sdk/hf_link/
  include/sdk/robot_abstract/
)

add_executable(handsfree_hw_node src/main.cpp src/transport_serial.cpp include/sdk/hf_link/hf_link.cpp include/sdk/hf_link/hf_link_state_machine.cpp src/hf_hw.cpp src/hf_hw_ros.cpp)

add_dependencies(handsfree_hw_node handsfree_msgs_gencpp)

target_link_libraries(handsfree_hw_node ${catkin_LIBRARIES})


