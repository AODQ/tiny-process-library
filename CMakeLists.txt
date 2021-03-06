cmake_minimum_required(VERSION 2.8)

project(tiny-process-library)

add_library(tiny-process-library STATIC)

target_sources(
  tiny-process-library
  PRIVATE
    process.cpp
)

if(PULCHER_PLATFORM STREQUAL "Win64")
  target_sources(tiny-process-library PRIVATE process_win.cpp)
  #If compiled using MSYS2, use sh to run commands
  if(MSYS)
    target_compile_definitions(tiny-process-library PUBLIC MSYS_PROCESS_USE_SH)
  endif()
else()
  target_sources(tiny-process-library PRIVATE process_unix.cpp)
endif()

find_package(Threads REQUIRED)

target_link_libraries(tiny-process-library ${CMAKE_THREAD_LIBS_INIT})
target_include_directories(
  tiny-process-library PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include
)

install(TARGETS tiny-process-library DESTINATION lib)
