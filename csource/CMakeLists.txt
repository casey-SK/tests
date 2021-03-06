cmake_minimum_required(VERSION 3.13)

# initialize pico-sdk from GIT
# (note this can come from environment, CMake cache etc)
#set(PICO_SDK_FETCH_FROM_GIT on)

# pico_sdk_import.cmake is a single file copied from this SDK
# note: this must happen before project()
include(pico_sdk_import.cmake)

project(tests)

# initialize the Raspberry Pi Pico SDK
pico_sdk_init()

# add program file(s)
file(GLOB MyCSources build/nimcache/*.c)
add_executable(tests ${MyCSources})

# Add directory containing this CMakeLists file to include search path.
# This is required so that the nimbase.h file is found. Other headers
# required for a project can also be placed here.
target_include_directories(tests PUBLIC ${CMAKE_CURRENT_LIST_DIR})

# pull in our pico_stdlib which pulls in commonly used features
# hardware_adc tinyusb_device tinyusb_board Currently supported.
target_link_libraries(tests pico_stdlib)
include(pico_libs.txt)

# enable usb output, disable uart output
pico_enable_stdio_usb(tests 1)
pico_enable_stdio_uart(tests 0)

# create map/bin/hex/uf2 file etc.
pico_add_extra_outputs(tests)

# add url via pico_set_program_url
pico_set_program_url(tests 1)
