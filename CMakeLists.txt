cmake_minimum_required(VERSION 3.25)

project(roboflex_util_png VERSION 0.1.1 DESCRIPTION "roboflex util png")

# specify the C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)


# -------------------- 
# Resolve dependencies

include(FetchContent)

# Fetch and make lodepng available
FetchContent_Declare(
  lodepng
  URL https://github.com/lvandeve/lodepng/archive/c18b949b71f45e78b1f9a28c5d458bce0da505d6.tar.gz
)

FetchContent_MakeAvailable(lodepng)

# Specify the source directory
set(LODEPNG_SRC_DIR "${lodepng_SOURCE_DIR}")

message("LODEPNG_SRC_DIR: ${LODEPNG_SRC_DIR}")

# Create the lodepng library
add_library(lodepng STATIC
  ${LODEPNG_SRC_DIR}/lodepng.cpp
)
set_property(TARGET lodepng PROPERTY 
    POSITION_INDEPENDENT_CODE ON
)

# Specify the include directory for the lodepng library
target_include_directories(lodepng PUBLIC ${LODEPNG_SRC_DIR})


# download and build roboflex_core
FetchContent_Declare(roboflex_core
    GIT_REPOSITORY https://github.com/flexrobotics/roboflex.git
    GIT_TAG        main
)
set(BUILD_ROBOFLEX_PYTHON_EXT OFF CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(roboflex_core)


# -------------------- 
# Define the library

add_library(roboflex_util_png STATIC
    src/png.cpp
    include/roboflex_util_png/png.h
)

# Set some properties on our library
set_property(TARGET roboflex_util_png PROPERTY 
    POSITION_INDEPENDENT_CODE ON
)

# Include directories when we compile our library
target_include_directories(roboflex_util_png PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> 
    $<INSTALL_INTERFACE:include>
    ${LODEPNG_SRC_DIR}
)

# Link against the necessary libraries
target_link_libraries(roboflex_util_png PUBLIC 
    roboflex_core 
    lodepng
)


# -------------------- 
# install

# If you need to install the util_png library
install(TARGETS roboflex_util_png 
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)
#install(FILES png.h DESTINATION include/roboflex_util_png)
install(DIRECTORY include/roboflex_util_png
    DESTINATION include
)


# --------------------
# build python bindings

add_subdirectory(python)
