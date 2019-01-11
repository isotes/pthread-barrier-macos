cmake_minimum_required(VERSION 3.5)
project(pthread-barrier-macos VERSION 1.0 LANGUAGES C)

find_package(Threads REQUIRED)

add_library(pthread-barrier-macos src/pthread_barrier.c)
target_compile_features(pthread-barrier-macos PRIVATE c_std_99)
target_compile_options(pthread-barrier-macos PRIVATE -Wall -Wextra)
target_link_libraries(pthread-barrier-macos PUBLIC Threads::Threads)
target_include_directories(pthread-barrier-macos PUBLIC
	$<INSTALL_INTERFACE:inc>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/inc>)
