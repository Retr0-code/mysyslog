cmake_minimum_required(VERSION 3.17.0)

project(mysyslog VERSION 0.1)

# Configure compiler
set(DIR_INCLUDE "include")
set(DIR_SOURCE "src")

# Server variables
set(MYSYSLOG_SRC
    ${DIR_SOURCE}/mysyslog.c
)

set(DRIVERS_SRC
    ${DIR_SOURCE}/drivers/mysyslog_json.c
    ${DIR_SOURCE}/drivers/mysyslog_text.c
)

add_compile_options(-fPIC)

foreach(driver ${DRIVERS_SRC})
    get_filename_component(lib_name ${driver} NAME_WE)
    add_library(${lib_name} SHARED ${driver})
    target_compile_definitions(${lib_name} PRIVATE USE_BACKEND_MYSYSLOG)
    target_include_directories(${lib_name} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/${DIR_INCLUDE})
endforeach()


add_library(${PROJECT_NAME} SHARED ${MYSYSLOG_SRC})
target_compile_definitions(${PROJECT_NAME} PRIVATE USE_BACKEND_MYSYSLOG)
target_include_directories(${PROJECT_NAME} PUBLIC ${DIR_INCLUDE})

