cmake_minimum_required(VERSION 2.8.3)
project(octomap_world)

find_package(catkin_simple REQUIRED
            pcl_ros
            pcl_conversions)
find_package(OpenCV REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

option(USE_OCTOMAP_OMP "Use OMP in Octomap" OFF)

if (USE_OCTOMAP_OMP)
  FIND_PACKAGE( OpenMP REQUIRED)
  SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
  SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif (USE_OCTOMAP_OMP)

add_definitions(-std=c++14)

#############
# LIBRARIES #
#############
cs_add_library(${PROJECT_NAME}
  src/octomap_world.cc
  src/octomap_manager.cc
)

include_directories(
    ${OpenCV_INCLUDE_DIRS}
)
link_directories(
    ${OpenCV_LIBRARY_DIRS}
)
############
# BINARIES #
############
cs_add_executable(octomap_manager
  src/octomap_manager_node.cc
)
target_link_libraries(octomap_manager ${PROJECT_NAME} ${OpenCV_LIBS})

##########
# EXPORT #
##########
cs_install()
cs_export()
