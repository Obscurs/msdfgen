cmake_minimum_required(VERSION 3.10)
project(FreetypeAndMsdfgen)

# Global configurations
set(CMAKE_C_STANDARD 99)  # Default C standard
set(CMAKE_CXX_STANDARD 17)  # Default C++ standard

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
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftbase.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftbbox.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftbdf.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftbitmap.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftcid.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftfstype.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftgasp.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftglyph.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftgxval.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftinit.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftmm.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftotval.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftpatent.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftpfr.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftstroke.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftsynth.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/fttype1.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftwinfnt.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftsystem.c
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/base/ftdebug.c
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
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/src/svg/svg.c
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