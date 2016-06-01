cmake_minimum_required(VERSION 2.8)

enable_language(C)

INCLUDE (CheckIncludeFiles)
INCLUDE (CheckTypeSize)
INCLUDE (CheckSymbolExists)
INCLUDE (CheckFunctionExists)
INCLUDE (CheckLibraryExists)

# set path to additional CMake modules
SET(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake ${CMAKE_MODULE_PATH})

function (check_type_exists type variable header default)
  set(CMAKE_EXTRA_INCLUDE_FILES "${header}")
  check_type_size("${type}" "${variable}")

  if (NOT HAVE_${type})
    set("${variable}" "${default}" PARENT_SCOPE)
  else ()
    set("${variable}" "FALSE" PARENT_SCOPE)
  endif ()
  set(CMAKE_EXTRA_INCLUDE_FILES)
endfunction ()

INCLUDE (TestSignalType)
INCLUDE (CheckForVarArgs)
INCLUDE (CheckForVaCopy)

# see configure.ac L47
check_type_exists("socklen_t"    SOCKLEN_T "sys/socket.h"     "")

check_include_files(string.h             HAVE_STRING_H)
check_include_files(stdlib.h             HAVE_STDLIB_H)

# see configure.ac L273
check_include_files(unistd.h             HAVE_UNISTD_H)
check_include_files(sys/types.h          HAVE_SYS_TYPES_H)
check_include_files(sys/sendfile.h       HAVE_SYS_SENDFILE_H)
check_include_files(sys/signal.h         HAVE_SYS_SIGNAL_H)
# see configure.ac L274
check_include_files(ctype.h              HAVE_CTYPE_H)
check_include_files(sys/resource.h       HAVE_SYS_RESOURCE_H)
check_include_files(sys/socket.h         HAVE_SYS_SOCKET_H)
check_include_files(sys/select.h         HAVE_SYS_SELECT_H)
# see configure.ac L275
check_include_files(netinet/in.h         HAVE_NETINET_IN_H)
# see configure.ac L280
check_include_files(arpa/nameser.h       HAVE_ARPA_NAMESER_H)
# see configure.ac L285
check_include_files(resolv.h             HAVE_RESOLV_H)
# see configure.ac L297
check_include_files(float.h              HAVE_FLOAT_H)
check_include_files(mcheck.h             HAVE_MCHECK_H)
check_include_files(alloca.h             HAVE_ALLOCA_H)
check_include_files(sys/mman.h           HAVE_SYS_MMAN_H)
check_include_files(sys/loadavg.h        HAVE_SYS_LOADAVG_H)
# see configure.ac L298
check_include_files(elf.h                HAVE_ELF_H)
# see configure.ac L299
check_include_files(fnmatch.h            HAVE_FNMATCH_H)

# see configure.ac L304
check_type_exists("in_port_t"    IN_PORT_T "netinet/in.h"     "")
check_type_exists("in_addr_t"    IN_ADDR_T "netinet/in.h"     "")

# see configure.ac L331
check_library_exists(nsl gethostent "" HAVE_NSL)
# see configure.ac L332
check_library_exists(socket setsockopt "" HAVE_SOCKET)
# see configure.ac L333
check_library_exists(resolv hstrerror "" HAVE_RESOLV)
# see configure.ac L333
check_library_exists(resolv inet_aton "" HAVE_RESOLV)
# see configure.ac L320
check_library_exists(popt poptGetContext "" HAVE_POPT)
check_include_files(popt.h HAVE_POPT_H)
if(NOT HAVE_POPT_H  AND NOT HAVE_POPT)
    #message(FATAL_ERROR "Missing popt")
    include_directories(popt)

    set(popt_SRC popt/findme.c  popt/popt.c  popt/poptconfig.c
                 popt/popthelp.c popt/poptparse.c)
endif()

