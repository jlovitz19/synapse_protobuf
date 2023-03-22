file(GLOB PROTO_FILES "*.proto")
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS DESCRIPTORS ${PROTO_FILES})  

add_library(${PROJECT_NAME}
  ${PROTO_SRCS}
  ${PROTO_HDRS}
)

target_include_directories(${PROJECT_NAME} PRIVATE
  ${Protobuf_INCLUDE_DIR}
  ${CMAKE_CURRENT_BINARY_DIR}
)

install(FILES
  ${PROTO_HDRS}
  DESTINATION include/${PROJECT_NAME}
)

install(TARGETS
  ${PROJECT_NAME}
  DESTINATION lib/${PROJECT_NAME}
)
