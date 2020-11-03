cmake_minimum_required(VERSION 3.6)
# Set the project name
project (hello_world)

# Set a default C++ compile flag
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DEX2" CACHE STRING "Set C++ Compiler Flags" FORCE)

# try conditional compilation
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)

# check results and add flag
if(COMPILER_SUPPORTS_CXX11)#
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)#
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

if( CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT )
  message(STATUS "Setting default CMAKE_INSTALL_PREFIX path to ${CMAKE_BINARY_DIR}/install")
  set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install" CACHE STRING "The path to use for make install" FORCE)
endif()

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message("Setting build type to 'RelWithDebInfo' as none was specified.")
  set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build." FORCE)
  #  set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release"
    "MinSizeRel" "RelWithDebInfo")
endif()










# find a boost install with the libraries filesystem and system
find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system)

# check if boost was found
if(Boost_FOUND)
    message ("boost found")
else()
    message (FATAL_ERROR "Cannot find Boost")
endif()



#create a dynamic library
add_library(HelloWorld SHARED src/HelloWorld.cc)
# add an alias for it 
add_library(hello::library ALIAS HelloWorld)

target_include_directories(HelloWorld 
	PUBLIC
	${PROJECT_SOURCE_DIR}/include
	#	${PROJECT_SOURCE_DIR}/include/   #note here
)



# create an executable
add_executable(${PROJECT_NAME} src/main.cc)

target_compile_definitions(${PROJECT_NAME}
    PRIVATE EX3
)

target_link_libraries(${PROJECT_NAME}
	PRIVATE
	hello::library	
#   HelloWorld
)

target_link_libraries(${PROJECT_NAME}
	PRIVATE
	Boost::filesystem
)

#install

install(TARGETS hello_world DESTINATION bin)
install(TARGETS HelloWorld LIBRARY DESTINATION lib)
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ DESTINATION include)
#install(DIRECTORY ${PROJECT_SOURCE_DIR}/include DESTINATION include) #is different
install(FILES sample.conf DESTINATION etc)
