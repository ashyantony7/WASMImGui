cmake_minimum_required(VERSION 3.15)
project(wasm-imgui)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

add_executable(${PROJECT_NAME} ${SRC_DIR}/main.cpp
        src/bindings/imgui_impl_glfw.cpp
        src/bindings/imgui_impl_opengl3.cpp
        src/configs.h
)

set(CMAKE_EXECUTABLE_SUFFIX ".html")
set_target_properties(${PROJECT_NAME} PROPERTIES
        LINK_FLAGS "-s DISABLE_EXCEPTION_CATCHING=1 -s USE_GLFW=3 -s FULL_ES3=1 -s WASM=1 -s ALLOW_MEMORY_GROWTH=1 -s NO_EXIT_RUNTIME=0 -s ASSERTIONS=1")

target_include_directories(${PROJECT_NAME}
        PRIVATE
        ${CMAKE_SOURCE_DIR}
        )

target_link_libraries(${PROJECT_NAME}
        PRIVATE
        ${CONAN_LIBS}
        )
