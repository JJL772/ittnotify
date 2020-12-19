project(ittnotify C CXX)

set(SRCS
	src/ittnotify/jitprofiling.c 
	src/ittnotify/ittnotify_static.c)

add_library(ittnotify STATIC ${SRCS})

include_directories(ittnotify PRIVATE include/)

if(NOT WIN32)
	target_link_libraries(ittnotify dl)
endif()
