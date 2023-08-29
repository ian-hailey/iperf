cmake_minimum_required(VERSION 3.9)
project(iperf VERSION 3.6)

include(cmake/configure.cmake)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/cmake/config/iperf_config.cmake.in
               ${CMAKE_CURRENT_BINARY_DIR}/cmake/iperf_config.h @ONLY)
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/cmake/config/version.cmake.in
               ${CMAKE_CURRENT_BINARY_DIR}/cmake/version.h @ONLY)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/cmake)

add_compile_options(-Wno-deprecated-declarations)

list(APPEND LIBIPERF_SRCS
    src/cjson.c src/dscp.c src/iperf_api.c src/iperf_auth.c
    src/iperf_client_api.c src/iperf_error.c src/iperf_locale.c
    src/iperf_sctp.c src/iperf_server_api.c src/iperf_tcp.c
    src/iperf_time.c src/iperf_udp.c src/iperf_util.c src/net.c
    src/t_api.c src/t_timer.c src/t_units.c src/t_uuid.c
    src/tcp_info.c src/timer.c src/units.c
    )
list(APPEND LIBIPERF_HDRS src/iperf_api.h)


add_library(${PROJECT_NAME} STATIC ${LIBIPERF_SRCS})
set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER ${LIBIPERF_HDRS})

add_executable(iperf3 src/main.c)
target_link_libraries(iperf3 ${PROJECT_NAME})

install(TARGETS ${PROJECT_NAME}
        ARCHIVE DESTINATION lib
        PUBLIC_HEADER DESTINATION include)

install(TARGETS iperf3 RUNTIME DESTINATION bin)

