cmake_minimum_required(VERSION 3.5)
project(uuv2_gazebo_model_plugins)

find_package(ament_cmake REQUIRED)
find_package(GAZEBO REQUIRED)
find_package(ignition-math6 REQUIRED)
find_package(Protobuf REQUIRED)

# GENERATE PROTOBUF MESSAGES
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS msgs/double.proto)


# BUILD

# uuv2_gazebo_model_plugins_msgs
add_library(uuv2_gazebo_model_plugins_msgs ${PROTO_SRCS} ${PROTO_PROTO_HDRS})
target_link_libraries(uuv2_gazebo_model_plugins_msgs ${PROTOBUF_LIBRARY})

# underwater_object_plugin
add_library(underwater_object_plugin SHARED
  src/BuoyantObject.cpp
  src/HydrodynamicModel.cpp
  src/UnderwaterObjectPlugin.cpp
)

target_include_directories(underwater_object_plugin PUBLIC include)
ament_target_dependencies(underwater_object_plugin PUBLIC GAZEBO ignition-math6)

# # thruster plugin
add_library(thruster_plugin SHARED
  src/ThrusterConversionFunction.cpp
  src/ThrusterDynamics.cpp
  src/ThrusterPlugin.cpp
)

target_include_directories(thruster_plugin PUBLIC include ${CMAKE_CURRENT_BINARY_DIR})
ament_target_dependencies(thruster_plugin PUBLIC GAZEBO ignition-math6)


# EXPORT
ament_export_libraries(underwater_object_plugin)
ament_export_dependencies(GAZEBO ignition-math6)


# INSTALL
install(
  DIRECTORY include/
  DESTINATION include
)

install(
  TARGETS underwater_object_plugin uuv2_gazebo_model_plugins_msgs thruster_plugin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/
  DESTINATION include
  FILES_MATCHING PATTERN "*.pb.*"
  PATTERN "*~" EXCLUDE
)


# TEST
if(BUILD_TESTING)
  # find_package(ament_lint_auto REQUIRED)
  find_package(launch_testing_ament_cmake REQUIRED)
  find_package(gazebo_ros REQUIRED)

  # ament_lint_auto_find_test_dependencies()
  add_launch_test(test/underwater_sphere_spawn.test.py)

endif()

ament_package()
