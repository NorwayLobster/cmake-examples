cmake_minimum_required(VERSION 3.6)
project (hello_world)

#static library
add_library(HelloWorld STATIC src/HelloWorld.cc)
add_library(hello::library ALIAS HelloWorld)

target_include_directories(HelloWorld 
	PUBLIC
	${PROJECT_SOURCE_DIR}/include/
)



#executable
add_executable(${PROJECT_NAME} src/main.cc)

target_link_libraries(${PROJECT_NAME}
	PRIVATE
	hello::library	
#   HelloWorld
)
