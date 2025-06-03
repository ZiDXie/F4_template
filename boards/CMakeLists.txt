file(GLOB_RECURSE BOARD_SOURCES
        "Core/Src/*.c"
        "Core/Startup/*.s"
        "Drivers/STM32F4xx_HAL_Driver/Src/*.c"
        "Drivers/CMSIS/Device/ST/STM32F4xx/Source/Templates/*.c"
)

add_library(template STATIC ${BOARD_SOURCES})
# 如需依赖第三方库，可在此添加
# target_link_libraries(template PUBLIC freertos)
