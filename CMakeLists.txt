cmake_minimum_required(VERSION 3.28)

project(ble_sdk_2_02_08_12 VERSION 2.2.8.12 LANGUAGES C)

include_guard(GLOBAL)

# Include shared architecture/chip settings
include("cmake/logic_modules/cortex-m3.cmake")
include("cmake/logic_modules/cc2650-ti-cgt.cmake")

# ─── Pre-built BLE Stack Libraries ──────────────────────────────────────────
add_library(ble_host STATIC IMPORTED GLOBAL)
set_target_properties(ble_host PROPERTIES
    IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/blelib/host/host_pxxx.a"
)

add_library(ble_ctrl STATIC IMPORTED GLOBAL)
set_target_properties(ble_ctrl PROPERTIES
    IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/blelib/ctrl/cc2640/cc2640_ctrl_pxxx.a"
)

add_library(ble_hci_tl STATIC IMPORTED GLOBAL)
set_target_properties(ble_hci_tl PROPERTIES
    IMPORTED_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}/blelib/hci_tl/cc26xx/cc26xx_hci_tl_none.a"
)

# ─── Gather Sources (profiles, icall, common — NOT the core stack) ───────────
file(GLOB_RECURSE BLE_SDK_SOURCES
    "${CMAKE_CURRENT_SOURCE_DIR}/src/common/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/components/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/config/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/icall/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/profiles/*.c"
    "${CMAKE_CURRENT_SOURCE_DIR}/src/target/*.c"
)

# ─── Gather Include Directories Recursively ──────────────────────────────────
file(GLOB_RECURSE BLE_SDK_HEADERS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.h")
list(TRANSFORM BLE_SDK_HEADERS REPLACE "/[^/]+\\.h$" "" OUTPUT_VARIABLE BLE_SDK_INCLUDE_DIRS)
list(REMOVE_DUPLICATES BLE_SDK_INCLUDE_DIRS)

# ─── Main Library Target ─────────────────────────────────────────────────────
add_library(${PROJECT_NAME} STATIC ${BLE_SDK_SOURCES})

# ─── Include Directories ─────────────────────────────────────────────────────
target_include_directories(${PROJECT_NAME} PUBLIC ${BLE_SDK_INCLUDE_DIRS})

# ─── Compile Definitions ─────────────────────────────────────────────────────
target_compile_definitions(${PROJECT_NAME} PRIVATE
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

# ─── Compiler Flags ──────────────────────────────────────────────────────────
target_compile_options(${PROJECT_NAME} PRIVATE
    --c99
    --gcc
    --diag_suppress=48
    --diag_warning=225
    --diag_wrap=off
    --display_error_number
    --gen_func_subsections=on
)

# ─── Link Everything Together ────────────────────────────────────────────────
target_link_libraries(${PROJECT_NAME} PUBLIC
    ble_host
    ble_ctrl
    ble_hci_tl
    cortex_m3_opts
    cc2650_config
)