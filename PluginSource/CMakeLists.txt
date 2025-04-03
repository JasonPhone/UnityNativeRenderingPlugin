cmake_minimum_required(VERSION 3.20.0)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin/)

set(CMAKE_VERBOSE_MAKEFILE ON)
add_compile_options("$<$<C_COMPILER_ID:MSVC>:/utf-8>")
add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/utf-8>")

set(CMAKE_BUILD_TYPE Debug)
message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")

project(RenderingPlugin)

set(PROJ_INCLUDE_LIST
    ${CMAKE_SOURCE_DIR}/source
)
set(PROJ_LINK_LIST
)
set(PROJ_SRC_LIST
    ${CMAKE_SOURCE_DIR}/source/RenderAPI.cpp
    ${CMAKE_SOURCE_DIR}/source/RenderAPI_Vulkan.cpp
    ${CMAKE_SOURCE_DIR}/source/RenderingPlugin.cpp
)

set(ENV{VULKAN_SDK} "C:/VulkanSDK/1.4.304.1")
find_package(Vulkan REQUIRED)
list(APPEND PROJ_INCLUDE_LIST
    ${Vulkan_INCLUDE_DIRS}
)
# list(APPEND PROJ_LINK_LIST
#     "${Vulkan_LIBRARIES}"
# )

# NPU part.
# set(OV_DIR ${CMAKE_SOURCE_DIR}/extern/openvino_20246)
# find_package(OpenVINO REQUIRED COMPONENTS Runtime HINTS ${OV_DIR}/runtime/cmake)
# list(APPEND PROJ_INCLUDE_LIST ${OV_DIR}/runtime/include)
# list(APPEND PROJ_LINK_LIST NPULib)

add_library(${PROJECT_NAME} SHARED
    ${PROJ_SRC_LIST}
)

target_include_directories(${PROJECT_NAME} PRIVATE
    ${PROJ_INCLUDE_LIST}
)

target_link_libraries(${PROJECT_NAME} PRIVATE
    ${PROJ_LINK_LIST}
)

install(TARGETS RenderingPlugin
    DESTINATION "C:/Users/zs_fe/Documents/code/UnityNativeRenderingPlugin/UnityProject/Assets/Plugins/"
    # ARCHIVE DESTINATION lib
    # RUNTIME DESTINATION bin
    # PUBLIC_HEADER DESTINATION include
)