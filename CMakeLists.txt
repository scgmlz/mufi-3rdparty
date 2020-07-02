cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)
include(PreventInSourceBuilds)

project(multifit_3rdparty VERSION 0.1 LANGUAGES CXX)

add_subdirectory(RootMinimizers)
