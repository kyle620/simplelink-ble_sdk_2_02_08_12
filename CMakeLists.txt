cmake_minimum_required(VERSION 3.28)

project(ble_sdk_2_02_08_12 VERSION 2.2.8.12 LANGUAGES C)

# Include the shared file for Cortex-M3 settings
include("cmake/logic_modules/cortex-m3.cmake")

# Inlclude the shared file for CC2650-specific settings
include("cmake/logic_modules/cc2650-ti-cgt.cmake")

# Include the function for handling TIRTOS configuration
include("cmake/external_libraries/TIRTOS-Config.cmake")

# ─── Compile Definitions ────────────────────────────────────────────────────
target_compile_definitions(ble_sdk_2_02_08_12 PRIVATE
    BOARD_DISPLAY_EXCLUDE_UART
    CC2650DK_7ID
    CC26XX
    GAPROLE_TASK_STACK_SIZE=540
    HEAPMGR_SIZE=0
    ICALL_MAX_NUM_ENTITIES=6
    ICALL_MAX_NUM_TASKS=3
    POWER_SAVING
    USE_ICALL
    xBOARD_DISPLAY_EXCLUDE_LCD
    xDisplay_DISABLE_ALL
    xdc_runtime_Assert_DISABLE_ALL
    xdc_runtime_Log_DISABLE_ALL
)

# ─── TI-CGT Compiler Flags ──────────────────────────────────────────────────
target_compile_options(ble_sdk_2_02_08_12 PRIVATE
    -g
    --c99
    --gcc
    --diag_suppress=48
    --diag_warning=225
    --diag_wrap=off
    --display_error_number
    --gen_func_subsections=on
    --abi=eabi
)

# ─── Gather Sources ─────────────────────────────────────────────────────────
file(GLOB_RECURSE BLE_SDK_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/boards/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/common/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/components/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/config/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/controller/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/host/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/icall/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/inc/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/profiles/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/rom/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/target/*.c"
)

# ─── Static Library Target ──────────────────────────────────────────────────
add_library(ble_sdk_2_02_08_12 STATIC ${BLE_SDK_SOURCES})

# ─── Include Directories ────────────────────────────────────────────────────
target_include_directories(ble_sdk_2_02_08_12 PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/src/inc"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/boards"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/common"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/components"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/config"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/controller"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/host"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/icall"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/profiles"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/rom"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/target"
)