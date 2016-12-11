project(top)

# Automatically escape macros
cmake_policy(SET CMP0005 NEW)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -fwrapv -fPIC -include errno.h -include inttypes.h")

set(CMAKE_SHARED_LINKER_FLAGS "-nodefaultlibs -nostdlib -fPIC -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")
set(CMAKE_EXE_LINKER_FLAGS "-nodefaultlibs -nostdlib -fPIC -Wl,--version-script=${DARLING_TOP_DIRECTORY}/darwin.map")

SET(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/darling")
SET(CMAKE_BUILD_WITH_INSTALL_RPATH TRUE)
SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_SOURCE_DIR}/platform-include
	${CMAKE_SOURCE_DIR}/src/external/corefoundation/Headers
	${CMAKE_BINARY_DIR}/src/external/corefoundation/Headers
	${CMAKE_SOURCE_DIR}/src/external/foundation/Headers
	${CMAKE_SOURCE_DIR}/kernel-include/iokit
	${CMAKE_SOURCE_DIR}/src/libc/darwin
	${CMAKE_SOURCE_DIR}/src/ncurses/include
	${CMAKE_SOURCE_DIR}/src/ncurses/panel
	${CMAKE_SOURCE_DIR}/src/libutil
)

add_definitions(
	-Wno-tautological-compare
	-Wno-switch
	-Wno-implicit-function-declaration

	-DTOP_ANONYMOUS_MEMORY
)

add_library(top-static STATIC
	libtop.c
)

add_darling_executable(top
	command.c
	cpu.c
	csw.c
	faults.c
	generic.c
	globalstats.c
	layout.c
	log.c
	main.c
	memstats.c
	messages.c
	options.c
	pgrp.c
	pid.c
	ports.c
	ppid.c
	preferences.c
	pstate.c
	statistic.c
	syscalls.c
	threads.c
	timestat.c
	top.c
	uid.c
	uinteger.c
	user.c
	userinput.c
	userinput_mode.c
	userinput_order.c
	userinput_sleep.c
	userinput_user.c
	workqueue.c
	userinput_signal.c
	logging.c
	userinput_secondary_order.c
	sig.c
	userinput_help.c
	power.c
)

target_link_libraries(top top-static CFF ncurses panel util IOKit)

install(TARGETS top DESTINATION libexec/darling/usr/bin)

install(FILES top.1 DESTINATION libexec/darling/usr/share/man/man1)
