project(subbinary)

# Create the executable
add_executable(${PROJECT_NAME} main.cpp)

# Link the static library from subproject1 using it's alias sub::lib1
# Link the header only library from subproject2 using it's alias sub::lib2
# This will cause the include directories for that target to be added to this project

#In this example because the subbinary executable links the sublibrary1 and sublibrary2 libraries it will automatically include the ${sublibrary1_SOURCE_DIR}/inc and ${sublibrary2_SOURCE_DIR}/inc folders as they are exported with the PUBLIC and INTERFACE scopes of the libraries.
target_link_libraries(${PROJECT_NAME}
    sub::lib1
    sub::lib2
)
