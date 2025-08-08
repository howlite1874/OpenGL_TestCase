cmake_minimum_required(VERSION 3.1)
project(opengl_cmake_skeleton)

set(CMAKE_CXX_STANDARD 11)
add_definitions(-DGLEW_STATIC)

add_subdirectory(lib/glfw EXCLUDE_FROM_ALL)
add_subdirectory(lib/glew EXCLUDE_FROM_ALL)
add_subdirectory(lib/glm EXCLUDE_FROM_ALL)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/src
)

set(COMMON_SOURCES
    src/Application.cpp
    src/glError.cpp
    src/Shader.cpp
)

configure_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/src/asset.hpp.in
  ${CMAKE_CURRENT_BINARY_DIR}/src/asset.hpp
)

function(add_testcase name)
    add_executable(${name}
        ${COMMON_SOURCES}
        tests/${name}/MyApplication.cpp
    )
    target_include_directories(${name}
        PRIVATE tests/${name}
        PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src
        PRIVATE ${CMAKE_CURRENT_BINARY_DIR}/src
    )
    target_link_libraries(${name}
        PRIVATE glfw
        PRIVATE libglew_static
        PRIVATE glm
    )
endfunction()

add_testcase(original)
