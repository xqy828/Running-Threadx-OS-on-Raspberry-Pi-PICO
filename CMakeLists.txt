cmake_minimum_required(VERSION 3.12)

# Pull in SDK (must be before project)
include(pico_sdk_import.cmake)

project(pico-threadx C CXX ASM)

# Initialize the SDK
pico_sdk_init()

#add_definitions(-DSysTick_Handler=isr_systick)

add_executable(pico-threadx  src/main.c src/tx_initialize_low_level.S )

pico_add_extra_outputs(pico-threadx)

set(THREADX_ARCH "cortex_m0")
set(THREADX_TOOLCHAIN "gnu")
add_subdirectory(../threadx-6.2.0_rel threadx)
target_link_libraries(pico-threadx pico_stdlib cmsis_core threadx)

add_compile_options(-Wall
        -Wno-format          # int != int32_t as far as the compiler is concerned because gcc has int32_t as long int
        -Wno-unused-function # we have some for the docs that aren't called
        -Wno-maybe-uninitialized
        )
