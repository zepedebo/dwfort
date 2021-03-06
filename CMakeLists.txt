cmake_minimum_required(VERSION 2.8)
project(dwfort)

include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${CMAKE_CURRENT_SOURCE_DIR}/client
  ${CMAKE_CURRENT_SOURCE_DIR}/client/forms
  ${CMAKE_CURRENT_SOURCE_DIR}/client/src
  ${CMAKE_CURRENT_SOURCE_DIR}/common
  ${CMAKE_CURRENT_SOURCE_DIR}/common/log
  ${CMAKE_CURRENT_SOURCE_DIR}/test
  ${CMAKE_CURRENT_SOURCE_DIR}/log
)

set(EXTRA_COMPILE_FLAGS "-Wall -g -std=c++0x")
set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} ${EXTRA_COMPILE_FLAGS})
set(CMAKE_EXE_LINKER_FLAGS ${CMAKE_EXE_LINKER_FLAGS})

set(client_srcs
  client/src/main.cpp
  client/src/wxwidget.h
  client/forms/MainWindow.cpp
  client/forms/MainWindow.h
)
source_group("client" FILES ${client_srcs})

set(common_srcs
  common/sample.h
  common/sample.cpp
  common/log/logger.h
  common/log/logger.cpp
)
source_group("common" FILES ${common_srcs})

set(test_srcs
  test/main.cpp
  test/catch/catch.hpp
  test/unit/sample.cpp
)
source_group("test" FILES ${test_srcs})

set(wxWidgets_CONFIGURATION mswu)
find_package(wxWidgets COMPONENTS core base REQUIRED)
include(${wxWidgets_USE_FILE})


add_library(common SHARED ${common_srcs})

add_executable(dwfort ${client_srcs})
add_executable(dwfort_test ${test_srcs})

target_link_libraries(dwfort common ${wxWidgets_LIBRARIES})
target_link_libraries(dwfort_test common)

enable_testing()
add_test(unit_tests dwfort_test)

# Copy log directory
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/log/log.conf DESTINATION ${CMAKE_CURRENT_BINARY_DIR}/log)
