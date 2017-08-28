cmake_minimum_required(VERSION 2.8)

project(oscc-joystick-commander)

find_package(PkgConfig REQUIRED)
pkg_check_modules(SDL2 REQUIRED sdl2)

if(KIA_SOUL)
    add_definitions(-DKIA_SOUL)
elseif(KIA_SOUL_EV)
    add_definitions(-DKIA_SOUL_EV)
endif()

add_executable(
    oscc-joystick-commander
    src/commander.c
    src/joystick.c
    src/main.c
    oscc/api/src/oscc.c)

target_include_directories(
    oscc-joystick-commander
    PRIVATE
    include
    oscc/api/include
    ${SDL2_INCLUDE_DIRS})

target_link_libraries(
    oscc-joystick-commander
    PRIVATE
    ${SDL2_LIBRARIES})

