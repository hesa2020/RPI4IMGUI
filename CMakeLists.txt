cmake_minimum_required(VERSION 3.3)
project(GLFWIMGUIIMPLOT)

#OpenGL
find_package(OpenGL REQUIRED)

#GLFW
#For my windows i needed to manually set to "C:/Program Files (x86)/GLFW/lib/cmake/glfw3" in the cmake GUI
find_package(glfw3 3.3.4 REQUIRED)

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
    $<BUILD_INTERFACE:${imgui_root}>/backends/imgui_impl_glfw.cpp
    $<BUILD_INTERFACE:${imgui_root}>/backends/imgui_impl_opengl2.cpp
    #$<$<PLATFORM_ID:Windows>:$<BUILD_INTERFACE:${imgui_root}>/backends/imgui_impl_win32.cpp> #Im not sure if we even need this with GLFW
)
target_include_directories(imgui INTERFACE
    $<BUILD_INTERFACE:${imgui_root}>
    #$<BUILD_INTERFACE:${imgui_root}/misc/freetype>
    $<BUILD_INTERFACE:${imgui_root}/backends>
)
target_compile_definitions(imgui INTERFACE IMGUI_DISABLE_INCLUDE_IMCONFIG_H)

#implot
add_library(implot INTERFACE)
set(implot_root ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/implot)
target_sources(implot INTERFACE
    $<BUILD_INTERFACE:${implot_root}>/implot.cpp
    $<BUILD_INTERFACE:${implot_root}>/implot_demo.cpp
    $<BUILD_INTERFACE:${implot_root}>/implot_items.cpp
)
target_include_directories(implot INTERFACE
    $<BUILD_INTERFACE:${implot_root}>
)
target_compile_definitions(implot INTERFACE)

## Create main executable
add_executable(main src/main.cpp)

target_include_directories(main 
  PRIVATE
    ${OPENGL_INCLUDE_DIR}
    "${CMAKE_CURRENT_LIST_DIR}/src"
)

target_link_libraries(main PRIVATE ${OPENGL_gl_LIBRARY} glfw imgui implot)
