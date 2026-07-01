set(EXECUTABLE_NAME detect_aruco_markers_behavior)

# Find dependencies
set(EXECUTABLE_DEPENDENCIES
  sensor_msgs
)

foreach(DEPENDENCY ${EXECUTABLE_DEPENDENCIES})
  find_package(${DEPENDENCY} REQUIRED)
endforeach()

# Include directories
include_directories(
  include
  include/${EXECUTABLE_NAME}
)

set(SOURCE_CPP_FILES
  src/${EXECUTABLE_NAME}.cpp
  src/${EXECUTABLE_NAME}_node.cpp
)

add_executable(${EXECUTABLE_NAME}_node ${SOURCE_CPP_FILES})
target_link_libraries(${EXECUTABLE_NAME}_node
  rclcpp::rclcpp
  std_msgs::std_msgs
  std_srvs::std_srvs
  as2_core::as2_core
  as2_msgs::as2_msgs
  as2_behavior::as2_behavior
  OpenCV
  cv_bridge::cv_bridge
  sensor_msgs::sensor_msgs
)

install(DIRECTORY
  config
  DESTINATION share/${PROJECT_NAME}/${EXECUTABLE_NAME})

install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME})

install(TARGETS
  ${EXECUTABLE_NAME}_node
  DESTINATION lib/${PROJECT_NAME})

ament_export_dependencies(${EXECUTABLE_DEPENDENCIES})
ament_export_include_directories(include)