cmake_minimum_required(VERSION 3.31)
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_VERSION 1)

# 工具链设置
set(CMAKE_C_COMPILER arm-none-eabi-gcc)
set(CMAKE_CXX_COMPILER arm-none-eabi-g++)
set(CMAKE_ASM_COMPILER arm-none-eabi-gcc)
set(CMAKE_AR arm-none-eabi-ar)
set(CMAKE_OBJCOPY arm-none-eabi-objcopy)
set(CMAKE_OBJDUMP arm-none-eabi-objdump)
set(SIZE arm-none-eabi-size)
set(CMAKE_TRY_COMPILE_TARGET_TYPE STATIC_LIBRARY)

project(template C CXX ASM)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_C_STANDARD 11)

# 全局编译选项
add_compile_definitions(ARM_MATH_CM4;ARM_MATH_MATRIX_CHECK;ARM_MATH_ROUNDING)
add_compile_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
add_link_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
add_compile_options(-mcpu=cortex-m4 -mthumb -mthumb-interwork)
add_compile_options(-ffunction-sections -fdata-sections -fno-common -fmessage-length=0)
add_compile_options($<$<COMPILE_LANGUAGE:ASM>:-x$<SEMICOLON>assembler-with-cpp>)

add_definitions(-DDEBUG -DUSE_HAL_DRIVER -DSTM32F407xx)

if ("${CMAKE_BUILD_TYPE}" STREQUAL "Release")
    add_compile_options(-Ofast)
elseif ("${CMAKE_BUILD_TYPE}" STREQUAL "RelWithDebInfo")
    add_compile_options(-Ofast -g)
elseif ("${CMAKE_BUILD_TYPE}" STREQUAL "MinSizeRel")
    add_compile_options(-Os)
else ()
    add_compile_options(-Og -g)
endif ()

add_link_options(-Wl,-gc-sections,--print-memory-usage,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map)
add_link_options(-mcpu=cortex-m4 -mthumb -mthumb-interwork)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

include_directories(
        ${CMAKE_SOURCE_DIR}/boards/Core/Inc
        ${CMAKE_SOURCE_DIR}/boards/Drivers/STM32F4xx_HAL_Driver/Inc
        ${CMAKE_SOURCE_DIR}/boards/Drivers/STM32F4xx_HAL_Driver/Inc/Legacy
        ${CMAKE_SOURCE_DIR}/boards/Drivers/CMSIS/Device/ST/STM32F4xx/Include
        ${CMAKE_SOURCE_DIR}/boards/Drivers/CMSIS/Include
)

add_subdirectory(third_party)
add_subdirectory(boards)
add_subdirectory(applications)

include(ClangFormat)
file(GLOB_RECURSE FORMAT "applications/*.h" "applications/*.c" "boards/*.h" "boards/*.c")
clangformat_setup(${FORMAT})