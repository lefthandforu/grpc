# This file was generated from BUILD using tools/make_cmakelists.py.

cmake_minimum_required(VERSION 3.1)

if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
else()
    cmake_policy(VERSION 3.12)
endif()

cmake_minimum_required (VERSION 3.0)
cmake_policy(SET CMP0048 NEW)

project(upb)
set(CMAKE_C_STANDARD 99)


# Prevent CMake from setting -rdynamic on Linux (!!).
SET(CMAKE_SHARED_LIBRARY_LINK_C_FLAGS "")
SET(CMAKE_SHARED_LIBRARY_LINK_CXX_FLAGS "")

# Set default build type.
if(NOT CMAKE_BUILD_TYPE)
  message(STATUS "Setting build type to 'RelWithDebInfo' as none was specified.")
  set(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE STRING
      "Choose the type of build, options are: Debug Release RelWithDebInfo MinSizeRel."
      FORCE)
endif()

# When using Ninja, compiler output won't be colorized without this.
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG(-fdiagnostics-color=always SUPPORTS_COLOR_ALWAYS)
if(SUPPORTS_COLOR_ALWAYS)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fdiagnostics-color=always")
endif()

# Implement ASAN/UBSAN options
if(UPB_ENABLE_ASAN)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=address")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fsanitize=address")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fsanitize=address")
  set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -fsanitize=address")
endif()

if(UPB_ENABLE_UBSAN)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsanitize=undefined")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fsanitize=address")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fsanitize=address")
  set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -fsanitize=address")
endif()

include_directories(..)
include_directories(../cmake)
include_directories(${CMAKE_CURRENT_BINARY_DIR})

if(NOT TARGET utf8_range)
  if(EXISTS ../external/utf8_range)
    # utf8_range is already installed
    include_directories(../external/utf8_range)
  else()
    include(FetchContent)
    FetchContent_Declare(
      utf8_range
      GIT_REPOSITORY "https://github.com/protocolbuffers/utf8_range.git"
      GIT_TAG "de0b4a8ff9b5d4c98108bdfe723291a33c52c54f"
    )
    FetchContent_GetProperties(utf8_range)
    if(NOT utf8_range_POPULATED)
      FetchContent_Populate(utf8_range)
      include_directories(${utf8_range_SOURCE_DIR})
    endif()
  endif()
endif()

if(APPLE)
  set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -undefined dynamic_lookup -flat_namespace")
elseif(UNIX)
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,--build-id")
endif()

enable_testing()

add_library(port INTERFACE)
add_library(upb INTERFACE)
target_link_libraries(upb INTERFACE
  base
  collections_internal
  fastdecode
  hash
  lex
  mem
  message_internal
  mini_table_internal
  port
  wire)
add_library(base
  ../upb/base/status.c
  ../upb/base/descriptor_constants.h
  ../upb/base/log2.h
  ../upb/base/status.h
  ../upb/base/string_view.h)
target_link_libraries(base
  port)
add_library(mini_table INTERFACE)
target_link_libraries(mini_table INTERFACE
  base
  mem
  mini_table_internal
  port)
add_library(mini_table_internal
  ../upb/mini_table/common.c
  ../upb/mini_table/decode.c
  ../upb/mini_table/encode.c
  ../upb/mini_table/extension_registry.c
  ../upb/mini_table/common.h
  ../upb/mini_table/common_internal.h
  ../upb/mini_table/decode.h
  ../upb/mini_table/encode_internal.h
  ../upb/mini_table/encode_internal.hpp
  ../upb/mini_table/enum_internal.h
  ../upb/mini_table/extension_internal.h
  ../upb/mini_table/extension_registry.h
  ../upb/mini_table/field_internal.h
  ../upb/mini_table/file_internal.h
  ../upb/mini_table/message_internal.h
  ../upb/mini_table/sub_internal.h
  ../upb/mini_table/types.h)
target_link_libraries(mini_table_internal
  base
  hash
  mem
  port)
add_library(message INTERFACE)
target_link_libraries(message INTERFACE
  mem
  message_internal
  mini_table
  port)
add_library(message_internal
  ../upb/message/message.c
  ../upb/message/extension_internal.h
  ../upb/message/internal.h
  ../upb/message/message.h)
target_link_libraries(message_internal
  base
  hash
  mem
  mini_table_internal
  port)
add_library(message_accessors
  ../upb/message/accessors.c
  ../upb/message/accessors_internal.h
  ../upb/message/accessors.h)
target_link_libraries(message_accessors
  collections_internal
  eps_copy_input_stream
  hash
  message_internal
  mini_table_internal
  port
  upb
  wire
  wire_reader)
add_library(message_promote
  ../upb/message/promote.c
  ../upb/message/promote.h)
target_link_libraries(message_promote
  collections_internal
  eps_copy_input_stream
  hash
  message_accessors
  message_internal
  mini_table_internal
  port
  upb
  wire
  wire_reader)
