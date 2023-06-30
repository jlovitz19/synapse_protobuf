set(PROTO_FILES
    actuators.proto
    bezier_curve.proto
    bezier_trajectory.proto
    can.proto
    clock.proto
    header.proto
    joy.proto
    nav_sat_fix.proto
    nav_sat_status.proto
    odometry.proto
    point.proto
    pose.proto
    pose_with_covariance.proto
    quaternion.proto
    time.proto
    timestamp.proto
    twist.proto
    twist_with_covariance.proto
    vector3.proto)

set(PROTO_SRCS)
set(PROTO_HDRS)
foreach(PROTO_FILE ${PROTO_FILES})
  message(STATUS "proto file: ${PROTO_FILE}")
  protobuf_generate_cpp(PROTO_SRC PROTO_HDR ${PROTO_FILE})
  list(APPEND PROTO_SRCS ${PROTO_SRC})
  list(APPEND PROTO_HDRS ${PROTO_HDR})
endforeach()

add_library(${PROJECT_NAME}
  ${PROTO_SRCS}
  ${PROTO_HDRS}
)

ament_target_dependencies(${PROJECT_NAME} Protobuf) 

target_include_directories(${PROJECT_NAME}
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
    $<INSTALL_INTERFACE:include>)

install(
  TARGETS ${PROJECT_NAME}
  EXPORT ${PROJECT_NAME}
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)

install(FILES
  ${PROTO_HDRS}
  DESTINATION include/${PROJECT_NAME}
)
