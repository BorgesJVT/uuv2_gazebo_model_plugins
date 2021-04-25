cmake_minimum_required(VERSION 3.5)
project(uuv2_gazebo_model_plugins)

find_package(ament_cmake REQUIRED)
find_package(GAZEBO REQUIRED)
find_package(ignition-math6 REQUIRED)

add_library(buoyant_object src/BuoyantObject.cc)
add_library(hydrodynamic_model src/HydrodynamicModel.cc)

target_include_directories(buoyant_object PUBLIC include)
target_include_directories(hydrodynamic_model PUBLIC include)

ament_target_dependencies(buoyant_object PUBLIC GAZEBO)
ament_target_dependencies(hydrodynamic_model PUBLIC
  GAZEBO
  ignition-math6
)

# Auto lint
if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