add_library(message_copy
  ../upb/message/copy.c
  ../upb/message/copy.h)
target_link_libraries(message_copy
  collections_internal
  message_accessors
  message_internal
  mini_table_internal
  port
  upb)
add_library(fastdecode INTERFACE)
target_link_libraries(fastdecode INTERFACE
  base
  collections_internal
  hash
  mem_internal
  message_internal
  mini_table_internal
  port
  wire)
add_library(generated_code_support__only_for_generated_code_do_not_use__i_give_permission_to_break_me INTERFACE)
target_link_libraries(generated_code_support__only_for_generated_code_do_not_use__i_give_permission_to_break_me INTERFACE
  base
  collections_internal
  hash
  upb)
add_library(generated_cpp_support__only_for_generated_code_do_not_use__i_give_permission_to_break_me INTERFACE)
target_link_libraries(generated_cpp_support__only_for_generated_code_do_not_use__i_give_permission_to_break_me INTERFACE
  base
  collections_internal
  hash
  message_copy
  mini_table
  upb)
add_library(generated_reflection_support__only_for_generated_code_do_not_use__i_give_permission_to_break_me INTERFACE)
target_link_libraries(generated_reflection_support__only_for_generated_code_do_not_use__i_give_permission_to_break_me INTERFACE
  base
  descriptor_upb_proto
  hash
  mini_table_internal
  reflection_internal)
add_library(collections INTERFACE)
target_link_libraries(collections INTERFACE
  base
  collections_internal
  mem
  port)
add_library(collections_internal
  ../upb/collections/array.c
  ../upb/collections/map.c
  ../upb/collections/map_sorter.c
  ../upb/collections/array.h
  ../upb/collections/array_internal.h
  ../upb/collections/map.h
  ../upb/collections/map_gencode_util.h
  ../upb/collections/map_internal.h
  ../upb/collections/map_sorter_internal.h
  ../upb/collections/message_value.h)
target_link_libraries(collections_internal
  base
  hash
  mem
  message_internal
  mini_table_internal
  port)
add_library(textformat
  ../upb/text/encode.c
  ../upb/text/encode.h
  ../upb/text_encode.h)
target_link_libraries(textformat
  collections_internal
  eps_copy_input_stream
  lex
  port
  reflection
  wire
  wire_reader
  wire_types)
add_library(json
  ../upb/json/decode.c
  ../upb/json/encode.c
  ../upb/json/decode.h
  ../upb/json/encode.h
  ../upb/json_decode.h
  ../upb/json_encode.h)
target_link_libraries(json
  collections
  lex
  port
  reflection
  upb)
add_library(mem INTERFACE)
target_link_libraries(mem INTERFACE
  mem_internal
  port)
add_library(mem_internal
  ../upb/mem/alloc.c
  ../upb/mem/arena.c
  ../upb/mem/alloc.h
  ../upb/mem/arena.h
  ../upb/mem/arena_internal.h)
target_link_libraries(mem_internal
  port)
add_library(wire INTERFACE)
target_link_libraries(wire INTERFACE
  mem
  message_internal
  mini_table_internal
  port
  wire_internal)
add_library(wire_internal
  ../upb/wire/decode.c
  ../upb/wire/decode_fast.c
  ../upb/wire/encode.c
  ../upb/wire/common.h
  ../upb/wire/common_internal.h
  ../upb/wire/decode.h
  ../upb/wire/decode_fast.h
  ../upb/wire/decode_internal.h
  ../upb/wire/encode.h
  ../upb/wire/swap_internal.h)
target_link_libraries(wire_internal
  base
  collections_internal
  eps_copy_input_stream
  hash
  mem_internal
  message_internal
  mini_table_internal
  port
  wire_reader
  wire_types
  utf8_range)
add_library(wire_types INTERFACE)
add_library(eps_copy_input_stream
  ../upb/wire/eps_copy_input_stream.c
  ../upb/wire/eps_copy_input_stream.h)
target_link_libraries(eps_copy_input_stream
  mem
  port)
add_library(wire_reader
  ../upb/wire/reader.c
  ../upb/wire/swap_internal.h
  ../upb/wire/reader.h)
target_link_libraries(wire_reader
  eps_copy_input_stream
  port
  wire_types)
add_library(hash
  ../upb/hash/common.c
  ../upb/hash/common.h
  ../upb/hash/int_table.h
  ../upb/hash/str_table.h)
target_link_libraries(hash
  base
  mem
  port)
add_library(lex
  ../upb/lex/atoi.c
  ../upb/lex/round_trip.c
  ../upb/lex/strtod.c
  ../upb/lex/unicode.c
  ../upb/lex/atoi.h
  ../upb/lex/round_trip.h
  ../upb/lex/strtod.h
  ../upb/lex/unicode.h)
target_link_libraries(lex
  port)


