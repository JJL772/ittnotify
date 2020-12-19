project(ittnotify C CXX)

include(GNUInstallDirs)

set(HEADERS
	include/ittnotify.h 
	include/jitprofiling.h
	include/libittnotify.h
	include/llvm_jit_event_listener.hpp
	include/legacy/ittnotify.h)

set(SRCS
	src/ittnotify/jitprofiling.c 
	src/ittnotify/ittnotify_static.c)

add_library(ittnotify STATIC ${SRCS})

include_directories(ittnotify PUBLIC include/)
set_target_properties(ittnotify PROPERTIES PUBLIC_HEADER "${HEADERS}")

if(NOT WIN32)
	target_link_libraries(ittnotify dl)
endif()

install(TARGETS ittnotify
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/ittnotify) 
