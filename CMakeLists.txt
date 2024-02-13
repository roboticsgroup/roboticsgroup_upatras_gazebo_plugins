
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(roboticsgroup_upatras_gazebo_plugins)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(gazebo_dev REQUIRED)
find_package(gazebo_ros REQUIRED)
find_package(control_toolbox REQUIRED)
find_package(gazebo REQUIRED)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wconversion -Wno-sign-conversion -Wpedantic -Wnon-virtual-dtor -Woverloaded-virtual)
endif()

include_directories(include)
link_directories(
  ${gazebo_dev_LIBRARY_DIRS}
)

add_library(roboticsgroup_upatras_gazebo_mimic_joint_plugin SHARED src/mimic_joint_plugin.cpp)
add_library(roboticsgroup_upatras_gazebo_disable_link_plugin SHARED src/disable_link_plugin.cpp)

ament_target_dependencies(
  roboticsgroup_upatras_gazebo_mimic_joint_plugin
  rclcpp
  gazebo_dev
  gazebo_ros
  sensor_msgs
  control_toolbox
)

ament_target_dependencies(
  roboticsgroup_upatras_gazebo_disable_link_plugin
  rclcpp
  gazebo_dev
  gazebo_ros
  sensor_msgs
)

# this line to exports the library
ament_export_include_directories(
  include
)
ament_export_dependencies(ament_cmake)
ament_export_dependencies(rclcpp)

ament_export_libraries(
  ${PROJECT_NAME}
)

install(DIRECTORY include/${PROJECT_NAME}/ DESTINATION include/${PROJECT_NAME})

install(TARGETS
    roboticsgroup_upatras_gazebo_mimic_joint_plugin
    roboticsgroup_upatras_gazebo_disable_link_plugin
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

ament_package()
