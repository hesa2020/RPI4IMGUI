cmake_minimum_required(VERSION 3.3)
project(main)

#OpenGL
find_package(OpenGL REQUIRED)

#UDEV
IF(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    set(IOKIT_LIBRARY "-framework IOKit -framework CoreFoundation" CACHE FILEPATH "IOKit framework" FORCE)
else()
    find_library(UDEV NAMES libudev.a)
    IF(UDEV)
        message(STATUS ${UDEV})
    else()
        find_library(UDEV NAMES libudev.so)
        IF(UDEV)
            message(STATUS ${UDEV})
        else()
            MESSAGE(FATAL_ERROR "LIB UDEV not found on your system, see install.txt how to get them installed.\nsudo apt-get install libudev-dev")
        ENDIF(UDEV)
    ENDIF(UDEV)
ENDIF()

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
    $<BUILD_INTERFACE:${imgui_root}>/backends/imgui_impl_opengl3.cpp
)
target_include_directories(imgui INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    $<BUILD_INTERFACE:${imgui_root}>
    #$<BUILD_INTERFACE:${imgui_root}/misc/freetype>
    $<BUILD_INTERFACE:${imgui_root}/backends>
)
target_compile_definitions(imgui INTERFACE IMGUI_DISABLE_INCLUDE_IMCONFIG_H)

## Create main executable
add_executable(${PROJECT_NAME} src/main.cpp src/LTimer.cpp)

target_include_directories(main 
  PRIVATE
    ${OPENGL_INCLUDE_DIR}
    "${CMAKE_CURRENT_LIST_DIR}/src"
)

target_link_libraries(${PROJECT_NAME} PRIVATE ${OPENGL_gl_LIBRARY} ${UDEV} SDL2::SDL2 SDL2::SDL2main imgui)
