cmake_minimum_required(VERSION 3.12)

# Change your executable name to something creative!
set(NAME audio)

include(pimoroni_pico_import.cmake)
include(pico_sdk_import.cmake)
include(pico_extras_import.cmake)

# Gooey boilerplate
project(${NAME} C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialize the SDK
pico_sdk_init()

# Add your source files
add_executable(${NAME}
    src/demo.cpp
    src/synth.cpp
    src/synth.hpp
    src/audio.hpp
)

# Include required libraries
# This assumes `pimoroni-pico` is stored alongside your project


# Don't forget to link the libraries you need!
target_link_libraries(${NAME}
    pico_stdlib
    pico_audio_i2s
)

target_compile_definitions(audio PRIVATE
# compile time configuration of I2S
PICO_AUDIO_I2S_MONO_INPUT=1
#define for our example code
USE_AUDIO_I2S=1
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
