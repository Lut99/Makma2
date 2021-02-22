cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Makma2 VERSION 1.0.0 DESCRIPTION "Vulkan-based 2D game engine, interfaceable as library")

# Be sure that both Vulkan & GLFW are installed
find_package(Vulkan REQUIRED)
find_package(glfw3 3.3 REQUIRED)
find_package(glm REQUIRED)

# Also include the debugger library
find_package(CppDebugger REQUIRED)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Define all include directories
get_target_property(GLFW_DIR glfw INTERFACE_INCLUDE_DIRECTORIES)
SET(INCLUDE_DIRS "${PROJECT_SOURCE_DIR}/src/lib" "" "${Vulkan_INCLUDE_DIRS}" "${GLFW_DIR}" "${GLM_INCLUDE_DIRS}")

# Load the libraries
add_subdirectory(src)



##### HELLOVIKINGROOM TARGET #####
# Make sure ndebug is appended
if (NOT CMAKE_BUILD_TYPE MATCHES Debug)
    add_definitions(-DNDEBUG)
endif()
# Specify which file will compile to the executable
add_executable(makma2 ${PROJECT_SOURCE_DIR}/src/Makma2.cpp)
# Set the output to bin directory
set_target_properties(makma2
                      PROPERTIES 
                      RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/bin
                      RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/bin
                      )

# Add the generated hpp's to the include directories, plus the library directory
target_include_directories(makma2 PUBLIC "${INCLUDE_DIRS}")

# Add which libraries to link
target_link_libraries(makma2 PUBLIC
                      ${EXTRA_LIBS}
                      ${Vulkan_LIBRARIES}
                      ${GLM_LIBRARIES}
                      )

# Also add our debugging library
target_link_libraries(makma2 PUBLIC debug cppdbg)



##### BUILDING SHADERS #####
# Define the custom commands to compile the shaders
# add_custom_command(TARGET makma2 POST_BUILD
#     COMMAND glslc -o ${PROJECT_SOURCE_DIR}/bin/vert.spv ${PROJECT_SOURCE_DIR}/src/lib/Shaders/shader.vert
#     COMMAND glslc -o ${PROJECT_SOURCE_DIR}/bin/frag.spv ${PROJECT_SOURCE_DIR}/src/lib/Shaders/shader.frag
#     COMMENT "Building shaders..."
# )



##### TARGET FOR TESTS #####
# Nothing as of yet
