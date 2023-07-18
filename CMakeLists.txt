cmake_minimum_required(VERSION 2.8)
project(gtsam_catkin)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake")
find_package(catkin REQUIRED)

set(gtsam_PREFIX "${CMAKE_CURRENT_BINARY_DIR}/gtsam_src-prefix")
set(gtsam_SOURCE_DIR "${gtsam_PREFIX}/src/gtsam_src")
execute_process(COMMAND ${CMAKE_COMMAND} -E make_directory ${gtsam_PREFIX}/src)

include(ExternalProject)
externalproject_add(gtsam_src
  GIT_REPOSITORY git@github.com:borglab/gtsam.git
  GIT_TAG 4.0.0
  CONFIGURE_COMMAND cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} -DGTSAM_TYPEDEF_POINTS_TO_VECTORS=ON -DGTSAM_BUILD_PYTHON=ON -DGTSAM_ALLOW_DEPRECATED_SINCE_V4=OFF -DGTSAM_BUILD_WRAP=OFF ../gtsam_src
  PATCH_COMMAND patch -p1 -d ${gtsam_SOURCE_DIR} -t -N < ${CMAKE_CURRENT_SOURCE_DIR}/4.0.0.diff
  BUILD_COMMAND make -j4
  INSTALL_COMMAND make install
)

catkin_package(
  CFG_EXTRAS gtsam-extras.cmake)

install(FILES package.xml DESTINATION share/${PROJECT_NAME})
