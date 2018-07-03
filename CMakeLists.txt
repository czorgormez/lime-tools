########################################################################
## Create lime-suite project
########################################################################
cmake_minimum_required(VERSION 3.0.2)
set(CMAKE_CONFIGURATION_TYPES "Debug;Release;RelWithDebInfo" CACHE TYPE INTERNAL FORCE)
project("Lime-tools")

########################################################################
## compiler flags
########################################################################
if(CMAKE_COMPILER_IS_GNUCXX)
    add_compile_options(-Ofast -march=native -ftree-vectorize -funsafe-math-optimizations)
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

find_package(Gnuplot REQUIRED)
find_package(FFTW3 REQUIRED)
find_package(LimeSuite REQUIRED)

message(STATUS "FFTW3 LIBRARIES: " ${FFTW3_LIBRARIES})
message(STATUS "FFTW3 INCLUDE DIRS: " ${FFTW3_INCLUDE_DIR})
message(STATUS "LIMESUITE LIBRARIES: " ${LIMESUITE_LIBRARIES})
message(STATUS "LIMESUITE INCLUDE DIRS: " ${LIMESUITE_INCLUDE_DIR})

include_directories(${LIMESUITE_INCLUDE_DIR} ${FFTW3_INCLUDE_DIR})

add_executable(LimeMon limemon/src/LimeMon.cpp)
target_link_libraries(LimeMon m ${LIMESUITE_LIBRARIES} ${FFTW3_LIBRARIES})
install(TARGETS LimeMon DESTINATION bin)

add_executable(LimeScan limescan/src/LimeScan.cpp)
target_link_libraries(LimeScan m ${LIMESUITE_LIBRARIES} ${FFTW3_LIBRARIES})
install(TARGETS LimeScan DESTINATION bin)

########################################################################
# uninstall target
########################################################################
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)

add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)


