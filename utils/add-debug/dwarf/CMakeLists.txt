# Attention: This CMakeLists.txt implements the bare minimum of what is
#            required by SWEB.

cmake_minimum_required(VERSION 3.0.2)

set(CMAKE_CXX_STANDARD 11)

add_custom_command(
    OUTPUT to_string.cc
    COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/gen_to_string.sh ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc
    DEPENDS ../elf/enum-print.py dwarf++.hh data.hh internal.hh
)

add_library(dwarf++ STATIC
    dwarf.cc cursor.cc die.cc value.cc abbrev.cc
    expr.cc rangelist.cc line.cc attrs.cc
    die_str_map.cc elf.cc to_string.cc
)

# This is needed for the out-of-source generated to_string.cc
target_include_directories(dwarf++ PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")
