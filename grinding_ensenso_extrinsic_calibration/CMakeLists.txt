cmake_minimum_required(VERSION 2.8.3)
project(grinding_ensenso_extrinsic_calibration)
add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
             cv_bridge
             moveit_ros_planning_interface
             pcl_ros
             roscpp
             std_msgs
             tf tf_conversions
             visualization_msgs
             ensenso_rviz_plugin)

find_package(PCL 1.8.0 REQUIRED)

catkin_package()

include_directories(${PCL_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS}) # Order matters!
add_definitions(${PCL_DEFINITIONS})

add_executable(${PROJECT_NAME} src/ensenso_extrinsic_calibration.cpp)
set_property(TARGET ${PROJECT_NAME} APPEND_STRING PROPERTY COMPILE_FLAGS -Wall)
target_link_libraries(${PROJECT_NAME} ${PCL_LIBRARIES} ${catkin_LIBRARIES})
add_dependencies(${PROJECT_NAME} ensenso_rviz_plugin_generate_messages_cpp)

install(TARGETS
  ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY launch/
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch)
