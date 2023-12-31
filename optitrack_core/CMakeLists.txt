cmake_minimum_required(VERSION 3.8)
project(optitrack_core)

if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(NatNetLib ${CMAKE_CURRENT_SOURCE_DIR}/../third_party/NatNet_SDK_4.1_ubuntu)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(optitrack_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)

include_directories(
  include
  ${NatNetLib}/include
)

link_directories(${NatNetLib}/lib)

add_executable(${PROJECT_NAME}
  src/main.cpp
)

install(TARGETS 
  ${PROJECT_NAME}
  DESTINATION lib/${PROJECT_NAME})

ament_target_dependencies(${PROJECT_NAME} rclcpp optitrack_msgs)


# target_include_directories(${PROJECT_NAME} PUBLIC
#   $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
#   $<INSTALL_INTERFACE:include>)

target_compile_features(${PROJECT_NAME} PUBLIC c_std_99 cxx_std_17)  # Require C99 and C++17


target_link_libraries(${PROJECT_NAME}
  libNatNet.so
)


if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
