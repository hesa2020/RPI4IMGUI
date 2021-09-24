cmake_minimum_required(VERSION 3.3)
project(GLFWIMGUIIMPLOT)

# stb does not have a CMake build, but is header-only
find_path(STB_INCLUDE_DIRS "stb.h") 

## Create main executable
add_executable(main src/main.cpp)
target_include_directories(main 
  PRIVATE 
    ${STB_INCLUDE_DIRS}
    "${CMAKE_CURRENT_LIST_DIR}/src"
)

target_link_libraries(main PRIVATE)