cmake_minimum_required(VERSION 3.5)
project(uuv2_gazebo_model_plugins)

find_package(ament_cmake REQUIRED)
find_package(GAZEBO REQUIRED)
find_package(ignition-math6 REQUIRED)

# BUILD
add_library(underwater_object_plugin SHARED
  src/BuoyantObject.cpp
  src/HydrodynamicModel.cpp
  src/UnderwaterObjectPlugin.cpp
)

target_include_directories(underwater_object_plugin PUBLIC include)
ament_target_dependencies(underwater_object_plugin PUBLIC GAZEBO ignition-math6)

# EXPORT
ament_export_libraries(underwater_object_plugin)
ament_export_dependencies(GAZEBO ignition-math6)

# INSTALL
install(
  DIRECTORY include/
  DESTINATION include
)

install(
  TARGETS underwater_object_plugin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
)


# TEST
if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
