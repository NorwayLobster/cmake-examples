# Set the minimum version of CMake that can be used
# To find the cmake version run
# $ cmake --version
cmake_minimum_required(VERSION 3.5)

# Set the project name
project (hello_headers)

# Create a sources variable with a link to all cpp files to compile

#v1
set(SOURCES
    src/Hello.cpp
    src/main.cpp
)
add_executable(${PROJECT_NAME} ${SOURCES})
#add_executable(hello_headers ${SOURCES})

#v2
#file(GLOB SOURCES "src/*.cpp")
#add_executable(${PROJECT_NAME} ${SOURCES})

#v3
# Add an executable with the above sources
#add_executable(${PROJECT_NAME} src/Hello.cpp src/main.cpp)


#add_executable(${PROJECT_NAME} src/*.cpp) wrong
#wrong
#add_executable(${PROJECT_NAME} src/main.cpp)
#add_executable(${PROJECT_NAME} src/Hello.cpp)


# Set the directories that should be included in the build command for this target
# when running g++ these will be included as -I/directory/path/
#target_include_directories(hello_headers
target_include_directories(${PROJECT_NAME}
    PRIVATE 
        ${PROJECT_SOURCE_DIR}/include
)
