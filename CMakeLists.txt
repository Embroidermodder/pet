cmake_minimum_required(VERSION 3.16)

# cmake
# build-essential
# arm-none-eabi-gcc

project(embroidermodder_pet VERSION 2.0.0 LANGUAGES C ASM)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

add_compile_options(
	-g
	-O2
	-Wall
	-Wextra
)

add_executable(embroidermodder_pet
    ${CMAKE_SOURCE_DIR}/src/main.c
)

add_subdirectory(libembroidery)
add_subdirectory(pico-sdk)

include_directories(
	${CMAKE_SOURCE_DIR}/libembroidery/src
	${CMAKE_SOURCE_DIR}/libembroidery/src/stb
	${CMAKE_SOURCE_DIR}/libembroidery/src/nanosvg
	${CMAKE_SOURCE_DIR}/src
)

target_link_libraries(embroidermodder_pet PRIVATE embroidery_static)

if (WIN32)
else(WIN32)
target_link_libraries(embroidermodder_pet PRIVATE m)
endif()
