Catkin wrapper package for GTSAM. Clone to your dry workspace and enjoy. Wraps its own Eigen.

Include in your project using

`find_package(catkin REQUIRED COMPONENTS
...
gtsam_catkin
)`

`include_directories(
...
${gtsam_catkin_INCLUDE_DIRS}
)`

`target_link_libraries(... ${gtsam_catkin_LIBRARIES})`

Note that for setting the Point typedefs to Eigen data members, I have defaulted to not wrapping Matlab wrappers.