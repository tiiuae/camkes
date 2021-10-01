#
# Copyright 2018, Data61, CSIRO (ABN 41 687 119 230)
#
# SPDX-License-Identifier: BSD-2-Clause
#

cmake_minimum_required(VERSION 3.7.2)

project(debug-simple C)

find_package(camkes-tool REQUIRED)
find_package(seL4_libs REQUIRED)
find_package(seL4 REQUIRED)
includeGlobalComponents()

# We need to preprocess our .camkes spec to insert the debug components
set(CAmkESCPP ON CACHE BOOL "" FORCE)

# Need support for the hardware debug API
set(HardwareDebugAPI ON CACHE BOOL "" FORCE)
set(KernelDebugBuild ON CACHE BOOL "" FORCE)

DeclareCAmkESComponent(Client SOURCES client.c)
DeclareCAmkESComponent(Echo SOURCES echo.c)

# Only support ia32 at the moment
if(("${KernelPlatform}" STREQUAL "pc99" AND NOT "${KernelSel4Arch}" STREQUAL "ia32")
   OR
   (NOT "${KernelPlatform}" STREQUAL "pc99")
)
    message(FATAL_ERROR "debug-simple application only supported on ia32")
endif()

# Since we cannot use generator expressions in CPP_FLAGS we have to hard code the include path
# that we want
#DeclareCAmkESRootserver(
#    debug-simple.camkes CPP_FLAGS -I${CMAKE_SOURCE_DIR}/projects/camkes-tool/libsel4camkes/include
#)
DeclareCAmkESRootserver(
    debug-simple.camkes
#    CPP_FLAGS
#    ${CAMKES_ROOT_VM_CPP_FLAGS}
    CPP_INCLUDES
#    ${CAMKES_ROOT_VM_CPP_INCLUDES}
    ${CAMKES_TOOL_DIR}/libsel4camkes/include/)
#libsel4camkes
