cmake_policy(SET CMP0091 NEW)
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

if(MSVC)
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /MT")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MT")
	set(CMAKE_STATIC_LINKER_FLAGS "${CMAKE_STATIC_LINKER_FLAGS} /NODEFAULTLIB")
endif()

add_library(ittnotify STATIC ${SRCS})

include_directories(ittnotify PUBLIC include/)
set_target_properties(ittnotify PROPERTIES PUBLIC_HEADER "${HEADERS}")
set_property(TARGET ittnotify PROPERTY
	MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

if(NOT WIN32)
	target_link_libraries(ittnotify dl)
endif()

install(TARGETS ittnotify
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/ittnotify) 
