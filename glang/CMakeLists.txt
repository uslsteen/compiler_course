cmake_minimum_required(VERSION 3.16)
project(glang)

include(cmake/subdirs.cmake)
include(cmake/print.cmake)

set(CMAKE_BUILD_TYPE Debug)

set(DIRS 3rd_party src lang)

foreach(DIR ${DIRS})
  add_subdirectory(${DIR})
endforeach()

set(TARGETS)
list(APPEND TARGETS ${LANG_SRC} ${LANG_LIB})

foreach(TARGET IN LISTS TARGETS)
  target_include_directories(${TARGET} PRIVATE ${CMAKE_SOURCE_DIR}/include)
  target_include_directories(${TARGET} PRIVATE ${CMAKE_SOURCE_DIR}/src/grammar)

  target_compile_options(${TARGET} PRIVATE -Wall -Wextra -Wpedantic)
  target_compile_features(${TARGET} PRIVATE cxx_std_20)
endforeach()

foreach(LIB ${LANG_LIB})
  target_link_libraries(${LIB} PRIVATE ${LANG_SRC})
endforeach()

