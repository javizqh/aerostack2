set(SUBPROJECT_NAME point_gimbal_behavior)

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
  include/${SUBPROJECT_NAME}
)

set(SOURCE_CPP_FILES
  src/${SUBPROJECT_NAME}.cpp
  src/${SUBPROJECT_NAME}_node.cpp
)

add_executable(${SUBPROJECT_NAME}_node ${SOURCE_CPP_FILES})
target_link_libraries(${SUBPROJECT_NAME}_node
  rclcpp::rclcpp
  rclcpp_action::rclcpp_action
  std_msgs::std_msgs
  std_srvs::std_srvs
  as2_core::as2_core
  as2_msgs::as2_msgs
  as2_behavior::as2_behavior
  geometry_msgs::geometry_msgs
  pluginlib::pluginlib
  sensor_msgs::sensor_msgs
)

# Library
add_library(${SUBPROJECT_NAME} SHARED ${SOURCE_CPP_FILES})
target_link_libraries(${SUBPROJECT_NAME}
  rclcpp::rclcpp
  rclcpp_action::rclcpp_action
  std_msgs::std_msgs
  std_srvs::std_srvs
  as2_core::as2_core
  as2_msgs::as2_msgs
  as2_behavior::as2_behavior
  geometry_msgs::geometry_msgs
  pluginlib::pluginlib
)

# Include headers in the library
target_include_directories(${SUBPROJECT_NAME} PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

# Install
install(
  DIRECTORY include/
  DESTINATION include
)

install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME})

install(DIRECTORY
  config
  DESTINATION share/${PROJECT_NAME}/${SUBPROJECT_NAME})

install(TARGETS
  ${SUBPROJECT_NAME}_node
  DESTINATION lib/${PROJECT_NAME})

ament_export_include_directories(
  include
)

ament_export_libraries(
  ${SUBPROJECT_NAME}
)

ament_export_targets(
  export_${SUBPROJECT_NAME}
)

install(
  TARGETS ${SUBPROJECT_NAME}
  EXPORT export_${SUBPROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

if(BUILD_TESTING)
  add_subdirectory(tests)
endif()