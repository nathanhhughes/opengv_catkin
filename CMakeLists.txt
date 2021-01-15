cmake_minimum_required(VERSION 2.8.3)
project(opengv_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(opengv_src
  GIT_REPOSITORY https://github.com/laurentkneip/opengv.git
  GIT_TAG master
#  GIT_PROGRESS 1
  UPDATE_COMMAND ""
  #PATCH_COMMAND patch -p1 < ${CMAKE_SOURCE_DIR}/use_catkinized_eigen.patch
  CMAKE_ARGS -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
  # Change the path to GTSAM's eigen to your own! ideally use gtsam_catkin...
  # -DEIGEN_INCLUDE_DIR=${CATKIN_DEVEL_PREFIX}/../src/eigen_catkin/include
  # -DEIGEN_INCLUDE_DIRS=${CATKIN_DEVEL_PREFIX}/../src/eigen_catkin/include
)

install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/opengv
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})
cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
          LIBRARIES opengv)
