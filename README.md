# Usage

***Theoretically, this can be used for all STM32 single-core MCUs.However, you need to modify the CMakeLists.txt.***

***Now cubemx can create cmake project,I don't know the meaning of this project and WHY I create it.Just for the cubeide project?***

## MODIFY FILES

In applications/CMakeLists.txt:

```cmake
target_link_libraries(template.elf template)
```

Add third-party after template.

If the boards need third-party.In boards/CMakeLists.txt:

```cmake
# target_link_libraries(template PUBLIC freertos)
```

Uncomment this code and add the third-party's name.

## HOW TO USE Third-party

You just need to copy the FREERTOS's CMakeLists and change the name.