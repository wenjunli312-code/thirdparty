cmake_minimum_required(VERSION 3.16)
project(thirdparty VERSION 1.0.0)

# thirdparty acts as a vendor directory: each subdirectory is a third-party library.
# Downstream projects use:
#   list(APPEND CMAKE_PREFIX_PATH "${thirdparty_SOURCE_DIR}")
#   find_package(nlohmann_json REQUIRED)
#   target_link_libraries(my_target PRIVATE nlohmann_json::nlohmann_json)
#   find_package(Eigen3 REQUIRED)
#   target_link_libraries(my_target PRIVATE Eigen3::Eigen)

# Provide imported targets for header-only libraries

# nlohmann_json (multi-header, installed at include/nlohmann/)
add_library(nlohmann_json::nlohmann_json INTERFACE)
add_library(nlohmann_json INTERFACE ALIAS nlohmann_json::nlohmann_json)
target_include_directories(nlohmann_json::nlohmann_json INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/nlohmann_json/include>
    $<INSTALL_INTERFACE:include>
)
target_compile_definitions(nlohmann_json::nlohmann_json INTERFACE
    JSON_NO_EXCEPTION=1
)

# Eigen3 (header-only)
# The CMake config from eigen itself uses Eigen3::Eigen
add_library(Eigen3::Eigen INTERFACE)
target_include_directories(Eigen3::Eigen INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/eigen>
    $<INSTALL_INTERFACE:include/eigen>
)