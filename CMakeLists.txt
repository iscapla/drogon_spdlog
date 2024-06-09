#####################################################################################################
# PROJECT INFORMATION
#####################################################################################################

cmake_minimum_required(VERSION 3.10...3.27 FATAL_ERROR)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++20")
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++20 -D_NATIVE_WCHAR_T_DEFINED=ON")
# Define UNICODE and _UNICODE for wide-character support
# add_definitions(-DUNICODE -D_UNICODE)

# # Ensure wchar_t support is enabled
# add_definitions(-D_WCHAR_T)

project (
  DrogonDebug
  DESCRIPTION       "DrogonDebug"
  VERSION           1.0.1
  LANGUAGES CXX
  )

set(CMAKE_MAIN_PROJECT_NAME ${PROJECT_NAME})

include(FetchContent)
include(CMakePrintHelpers)


################################
# FMT
################################

FetchContent_Declare(
  fmt
  GIT_REPOSITORY https://github.com/fmtlib/fmt.git
  GIT_TAG        10.2.1
  OVERRIDE_FIND_PACKAGE
)
if(WIN32)
  set(FMT_USE_WINDOWS_H ON)
endif()
FetchContent_MakeAvailable(fmt)

################################
# SPDLOG
################################

FetchContent_Declare(
  spdlog
  GIT_REPOSITORY https://github.com/gabime/spdlog.git
  GIT_TAG        v1.14.1
  OVERRIDE_FIND_PACKAGE
)
set(SPDLOG_INSTALL ON)
set(SPDLOG_COMPILED_LIB ON)
set(SPDLOG_BUILD_EXAMPLES OFF)  # Example: Disable building examples
set(SPDLOG_BUILD_TESTS OFF)     # Example: Disable building tests
set(SPDLOG_FMT_EXTERNAL ON)
set(spdlog_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(spdlog)

################################
# DROGON
################################

FetchContent_Declare(
  drogon
  GIT_REPOSITORY https://github.com/drogonframework/drogon.git
  GIT_TAG        v1.9.4
)
set(BUILD_CTL OFF CACHE BOOL "" FORCE)
set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(BUILD_ORM OFF CACHE BOOL "" FORCE)
set(USE_SUBMODULE ON CACHE BOOL "" FORCE)
set(USE_SPDLOG ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(drogon)
# target_compile_definitions(drogon PUBLIC UNICODE _UNICODE)

add_executable(${PROJECT_NAME} main.cpp)

target_link_libraries(${PROJECT_NAME} PRIVATE drogon spdlog)

install(TARGETS ${PROJECT_NAME} DESTINATION bin)