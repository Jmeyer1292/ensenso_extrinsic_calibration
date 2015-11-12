cmake_minimum_required(VERSION 2.8.3)
project(grinding_ensenso_extrinsic_calibration)

find_package(catkin REQUIRED COMPONENTS
             roscpp
             std_msgs
             message_generation
             cv_bridge
             tf tf_conversions
             moveit_ros_planning_interface
             visualization_msgs)

find_package(PCL 1.8.0 REQUIRED)

add_service_files(FILES PerformEnsensoCalibration.srv
                        TestEnsensoCalibration.srv)
generate_messages(DEPENDENCIES std_msgs)

catkin_package(
  CATKIN_DEPENDS message_runtime std_msgs
)

include_directories(${PCL_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS}) # Order matters!
add_definitions(${PCL_DEFINITIONS})

add_executable(ensenso_extrinsic_calibration src/ensenso_extrinsic_calibration.cpp)
target_link_libraries(ensenso_extrinsic_calibration ${PCL_LIBRARIES} ${catkin_LIBRARIES})
add_dependencies(ensenso_extrinsic_calibration grinding_ensenso_extrinsic_calibration_generate_messages_cpp)

