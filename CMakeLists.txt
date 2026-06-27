cmake_minimum_required(VERSION 3.16)
project(LudoGame LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# ─────────────────────────────────────────────────────────────────
# Automatically download & build raylib if it isn't found on the
# system. This means contributors don't have to manually install
# raylib before they can compile the game.
# ─────────────────────────────────────────────────────────────────
find_package(raylib 5.0 QUIET)

if (NOT raylib_FOUND)
    include(FetchContent)
    set(FETCHCONTENT_QUIET FALSE)

    FetchContent_Declare(
        raylib
        GIT_REPOSITORY https://github.com/raysan5/raylib.git
        GIT_TAG        5.5
    )

    set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
    set(BUILD_GAMES    OFF CACHE BOOL "" FORCE)

    FetchContent_MakeAvailable(raylib)
endif()

# ─────────────────────────────────────────────────────────────────
# Source files
# ─────────────────────────────────────────────────────────────────
file(GLOB SOURCES "${CMAKE_SOURCE_DIR}/LUDO_FIXED/*.cpp")

add_executable(ludo ${SOURCES})
target_link_libraries(ludo PRIVATE raylib)

# ─────────────────────────────────────────────────────────────────
# Platform-specific linker flags (mirrors raylib's own requirements)
# ─────────────────────────────────────────────────────────────────
if (APPLE)
    target_link_libraries(ludo PRIVATE "-framework IOKit" "-framework Cocoa" "-framework OpenGL")
elseif (WIN32)
    target_link_libraries(ludo PRIVATE opengl32 gdi32 winmm)
elseif (UNIX)
    target_link_libraries(ludo PRIVATE GL m pthread dl rt X11)
endif()

# Copy the executable's working directory expectations (e.g. ludo_save.txt)
set_target_properties(ludo PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
)
