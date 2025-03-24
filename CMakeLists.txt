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
  GIT_REPOSITORY git@github.com:borglab/gtsam.git
  GIT_TAG b10963802c13893611d5a88894879bed47adf9e0
  CONFIGURE_COMMAND cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} -DGTSAM_TYPEDEF_POINTS_TO_VECTORS=ON -DGTSAM_BUILD_PYTHON=ON -DGTSAM_ALLOW_DEPRECATED_SINCE_V4=OFF -DGTSAM_BUILD_WRAP=OFF -DGTSAM_BUILD_TESTS=OFF ../gtsam_src
  BUILD_COMMAND make -j4
  INSTALL_COMMAND make install
)

catkin_package(
  CFG_EXTRAS gtsam-extras.cmake)

install(FILES package.xml DESTINATION share/${PROJECT_NAME})
