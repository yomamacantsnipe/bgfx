CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

if(APPLE)
    project(BGFX CXX OBJCXX)
else()
    project(BGFX CXX)
endif()

set(SRC src/amalgamated.cpp)

include_directories(
        include

        # Dependencies
        ../bx/include
        ../bimg/include
        3rdparty
        3rdparty/khronos
)

add_definitions(-DBX_CONFIG_DEBUG=0)

set(CMAKE_CXX_STANDARD 20)
if(APPLE)
    set(CMAKE_OBJCXX_STANDARD 20)
    set(SRC src/amalgamated.mm)
endif()

add_library(bgfx STATIC ${SRC})
set_target_properties(bgfx PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(bgfx PROPERTIES CXX_STANDARD 20)
set_target_properties(bgfx PROPERTIES CXX_STANDARD_REQUIRED ON)
set_target_properties(bgfx PROPERTIES CXX_EXTENSIONS OFF)

if (APPLE)
    target_compile_options(bgfx PRIVATE -x objective-c++)
elseif (WIN32)
    target_compile_options(bgfx PRIVATE /Zc:__cplusplus /Zc:preprocessor)
    include_directories(../bx/include/compat/msvc)
endif()
