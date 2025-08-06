cmake_minimum_required(VERSION 3.16)

project(Arduino_MyLib VERSION 0.0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(ARDUINO_SDK_PATH "C:/Users/<username>/AppData/Local/Arduino15")
set(ARDUINO_CORE_PATH "${ARDUINO_SDK_PATH}/packages/arduino/hardware/avr/1.8.6/cores/arduino")
set(ARDUINO_VARIANT_PATH "${ARDUINO_SDK_PATH}/packages/arduino/hardware/avr/1.8.6/variants/standard")
set(ARDUINO_TOOLCHAIN_PATH "${ARDUINO_SDK_PATH}/packages/arduino/tools/avr-gcc/7.3.0-atmel3.6.1-arduino7/avr/include")

# Collect header files
file(GLOB_RECURSE HEADERS "src/*.h" "src/*.hpp")

# Create dummy source file for IDE support
set(DUMMY_SOURCE "${CMAKE_BINARY_DIR}/dummy.cpp")
file(WRITE ${DUMMY_SOURCE} "// Dummy file for IDE support\n")

# Create library with dummy source + headers
add_library(${PROJECT_NAME} ${DUMMY_SOURCE} ${HEADERS})

# Add after your existing target_include_directories
target_include_directories(${PROJECT_NAME}
        PUBLIC
        # Lib source paths
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/src

        # Arduino core paths
        ${ARDUINO_CORE_PATH}
        ${ARDUINO_VARIANT_PATH}
        ${ARDUINO_TOOLCHAIN_PATH}

        # Arduino libraries (common ones)
        C:/Users/<username>/AppData/Local/Arduino15/libraries

        # Arduino MyLib dependencies
        C:/Users/<username>/Documents/Arduino/libraries/FirstDependency

        # HW specific libs
        /home/username/.arduino15/packages/arduino/hardware/avr/1.8.6/libraries
)

# Add Arduino defines
target_compile_definitions(${PROJECT_NAME}
        PUBLIC
        ARDUINO=10819
        AVR
        __AVR_ATmega328P__
        F_CPU=16000000L
)

# Mark headers as public headers
set_target_properties(${PROJECT_NAME} PROPERTIES
        PUBLIC_HEADER "${HEADERS}"
        LINKER_LANGUAGE CXX
)