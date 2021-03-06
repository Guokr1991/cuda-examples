find_package(OpenGL REQUIRED)

include(FindPkgConfig)

add_library(gears gears.c)
target_link_libraries(gears ${OPENGL_LIBRARIES} m)
set_target_properties(gears PROPERTIES COMPILE_FLAGS "-std=c99")

find_package(GLUT)
if(GLUT_FOUND)
    include_directories(${OPENGL_INCLUDE_DIR} ${GLUT_INCLUDE_DIR})
    add_executable(gears_GLUT gears_GLUT.cpp)
    target_link_libraries(gears_GLUT ${GLUT_LIBRARIES} gears ${OPENGL_LIBRARIES} m)
endif(GLUT_FOUND)

pkg_search_module(SDL2 sdl2)
if(SDL2_FOUND)
    include_directories(${SDL2_INCLUDE_DIRS})
    add_executable(gears_SDL gears_SDL.c)
    target_link_libraries(gears_SDL ${SDL2_LIBRARIES} gears)
endif(SDL2_FOUND)

find_package(Qt5Widgets)
if(Qt5Widgets_FOUND)
    find_package(Qt5OpenGL REQUIRED)
    set(CMAKE_AUTOMOC TRUE)
    include_directories(${Qt5Widgets_INCLUDE_DIRS} ${Qt5OpenGL_INCLUDE_DIRS})
    add_executable(gears_Qt ${FILES_MOC} gears_Qt.cpp mainwindow.cpp glwidget.cpp)
    target_link_libraries(gears_Qt Qt5::Widgets Qt5::OpenGL gears)
    set_target_properties(gears_Qt PROPERTIES COMPILE_FLAGS "-std=c++11")
endif(Qt5Widgets_FOUND)
