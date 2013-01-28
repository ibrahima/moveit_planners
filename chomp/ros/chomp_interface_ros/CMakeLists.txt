cmake_minimum_required(VERSION 2.8.3)
project(moveit_chomp_interface_ros)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(Eigen REQUIRED)
find_package(Boost REQUIRED system filesystem date_time thread serialization)
find_package(catkin REQUIRED COMPONENTS
  moveit_core
  eigen_conversions
  moveit_chomp_planner
  )

set(MOVEIT_LIB_NAME moveit_chomp_planner)

catkin_package(
  LIBRARIES
    ${MOVEIT_LIB_NAME}
  INCLUDE_DIRS
    include
  CATKIN_DEPENDS
    moveit_core
    moveit_chomp_planner)

include_directories(SYSTEM
                    ${EIGEN_INCLUDE_DIRS}
                    ${Boost_INCLUDE_DIRS})

include_directories(include 
                    ${catkin_INCLUDE_DIRS})

link_directories(${catkin_LIBRARY_DIRS})

add_library(${PROJECT_NAME} src/chomp_interface_ros.cpp)

set(SOURCE_FILES src/chomp_plugin.cpp)
set(LIBRARY_NAME chomp_planner_plugin)
# include(CMakeDetermineSystem)
# if(CMAKE_SYSTEM_NAME MATCHES "Darwin")
#   rosbuild_add_library_module(${LIBRARY_NAME} ${SOURCE_FILES})
# else(CMAKE_SYSTEM_NAME MATCHES "Darwin")
#   rosbuild_add_library(${LIBRARY_NAME} ${SOURCE_FILES})
# endif(CMAKE_SYSTEM_NAME MATCHES "Darwin")
add_library(${LIBRARY_NAME} ${SOURCE_FILES})

target_link_libraries(${LIBRARY_NAME} ${PROJECT_NAME})
