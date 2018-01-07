cmake_minimum_required(VERSION 2.8)
PROJECT(TestProject)

#To put all the executables in build directory
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}) 

enable_language(CXX)

#Added to detect compiler support for C++11
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

################################################################################
if (NOT CMAKE_BUILD_TYPE)
  message(STATUS "No build type selected, default to Release")
  set(CMAKE_BUILD_TYPE "Release")
endif()
string(TOUPPER ${CMAKE_BUILD_TYPE} _build_type)

# Setting the C++ compiler to use
set("${CMAKE_CXX_COMPILER_ID}" TRUE CACHE STRING "C++ Compiler")
if (NOT GNU AND NOT Clang AND NOT Intel)
  message(WARNING "Unsupported compiler. Build will likely fail.")
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/cmake/modules)

include(MacroUtilities)
################################################################################

 
FIND_PACKAGE(Qt4 REQUIRED)
FIND_PACKAGE(ROOT REQUIRED)
INCLUDE(${QT_USE_FILE})
#include_directories(${QT_INCLUDE_DIR})

include_directories(${ROOT_INCLUDE_DIR})
 
#QT4_WRAP_UI(UISrcs mainwindow.ui)
#QT4_WRAP_CPP(MOCSrcs mainwindow.h canvas.h)
QT4_WRAP_CPP(MOCSrcs canvas.h)
 
include_directories(${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR})
 
#ADD_EXECUTABLE(MyProject main.cpp mainwindow.cpp ${MOCSrcs} ${UISrcs})
ADD_EXECUTABLE(MyProject main.cxx canvas.cxx ${MOCSrcs}) # ${UISrcs})
TARGET_LINK_LIBRARIES(MyProject ${QT_LIBRARIES} ${ROOT_LIBRARIES})
