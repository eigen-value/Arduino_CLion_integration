# CMakeLists.txt file for arm boards
# UNO R4 MINIMA example

cmake_minimum_required(VERSION 3.16)

project(MyLib VERSION 0.0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(ARDUINO_SDK_PATH "C:/Users/<username>/AppData/Local/Arduino15")
set(ARDUINO_LIBS_PATH "C:/Users/<username>/Documents/Arduino/libraries")
set(ARDUINO_CORE_PATH "${ARDUINO_SDK_PATH}/packages/arduino/hardware/renesas_uno/1.4.1/cores/arduino")
set(ARDUINO_VARIANT_PATH "${ARDUINO_SDK_PATH}/packages/arduino/hardware/renesas_uno/1.4.1/variants/MINIMA")
set(ARDUINO_TOOLCHAIN_PATH "${ARDUINO_SDK_PATH}/packages/arduino/tools/arm-none-eabi-gcc/7-2017q4/lib/gcc/arm-none-eabi/7.2.1/include")

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
        ${ARDUINO_SDK_PATH}/libraries

        # Arduino RouterBridge dependencies
#        ${ARDUINO_LIBS_PATH}/CAN/src
#        ${ARDUINO_LIBS_PATH}/Arduino_BMI270_BMM150/src
#        ${ARDUINO_LIBS_PATH}/Adafruit_BMP280_Library

        # HW specific libs
#        ${ARDUINO_SDK_PATH}/packages/arduino/hardware/renesas_uno/1.4.1/libraries/EEPROM/src
#        ${ARDUINO_SDK_PATH}/packages/arduino/hardware/renesas_uno/1.4.1/libraries/Wire
)

# Add Arduino defines
target_compile_definitions(${PROJECT_NAME}
        PUBLIC
        ARDUINO=10819
        ARDUINO_UNOR4_MINIMA
        ARDUINO_ARCH_RENESAS
        __RA4M1__
        CPU_SPEED=48000000L
)

# Mark headers as public headers
set_target_properties(${PROJECT_NAME} PROPERTIES
        PUBLIC_HEADER "${HEADERS}"
        LINKER_LANGUAGE CXX
)