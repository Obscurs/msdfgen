cmake_minimum_required(VERSION 3.10)
project(FreetypeAndMsdfgen)

# Global configurations
set(CMAKE_C_STANDARD 99)  # Default C standard
set(CMAKE_CXX_STANDARD 17)  # Default C++ standard

# Variable to specify the output directory
set(OUTPUT_DIR ${CMAKE_BINARY_DIR}/output)

file(GLOB SRC_FREETYPE_BASE "${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/*.c")
file(GLOB HEADER_FREETYPE_INC_FREETYPE "${CMAKE_CURRENT_SOURCE_DIR}/freetype/include/freetype/internal/*.h")
file(GLOB HEADER_FREETYPE_INC_FREETYPE_INTERNAL "${CMAKE_CURRENT_SOURCE_DIR}/freetype/include/freetype/internal/*.h")

# Freetype library configuration
add_library(freetype STATIC
    # Header files
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/include/ft2build.h
    ${HEADER_FREETYPE_INC_FREETYPE}
    ${HEADER_FREETYPE_INC_FREETYPE_INTERNAL}
    # Source files
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/autofit/autofit.c
    ${SRC_FREETYPE_BASE}
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/bdf/bdf.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/bzip2/ftbzip2.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/cache/ftcache.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/cff/cff.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/cid/type1cid.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/gzip/ftgzip.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/lzw/ftlzw.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/pcf/pcf.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/pfr/pfr.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/psaux/psaux.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/pshinter/pshinter.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/psnames/psnames.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/raster/raster.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/sdf/sdf.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/sfnt/sfnt.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/smooth/smooth.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/truetype/truetype.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/type1/type1.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/type42/type42.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/winfonts/winfnt.c
)

target_include_directories(freetype PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/freetype/include"
)

target_compile_definitions(freetype PUBLIC FT2_BUILD_LIBRARY _CRT_SECURE_NO_WARNINGS _CRT_NONSTDC_NO_WARNINGS)

file(GLOB SRC_MSDFGEN
    ${CMAKE_CURRENT_SOURCE_DIR}/core/**.h
    ${CMAKE_CURRENT_SOURCE_DIR}/core/**.hpp
    ${CMAKE_CURRENT_SOURCE_DIR}/core/**.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/ext/**.h
    ${CMAKE_CURRENT_SOURCE_DIR}/ext/**.hpp
    ${CMAKE_CURRENT_SOURCE_DIR}/ext/**.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/lib/**.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/include/**.h
 )
# Msdfgen library configuration
add_library(msdfgen STATIC ${SRC_MSDFGEN})

target_include_directories(msdfgen PUBLIC include freetype/include)
target_compile_definitions(msdfgen PUBLIC MSDFGEN_USE_CPP11)
target_link_libraries(msdfgen PRIVATE freetype)

# Set up output directories and compile options for different build types
foreach(TARGET freetype msdfgen)
    set_target_properties(${TARGET} PROPERTIES
        ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${OUTPUT_DIR}/debug/${TARGET}
        ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${OUTPUT_DIR}/release/${TARGET}
        ARCHIVE_OUTPUT_DIRECTORY_MINSIZEREL ${OUTPUT_DIR}/dist/${TARGET}
        LIBRARY_OUTPUT_DIRECTORY_DEBUG ${OUTPUT_DIR}/debug/${TARGET}
        LIBRARY_OUTPUT_DIRECTORY_RELEASE ${OUTPUT_DIR}/release/${TARGET}
        LIBRARY_OUTPUT_DIRECTORY_MINSIZEREL ${OUTPUT_DIR}/dist/${TARGET}
        RUNTIME_OUTPUT_DIRECTORY_DEBUG ${OUTPUT_DIR}/debug/${TARGET}
        RUNTIME_OUTPUT_DIRECTORY_RELEASE ${OUTPUT_DIR}/release/${TARGET}
        RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL ${OUTPUT_DIR}/dist/${TARGET}
    )
endforeach()