# see configure.ac L375
check_function_exists("getpagesize" HAVE_GETPAGESIZE)
# see configure.ac L376
check_function_exists("sendfile" HAVE_SENDFILE)
check_function_exists("setsid" HAVE_SETSID)
check_function_exists("flock" HAVE_FLOCK)
check_function_exists("lockf"    HAVE_LOCKF)
check_function_exists("hstrerror"    HAVE_HSTRERROR)
check_function_exists("strerror"    HAVE_STRERROR)
check_function_exists("setuid" HAVE_SETUID)
check_function_exists("setreuid" HAVE_SETREUID)
# see configure.ac L377
check_function_exists("getuid"          HAVE_GETUID)
check_function_exists("geteuid"         HAVE_GETEUID)
check_function_exists("mcheck"          HAVE_MCHECK)
check_function_exists("wait4"           HAVE_WAIT4)
check_function_exists("wait3"           HAVE_WAIT3)
check_function_exists("waitpid"         HAVE_WAITPID)
check_function_exists("setgroups"       HAVE_SETGROUPS)
# see configure.ac L378
check_function_exists("snprintf"        HAVE_SNPRINTF)
check_function_exists("vsnprintf"       HAVE_VSNPRINTF)
check_function_exists("vasprintf"       HAVE_VASPRINTF)
check_function_exists("asprintf"        HAVE_ASPRINTF)
check_function_exists("getcwd"          HAVE_GETCWD)
check_function_exists("getwd"           HAVE_GETWD)
check_function_exists("mkdtemp"         HAVE_MKDTEMP)
# see configure.ac L379
check_function_exists("getrusage"       HAVE_GETRUSAGE)
check_function_exists("strsignal"       HAVE_STRSIGNAL)
check_function_exists("gettimeofday"    HAVE_GETTIMEOFDAY)
# see configure.ac L380
check_function_exists("getaddrinfo"     HAVE_GETADDRINFO)
check_function_exists("getnameinfo"     HAVE_GETNAMEINFO)
check_function_exists("inet_ntop"       HAVE_INET_NTOP)
check_function_exists("inet_ntoa"       HAVE_INET_NTOA)
# see configure.ac L381
check_function_exists("strndup"         HAVE_STRNDUP)
check_function_exists("strsep"          HAVE_STRSEP)
check_function_exists("mmap"            HAVE_MMAP)
check_function_exists("strlcpy"         HAVE_STRLCPY)

# see configure.ac L383
check_function_exists("getloadavg"      HAVE_GETLOADAVG)
# see configure.ac L384
check_function_exists("getline"         HAVE_GETLINE)

# see configure.ac L386
#check_function_exists("snprintf"        HAVE_SNPRINTF)
#check_function_exists("vsnprintf"       HAVE_VSNPRINTF)
#check_function_exists("vasprintf"       HAVE_VASPRINTF)
#check_function_exists("asprintf"        HAVE_ASPRINTF)
#check_function_exists("strndup"         HAVE_STRNDUP)

# see configure.ac L405
CHECK_FOR_ISO_C_VARARGS(HAVE_ISO_C_VARARG_MACROS)
CHECK_FOR_GNU_C_VARARGS(HAVE_GNU_C_VARARG_MACROS)

# see configure.ac L414
CHECK_FOR_VA_COPY(HAVE_VA_COPY)

add_definitions(-DSYSCONFDIR="/usr/local/etc")

CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/src/config.h.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/src/config.h)

include_directories(${CMAKE_CURRENT_BINARY_DIR}/src)
include_directories(${CMAKE_SOURCE_DIR}/lzo)

# see Makefile.in L230
set(common_obj
    src/arg.c src/argutil.c
	src/cleanup.c src/compress.c
	src/trace.c src/util.c src/io.c src/exec.c
	src/rpc.c src/tempfile.c src/bulk.c src/help.c src/filename.c
	src/lock.c
	src/netutil.c
	src/pump.c
	src/sendfile.c
	src/safeguard.c src/snprintf.c src/timeval.c
	src/dotd.c
	src/hosts.c src/hostfile.c
	src/implicit.c src/loadfile.c
	lzo/minilzo.c
	${ZEROCONF_COMMON_OBJS}
	${AUTH_COMMON_OBJS}
    )

project(distcc)

# see Makefile.in L246
add_executable(${PROJECT_NAME} src/backoff.c
	src/climasq.c src/clinet.c src/clirpc.c
	src/compile.c src/cpp.c
	src/distcc.c
	src/remote.c
	src/ssh.c src/state.c src/strip.c
	src/timefile.c src/traceenv.c
	src/include_server_if.c
	src/where.c
	${ZEROCONF_DISTCC_OBJS}
	${AUTH_DISTCC_OBJS}
	src/emaillog.c
	${common_obj}
    )

# see Makefile.in L260
project(distccd)

add_definitions(-DHAVE_CONFIG_H)

add_executable(${PROJECT_NAME} src/access.c
	src/daemon.c  src/dopt.c src/dparent.c src/dsignal.c
	src/ncpus.c
	src/prefork.c
	src/stringmap.c
	src/serve.c src/setuid.c src/srvnet.c src/srvrpc.c src/state.c
	src/stats.c
	src/fix_debug_info.c
	${ZEROCONF_DISTCC_OBJS}
	${AUTH_DISTCC_OBJS}
	src/emaillog.c
	${common_obj} ${popt_SRC}
    )

#project(distccd)
