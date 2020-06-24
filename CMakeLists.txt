cmake_minimum_required(VERSION 2.8)
project(gtsam_catkin)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_LIST_DIR}/cmake")
find_package(catkin REQUIRED)

set(EIGEN3_INCLUDE_DIR "$ENV{EIGEN3_INCLUDE_DIR}")
if(NOT EIGEN3_INCLUDE_DIR)
  message(
    FATAL_ERROR
      "Please point the environment variable EIGEN3_INCLUDE_DIR to the include directory of your Eigen3 installation."
  )
endif()

include(ExternalProject)
externalproject_add(gtsam_src
  GIT_REPOSITORY https://github.com/borglab/gtsam.git
  GIT_TAG 4.0.2
  CONFIGURE_COMMAND cmake -DEIGEN3_INCLUDE_DIR=${EIGEN3_INCLUDE_DIR} -DGTSAM_USE_SYSTEM_EIGEN=ON -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} -DGTSAM_ALLOW_DEPRECATED_SINCE_V4=OFF -DGTSAM_BUILD_WRAP=OFF -DGTSAM_BUILD_TESTS=OFF -DGTSAM_BUILD_TIMING=OFF -DGTSAM_INSTALL_CYTHON_TOOLBOX=OFF -DGTSAM_BUILD_EXAMPLES_ALWAYS=OFF -DGTSAM_BUILD_UNSTABLE=ON -DGTSAM_BUILD_PYTHON=OFF ../gtsam_src
  BUILD_COMMAND make
  INSTALL_COMMAND make install
)

catkin_package(
  CFG_EXTRAS gtsam-extras.cmake)

install(FILES package.xml DESTINATION share/${PROJECT_NAME})
