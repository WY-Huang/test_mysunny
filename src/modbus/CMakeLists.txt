cmake_minimum_required(VERSION 3.6)
project(modbus)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(PkgConfig REQUIRED)
pkg_check_modules(MODBUS REQUIRED IMPORTED_TARGET libmodbus)

add_library(modbus SHARED src/modbus.cpp 
                          src/jsoncpp.cpp 
                          src/jsonfuction.cpp 
                          src/TCPClient.cpp 
                          src/TCPServer.cpp 
                          src/TCPServer2.cpp
                          src/FileOut.cpp 
                          src/E2proomData.cpp
                          src/E2proomData_als100.cpp
                          src/E2proomData_als101.cpp
                          src/E2proomData_als102.cpp
                          src/E2proomData_als103.cpp
                          src/E2proomData_als104.cpp
                          src/E2proomData_als105.cpp
                          src/E2proomData_als106.cpp
                          src/E2proomData_als108.cpp
                          src/modbus_als100.cpp
                          src/modbus_als101.cpp
                          src/modbus_als102.cpp
                          src/modbus_als103.cpp
                          src/modbus_als104.cpp
                          src/modbus_als105.cpp
                          src/modbus_als106.cpp
                          src/modbus_als108.cpp)

target_include_directories(modbus PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> $<INSTALL_INTERFACE:include>)

ament_target_dependencies(modbus rclcpp rclcpp_components)

target_link_libraries(modbus PkgConfig::MODBUS)

rclcpp_components_register_node(modbus PLUGIN "modbus::Modbus" EXECUTABLE modbus_node)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
  add_subdirectory(test)
endif()

install(TARGETS modbus EXPORT export_modbus DESTINATION lib)
install(DIRECTORY include/ DESTINATION include)
install(DIRECTORY launch DESTINATION share/${PROJECT_NAME})
install(DIRECTORY config DESTINATION share/${PROJECT_NAME})

ament_export_targets(export_modbus HAS_LIBRARY_TARGET)

ament_package()
