cmake_minimum_required(VERSION 2.8.12)
project(multi_lidar_calibrator)

add_definitions(-std=c++11 -ofast)

find_package(catkin REQUIRED COMPONENTS
        roscpp
        std_msgs
        sensor_msgs
        geometry_msgs
        pcl_ros
        pcl_conversions
        eigen_conversions
        tf_conversions
        )

catkin_package(CATKIN_DEPENDS
        std_msgs
        sensor_msgs
        geometry_msgs
        )

find_package(OpenCV REQUIRED)

#find_package(yaml-cpp REQUIRED )

#find_package(PkgConfig REQUIRED)
#pkg_check_modules(YAML_CPP REQUIRED yaml-cpp)
#find_path(YAML_CPP_INCLUDE_DIR
#          NAMES yaml_cpp.h
#          PATHS ${YAML_CPP_INCLUDE_DIRS})
#find_library(YAML_CPP_LIBRARY
#             NAMES YAML_CPP
#             PATHS ${YAML_CPP_LIBRARY_DIRS})

#if(NOT ${YAML_CPP_VERSION} VERSION_LESS "0.5")
#    add_definitions(-DHAVE_NEW_YAMLCPP)
#endif(NOT ${YAML_CPP_VERSION} VERSION_LESS "0.5")
 
 
#link_directories(
#    /usr/lib/
#    )
 
#include_directories(
#    include
#    yaml-cpp）

###########
## Build ##
###########

include_directories(
        ${catkin_INCLUDE_DIRS}
        include
)



SET(CMAKE_CXX_FLAGS "-O2 -g -Wall ${CMAKE_CXX_FLAGS}")

# MultiLidar Calibrator
add_library(multi_lidar_calibrator_lib SHARED
        src/multi_lidar_calibrator.cpp
        include/multi_lidar_calibrator.h)

target_include_directories(multi_lidar_calibrator_lib PRIVATE
        ${OpenCV_INCLUDE_DIRS}
        )

target_link_libraries(multi_lidar_calibrator_lib
        ${catkin_LIBRARIES}
        /usr/local/lib/libyaml-cpp.so
        )

add_executable(multi_lidar_calibrator
        src/multi_lidar_calibrator_node.cpp
        )

target_include_directories(multi_lidar_calibrator PRIVATE
        include)

target_link_libraries(multi_lidar_calibrator
        multi_lidar_calibrator_lib)

install(TARGETS
        multi_lidar_calibrator multi_lidar_calibrator_lib
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(DIRECTORY launch/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch
        PATTERN ".svn" EXCLUDE)