set(PLUGIN_NAME dc_servo)

# find dependencies
set(PLUGIN_DEPENDENCIES
${SUBPROJECT_DEPENDENCIES}
)

foreach(DEPENDENCY ${PLUGIN_DEPENDENCIES})
  find_package(${DEPENDENCY} REQUIRED)
endforeach()


include_directories(
  include
  include/${PLUGIN_NAME}
  ${CMAKE_CURRENT_SOURCE_DIR}/../../include
)

set(SOURCE_CPP_FILES
  src/${PLUGIN_NAME}.cpp
)

# Library
add_library(${PLUGIN_NAME} SHARED ${SOURCE_CPP_FILES})
target_link_libraries(${PLUGIN_NAME}
  ${SUBPROJECT_NAME}_lib
  ${SUBPROJECT_NAME}_plugin_base
)
target_link_libraries(${PLUGIN_NAME}
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

install(
  DIRECTORY include/
  DESTINATION include/${PROJECT_NAME}/${SUBPROJECT_NAME}
)

ament_export_include_directories(
  include
)
ament_export_libraries(
  ${PLUGIN_NAME}
)
ament_export_targets(
  export_${PLUGIN_NAME}
)

install(
  TARGETS ${PLUGIN_NAME}
  EXPORT export_${PLUGIN_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)