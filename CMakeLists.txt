cmake_minimum_required(VERSION 3.5)
project(uuv2_gazebo_model_plugins)

find_package(ament_cmake REQUIRED)
find_package(GAZEBO REQUIRED)

add_library(buoyant_object src/BuoyantObject.cc)

target_include_directories(buoyant_object PUBLIC include)
ament_target_dependencies(buoyant_object PUBLIC GAZEBO)

# Auto lint
if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
