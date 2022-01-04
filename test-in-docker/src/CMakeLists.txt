cmake_minimum_required (VERSION 2.8)

project(lwip-runner)

find_path( PCAP_INCLUDE_DIR NAMES pcap/pcap.h pcap.h HINTS "${PCAP_HINTS}/include")

find_library( PCAP_LIBRARY NAMES pcap wpcap HINTS "${PCAP_HINTS}/lib")

find_path(LWIP_DIR src/include/lwip/init.h ${CMAKE_CURRENT_SOURCE_DIR}/../../../lwip)

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/../lwip-include
	${PCAP_INCLUDE_DIR}
	${LWIP_DIR}/src/include
	${CMAKE_CURRENT_SOURCE_DIR}/../../../ff
	${CMAKE_CURRENT_SOURCE_DIR}/../..
)

include(${LWIP_DIR}/src/Filelists.cmake)

add_compile_options(-Wall -Wextra -Werror -g3)

add_executable(lwip-runner
	${CMAKE_CURRENT_SOURCE_DIR}/main.c
	${CMAKE_CURRENT_SOURCE_DIR}/../lwip-include/sys_arch.c
	${CMAKE_CURRENT_SOURCE_DIR}/../../ftpd.c
	${CMAKE_CURRENT_SOURCE_DIR}/../../vfs.c
	${CMAKE_CURRENT_SOURCE_DIR}/../../../ff/source/ff.c
)
target_link_libraries(lwip-runner ${PCAP_LIBRARY} lwipcore)
