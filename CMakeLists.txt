cmake_minimum_required(VERSION 3.21)

project(SendOpencvToQmlQthread LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

set(QT_MAJOR_VERSION 6)

find_package(OpenCV REQUIRED)
include_directories( ${OpenCV_INCLUDE_DIRS} )
find_package(Qt6 REQUIRED COMPONENTS Core Quick Qml Multimedia Widgets)
include_directories(${Qt6Widgets_INCLUDE_DIRS})
include_directories(${Qt6Quick_INCLUDE_DIRS})
include_directories(${_INCLUDE_DIRS})

add_library(opencvimageprovider 
    opencvimageprovider.cpp)
target_link_libraries(opencvimageprovider
Qt6::Core
Qt6::Gui
Qt6::Quick
Qt6::Widgets)
add_library(videostreamer 
    videostreamer.cpp)

qt_add_executable(AppSendOpencvToQmlQthread
    main.cpp
)

target_link_libraries(AppSendOpencvToQmlQthread PUBLIC
    Qt6::Core
    Qt6::Gui
    Qt6::Quick
    Qt6::Widgets
    ${OpenCV_LIBS}
    opencvimageprovider
    videostreamer
)

qt_add_qml_module(AppSendOpencvToQmlQthread
    URI SendOpencvToQmlQthread
    VERSION 1.0
    QML_FILES main.qml
)

install(TARGETS AppSendOpencvToQmlQthread
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})