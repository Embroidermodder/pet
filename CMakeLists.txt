cmake_minimum_required(VERSION 3.12)

# cmake build-essential gcc-arm-none-eabi

set(NAME embroidermodder_pet)

set(EMBROIDERY_BUILD_SHARED_LIBS OFF)

include(pimoroni_pico_import.cmake)
include(pico_sdk_import.cmake)

project(${NAME} C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# add_compile_options(
# 	-g
# 	-O2
# 	-Wall
# 	-Wextra
# )

# Initialize the SDK
pico_sdk_init()

# Add your source files
add_executable(${NAME}
    src/main.cpp
)

# Include required libraries
# This assumes `pimoroni-pico` is stored alongside your project
include(common/pimoroni_i2c)
include(common/pimoroni_bus)
include(libraries/bitmap_fonts/bitmap_fonts)
include(libraries/hershey_fonts/hershey_fonts)
include(libraries/pico_explorer/pico_explorer)

# add_subdirectory(libembroidery)
#
# 	${CMAKE_SOURCE_DIR}/libembroidery/src
# 	${CMAKE_SOURCE_DIR}/libembroidery/src/stb
# 	${CMAKE_SOURCE_DIR}/libembroidery/src/nanosvg
include_directories(
 	${CMAKE_SOURCE_DIR}/src
)

# embroidery_static
target_link_libraries(${NAME}
    pimoroni_i2c
    pico_explorer
)

# create map/bin/hex file etc.
pico_add_extra_outputs(${NAME})

# Set up files for the release packages
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${NAME}.uf2
    ${CMAKE_CURRENT_LIST_DIR}/README.md
    DESTINATION .
)

set(CPACK_INCLUDE_TOPLEVEL_DIRECTORY OFF)
set(CPACK_GENERATOR "ZIP" "TGZ")
include(CPack)
