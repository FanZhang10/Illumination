cmake_minimum_required(VERSION 2.6)
 
# Name of project
project(IlluminationDemo)

# Specify project files: header files and source files
set(HDRS
	./ogre_application.h
)
 
set(SRCS
	./ogre_application.cpp ./main.cpp ./ShinyMaterialVp.glsl ./ShinyMaterialFp.glsl ./ShinyBlueMaterialVp.glsl ./ShinyBlueMaterialFp.glsl ./ShinyBlue.material ./Shiny.material
)

# The rules here are specific to Windows Systems
if(WIN32)
    # Get Ogre directory from the environment variable
    set(OGRE_HOME $ENV{OGRE_HOME})

    # Set up Ogre include directories
    include_directories(
        "${OGRE_HOME}/include"
        "${OGRE_HOME}/include/OIS"
        "${OGRE_HOME}/include/OGRE"
        "${OGRE_HOME}/boost"
    )

    # Set up Ogre library directories
    link_directories(
        "${OGRE_HOME}/lib/"
        "${OGRE_HOME}/boost/lib"
    )

    # Applications using Ogre need to be run from inside the Ogre
    # directory. This cmake command needs to come before add_executable
    set(EXECUTABLE_OUTPUT_PATH "${OGRE_HOME}/bin/")

    # Add path name
    configure_file(path_config.h.in path_config.h)

    # Add executable based on the source files
    add_executable(IlluminationDemo ${HDRS} ${SRCS})

    # Set up names of Ogre libraries
    target_link_libraries(IlluminationDemo
        "OgreMain_d.lib"
        "OIS_d.lib"
        "OgreOverlay_d.lib"
    )

    # Avoid ZERO_CHECK target 
    set(CMAKE_SUPPRESS_REGENERATION TRUE)
 
    # This will use the proper libraries in debug mode
    set_target_properties(IlluminationDemo PROPERTIES DEBUG_POSTFIX _d)
endif(WIN32)
