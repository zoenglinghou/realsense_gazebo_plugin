cmake_minimum_required(VERSION 2.8.3)
project(realsense_gazebo_plugin)

# Ignore packages in this directory if gazebo is not present
find_package(gazebo_ros QUIET)
if (gazebo_ros_FOUND)
    # Load catkin and all dependencies required for this package
    find_package(catkin REQUIRED COMPONENTS
            roscpp
            gazebo_dev
            image_transport
            camera_info_manager
            sensor_msgs
            )

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${GAZEBO_CXX_FLAGS}")

    catkin_package(
            DEPENDS
            roscpp
            gazebo_ros
            gazebo_dev
            image_transport
            camera_info_manager
            sensor_msgs
    )

    include_directories(
            include)
    include_directories(
            SYSTEM
            ${catkin_INCLUDE_DIRS}
            ${GAZEBO_INCLUDE_DIRS}
    )
    link_directories(${GAZEBO_LIBRARY_DIRS})

    add_library(
            ${PROJECT_NAME}
            src/RealSensePlugin.cpp
            src/gazebo_ros_realsense.cpp
    )

    target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${GAZEBO_LIBRARIES})
    add_dependencies(${PROJECT_NAME} ${catkin_EXPORTED_TARGETS})

    #install(
    #        TARGETS
    #        ${PROJECT_NAME}
    #        DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
    #        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    #)
    #
    #foreach (dir include)
    #    install(DIRECTORY ${dir}/
    #            DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
    #endforeach (dir)
else ()
    # Dummy Install
    install(FILES README.md
            DESTINATION /tmp)
endif ()