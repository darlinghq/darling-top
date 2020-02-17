project(top)

# Automatically escape macros
cmake_policy(SET CMP0005 NEW)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc -fwrapv -include errno.h -include inttypes.h")

include_directories(${CMAKE_SOURCE_DIR}/src/libutil)

add_definitions(
	-Wno-tautological-compare
	-Wno-switch
	-Wno-implicit-function-declaration

	-DTOP_ANONYMOUS_MEMORY
)

add_darling_object_library(top-static
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
	$<TARGET_OBJECTS:top-static>
)

target_link_libraries(top CoreFoundation ncurses panel util IOKit)

install(TARGETS top DESTINATION libexec/darling/usr/bin)

install(FILES top.1 DESTINATION libexec/darling/usr/share/man/man1)
