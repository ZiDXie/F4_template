file(GLOB APP_SOURCES "Src/*.*")
file(GLOB APP_HEADERS "Inc/*.*")

add_executable(template.elf ${APP_SOURCES} ${APP_HEADERS})
target_include_directories(template.elf PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/Inc")

target_link_libraries(template.elf template)

file(GLOB LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/boards/*FLASH.ld)
target_link_options(template.elf PRIVATE -T ${LINKER_SCRIPT})