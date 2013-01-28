cmake_minimum_required(VERSION 2.8.3)
project(moveit_chomp_planner)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

find_package(Eigen REQUIRED)
find_package(Boost REQUIRED system filesystem date_time thread serialization)
find_package(catkin REQUIRED COMPONENTS
  moveit_core
  eigen_conversions
  )

set(MOVEIT_LIB_NAME moveit_chomp_planner)

catkin_package(
  LIBRARIES
    ${MOVEIT_LIB_NAME}
  INCLUDE_DIRS
    include
  CATKIN_DEPENDS
    moveit_core)

include_directories(SYSTEM
                    ${EIGEN_INCLUDE_DIRS}
                    ${Boost_INCLUDE_DIRS})

include_directories(include 
                    ${catkin_INCLUDE_DIRS})

link_directories(${catkin_LIBRARY_DIRS})

add_library(${MOVEIT_LIB_NAME}
  src/chomp_cost.cpp
  src/chomp_parameters.cpp
  src/chomp_trajectory.cpp
  src/chomp_optimizer.cpp
  src/chomp_planner.cpp
)

target_link_libraries(${MOVEIT_LIB_NAME} ${catkin_LIBRARIES})