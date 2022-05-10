cmake_minimum_required(VERSION 3.3)
project(RPI4IMGUISDL2)

#OpenGL
find_package(OpenGL REQUIRED)

find_package(SDL2 REQUIRED)
include_directories(${SDL2_INCLUDE_DIRS})

# imgui
add_library(imgui INTERFACE)
set(imgui_root ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/imgui)

target_sources(imgui INTERFACE
    $<BUILD_INTERFACE:${imgui_root}>/imgui.cpp
    $<BUILD_INTERFACE:${imgui_root}>/imgui_demo.cpp
    $<BUILD_INTERFACE:${imgui_root}>/imgui_draw.cpp
    $<BUILD_INTERFACE:${imgui_root}>/imgui_tables.cpp
    $<BUILD_INTERFACE:${imgui_root}>/imgui_widgets.cpp
    #$<BUILD_INTERFACE:${imgui_root}>/misc/freetype/imgui_freetype.cpp
    $<BUILD_INTERFACE:${imgui_root}>/backends/imgui_impl_sdl.cpp
    $<BUILD_INTERFACE:${imgui_root}>/backends/imgui_impl_opengl2.cpp
)
target_include_directories(imgui INTERFACE
    $<BUILD_INTERFACE:${imgui_root}>
    #$<BUILD_INTERFACE:${imgui_root}/misc/freetype>
    $<BUILD_INTERFACE:${imgui_root}/backends>
)
target_compile_definitions(imgui INTERFACE IMGUI_DISABLE_INCLUDE_IMCONFIG_H)

## Create main executable
add_executable(main src/main.cpp)

target_include_directories(main 
  PRIVATE
    ${OPENGL_INCLUDE_DIR}
    "${CMAKE_CURRENT_LIST_DIR}/src"
)

target_link_libraries(main PRIVATE ${OPENGL_gl_LIBRARY} ${SDL2_LIBRARIES} imgui)
