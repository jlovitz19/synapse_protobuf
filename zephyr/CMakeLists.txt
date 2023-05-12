# SPDX-License-Identifier: Apache-2.0
if (CONFIG_SYNAPSE_PROTOBUF)

set(SYNAPSE_PROTOBUF_DIR ${ZEPHYR_CURRENT_MODULE_DIR})
set(PROTO_DIR ${SYNAPSE_PROTOBUF_DIR}/proto)
 
file(GLOB PROTO_FILES "${PROTO_DIR}/*.proto")
file(GLOB OPTION_FILES "${PROTO_DIR}/*.option")

message(STATUS "files:${PROTO_FILES}")
   
set(NANOPB_IMPORT_DIRS "${PROTO_DIR}")
set(NANOPB_OPTIONS "-I${CMAKE_SOURCE_DIR}")

nanopb_generate_cpp(PROTO_SOURCES PROTO_HEADERS
        RELPATH ${PROTO_DIR} ${PROTO_FILES})
message(STATUS "srcs:${PROTO_SOURCES}")
   
zephyr_library_named(synapse_protobuf)

target_compile_options(synapse_protobuf PRIVATE -Wall -Wextra -Wpedantic -Werror)
    
# we need to be able to include generated header files
zephyr_include_directories(
  ${CMAKE_CURRENT_BINARY_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}/..
)

zephyr_library_sources(
  ${PROTO_SOURCES}
  ${PROTO_HEADERS}
  )

# necessary for module to calll nanopb_generate
add_dependencies(app synapse_protobuf)
        
endif()
