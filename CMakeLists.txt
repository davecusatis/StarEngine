cmake_minimum_required(VERSION 3.2)
project(StarEngine)

set(CMAKE_EXECUTABLE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
#set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set(EX_PROJ_SOURCE_DIR ${CMAKE_BINARY_DIR}/deps/Source)
set(EX_PROJ_BUILD_DIR ${CMAKE_BINARY_DIR}/deps/Build)

find_package(OpenGL REQUIRED) 
include_directories(${OPENGL_INCLUDE_DIR})

configure_file(deps-CMakeLists.txt deps/CMakeLists.txt COPYONLY)
execute_process(COMMAND ${CMAKE_COMMAND} . WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/deps)
execute_process(COMMAND ${CMAKE_COMMAND} --build . WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/deps)

include_directories(${EX_PROJ_SOURCE_DIR}/GLM_EX/glm
                    ${EX_PROJ_SOURCE_DIR}/GLFW_EX/include
                    ${EX_PROJ_SOURCE_DIR}/GLEW_EX/include)

add_subdirectory(${CMAKE_BINARY_DIR}/../src)
set(ABSOLUTE_SRC_FILES "")
foreach(filen ${SRC_FILES})
	list(APPEND ABSOLUTE_SRC_FILES "src/${filen}")
endforeach(filen)

add_executable(StarEngine ${ABSOLUTE_SRC_FILES})
#add_library(StarEngine ${ABSOLUTE_SRC_FILES})

add_definitions(-DGLEW_STATIC)
target_link_libraries(StarEngine ${OPENGL_LIBRARIES})
target_link_libraries(StarEngine debug ${EX_PROJ_BUILD_DIR}/GLFW_EX/src/Debug/glfw3${CMAKE_STATIC_LIBRARY_SUFFIX})
target_link_libraries(StarEngine debug ${EX_PROJ_BUILD_DIR}/GLEW_EX/lib/Debug/glewd${CMAKE_STATIC_LIBRARY_SUFFIX})
target_link_libraries(StarEngine optimized ${EX_PROJ_BUILD_DIR}/GLFW_EX/src/Release/glfw3${CMAKE_STATIC_LIBRARY_SUFFIX})
target_link_libraries(StarEngine optimized ${EX_PROJ_BUILD_DIR}/GLEW_EX/lib/Release/glew${CMAKE_STATIC_LIBRARY_SUFFIX})
