cmake_minimum_required(VERSION 2.8)
project(gtsam_catkin)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake")
find_package(catkin REQUIRED)

include(ExternalProject)
externalproject_add(gtsam_src
  GIT_REPOSITORY https://bitbucket.org/gtborg/gtsam.git
  GIT_TAG 4.0.0-alpha2
  CONFIGURE_COMMAND cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} -DGTSAM_TYPEDEF_POINTS_TO_VECTORS=ON -DGTSAM_BUILD_PYTHON=ON -DGTSAM_ALLOW_DEPRECATED_SINCE_V4=OFF -DGTSAM_BUILD_WRAP=OFF -DGTSAM_BUILD_TESTS=OFF -DGTSAM_BUILD_TIMING=OFF -DGTSAM_BUILD_EXAMPLES_ALWAYS=OFF ../gtsam_src
  BUILD_COMMAND make
  INSTALL_COMMAND make install
)

catkin_package(
  CFG_EXTRAS gtsam-extras.cmake)

install(FILES package.xml DESTINATION share/${PROJECT_NAME})
