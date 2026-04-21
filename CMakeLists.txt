cmake_minimum_required(VERSION 3.10)

project(framebuffer-vncserver
  LANGUAGES C
  DESCRIPTION "VNC server for Linux framebuffer devices"
  HOMEPAGE_URL "https://github.com/ponty/framebuffer-vncserver"
)


set(CMAKE_POSITION_INDEPENDENT_CODE ON)
# aka link-time-optimization (LTO)
set(CMAKE_INTERPROCEDURAL_OPTIMIZATION ON)

# https://best.openssf.org/Compiler-Hardening-Guides/Compiler-Options-Hardening-Guide-for-C-and-C++.html
add_compile_options(
  -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=3 -Wall -Wextra -Wpedantic

  -Wstack-protector -fstack-clash-protection -fstack-protector-strong
  -Wformat=2 -Wformat-nonliteral -Wwrite-strings -Werror=format-security
)


file(GLOB SOURCES src/*.c)
add_executable(${PROJECT_NAME} ${SOURCES})
install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)

# LIBVNC
find_library(LIBVNC NAMES libvncserver vncserver REQUIRED)
target_link_libraries(${PROJECT_NAME} ${LIBVNC})


message(STATUS "LIBVNC:         " ${LIBVNC})
