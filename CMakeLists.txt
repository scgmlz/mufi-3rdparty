cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

if(APPLE)
if(POLICY CMP0025)
    cmake_policy(SET CMP0025 NEW)
endif()
endif()

if(POLICY CMP0020)
    cmake_policy(SET CMP0020 NEW)
endif()

if(POLICY CMP0028)
    cmake_policy(SET CMP0028 NEW)
endif()

if(POLICY CMP0042)
    cmake_policy(SET CMP0042 NEW)
endif()

if(POLICY CMP0043)
    cmake_policy(SET CMP0043 NEW)
endif()

if(POLICY CMP0071)
  cmake_policy(SET CMP0071 NEW)
endif()

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)
include(PreventInSourceBuilds)

project(multifit_3rdparty VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)
if(WIN32)
    if(${CMAKE_SIZEOF_VOID_P} EQUAL 8)
      set(BORNAGAIN_ARCHITECTURE win64)
      set(BORNAGAIN_PLATFORM win64)
    else()
        set(BORNAGAIN_ARCHITECTURE win32)
        set(BORNAGAIN_PLATFORM win32)
    endif()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP /D_USE_MATH_DEFINES -I${CMAKE_SOURCE_DIR} -FIw32pragma.h")
    set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} /NODEFAULTLIB:libcmtd.lib")
endif()

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
file(MAKE_DIRECTORY ${CMAKE_LIBRARY_OUTPUT_DIRECTORY})
if(WIN32)
    set(libprefix _lib)
    set(libsuffix .dll)
elseif(APPLE)
    set(libprefix _lib)
    set(libsuffix .so)
else()
    set(libprefix _lib)
    set(libsuffix .so)
endif()

if(WIN32)
    set(destination_bin bin)
    set(destination_lib ${destination_bin})
    set(destination_gui bin)
    set(destination_include include)
    set(destination_examples Examples)
    set(destination_images Images)
    set(destination_libexec python)
else()
    set(destination_suffix BornAgain-${BornAgain_VERSION_MAJOR}.${BornAgain_VERSION_MINOR})
    if(APPLE AND BORNAGAIN_APPLE_BUNDLE)
        set(destination_bundle BornAgain.app)
        set(destination_prefix ${destination_bundle}/Contents/)
        set(destination_libexec ${destination_prefix}libexec/${destination_suffix})
    else()
        if(APPLE)
            set(QTDIR $ENV{QTDIR})
            message(STATUS "QTDIR is ${QTDIR}")
            set(CMAKE_INSTALL_RPATH ${QTDIR}/lib)
        endif()
        set(destination_prefix "")
        set(destination_libexec ${destination_prefix}lib/${destination_suffix})
    endif()
    set(destination_bin ${destination_prefix}bin)
    set(destination_gui ${destination_libexec})
    set(destination_lib ${destination_prefix}lib/${destination_suffix})
    set(destination_include ${destination_prefix}include/${destination_suffix})
    set(destination_share ${destination_prefix}share/${destination_suffix}/)
    set(destination_examples ${destination_share}Examples)
    set(destination_images ${destination_share}Images)
endif()

message(STATUS "Destination directories:
    bin->${destination_bin},
    lib->${destination_lib},
    gui->${destination_gui},
    include->${destination_include},
    share->${destination_share},
    examples->${destination_examples},
    images->${destination_images},
    libexec->${destination_libexec}")

find_package(GSL REQUIRED)


add_subdirectory(RootMinimizers)
