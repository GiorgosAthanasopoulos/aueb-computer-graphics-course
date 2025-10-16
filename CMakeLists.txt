cmake_minimum_required(VERSION 3.11)
project(AuebComputerGraphicsCourse LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# GLFW
find_package(glfw3 3.3 REQUIRED)

# GLAD
add_library(glad STATIC lib/glad/glad.c)
target_include_directories(glad PUBLIC lib)

# GLM
include(FetchContent)
FetchContent_Declare(
	glm
	GIT_REPOSITORY	https://github.com/g-truc/glm.git
	GIT_TAG 	0af55ccecd98d4e5a8d1fad7de25ba429d60e863 #refs/tags/1.0.1
)
FetchContent_MakeAvailable(glm)

# ImGui
file(GLOB IMGUI_SRC
    lib/imgui/*.cpp
    lib/imgui/backends/imgui_impl_glfw.cpp
    lib/imgui/backends/imgui_impl_opengl3.cpp
)
add_library(imgui STATIC ${IMGUI_SRC})
target_include_directories(imgui PUBLIC lib/imgui)

# Our source
add_executable(AuebComputerGraphicsCourse src/main.cpp)

target_include_directories(AuebComputerGraphicsCourse PRIVATE ${CMAKES_SOURCE_DIR}/include/stb)
target_include_directories(AuebComputerGraphicsCourse PRIVATE ${CMAKES_SOURCE_DIR}/include/tinyobjloader)

# Link everything
target_link_libraries(AuebComputerGraphicsCourse PRIVATE glad glfw glm::glm imgui)
target_include_directories(AuebComputerGraphicsCourse PRIVATE include)

if(UNIX)
    target_link_libraries(AuebComputerGraphicsCourse PRIVATE dl GL)
endif()
