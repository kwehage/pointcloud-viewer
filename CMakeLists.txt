cmake_minimum_required(VERSION 2.8.11)
project(pointcloud-viewer)
set(VERSION_MAJOR 0 CACHE STRING "Project major version number.")
set(VERSION_MINOR 0 CACHE STRING "Project minor version number.")
set(VERSION_PATCH 1 CACHE STRING "Project patch version number.")

find_package(Qt5Core REQUIRED)
find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets CONFIG REQUIRED)
find_package(OpenCV REQUIRED)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

set(pointcloud_viewer_src
  main.cc
  qtviewer.cc 
  openglwindow.cc
  renderwindow.cc
  tiny_obj_loader.cc
  resources.qrc
)

add_executable(
  viewer
  ${pointcloud_viewer_src}
)

target_include_directories(
  viewer PRIVATE
  /usr/local/include/libfreenect
  /usr/include/opencv4
  /usr/local/lib
)

target_link_directories(
  viewer PRIVATE 
  /usr/local/lib
)

message(STATUS ${OpenCV_LIBS})

target_link_libraries(
  viewer PRIVATE
  freenect
  ${OpenCV_LIBS}
)

qt5_use_modules(viewer Core Widgets Gui)

target_compile_options(
  viewer
  PRIVATE
  -march=x86-64
  -mtune=generic
  -fstack-protector-strong
  --param=ssp-buffer-size=4
  -O3
  -std=c++11
  -lstdc++
  -W
)
