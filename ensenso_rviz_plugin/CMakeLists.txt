cmake_minimum_required(VERSION 2.8.3)
project(ensenso_rviz_plugin)
#add_definitions( -std=c++11 )

find_package(catkin REQUIRED COMPONENTS
             message_generation
             rviz
             std_msgs)

add_service_files(FILES PerformEnsensoCalibration.srv
                        TestEnsensoCalibration.srv)
generate_messages(DEPENDENCIES std_msgs)

catkin_package(CATKIN_DEPENDS message_runtime
                              std_msgs)

include_directories(${catkin_INCLUDE_DIRS})

find_package(Qt5 COMPONENTS Core Widgets Concurrent REQUIRED)
#include(${QT_USE_FILE})

add_definitions(-DQT_NO_KEYWORDS)

qt5_wrap_cpp(MOC_FILES
  src/ensenso_panel.h
)

set(SOURCE_FILES
  src/ensenso_panel.cpp 
  ${MOC_FILES}
)

add_library(${PROJECT_NAME} ${SOURCE_FILES})
set_property(TARGET ${PROJECT_NAME} APPEND_STRING PROPERTY COMPILE_FLAGS -Wall)
target_link_libraries(${PROJECT_NAME} ${QT_LIBRARIES} ${catkin_LIBRARIES} Qt5::Widgets Qt5::Core Qt5::Concurrent)
add_dependencies(${PROJECT_NAME} ${PROJECT_NAME}_generate_messages_cpp)

install(TARGETS
  ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(FILES 
  plugin_description.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

