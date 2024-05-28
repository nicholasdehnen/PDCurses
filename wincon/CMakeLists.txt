# Options to control the build
option(DLL "Build as a DLL" OFF)
option(WIDE "Enable wide-character support" OFF)
option(UTF8 "Force UTF-8 encoding" OFF)
 # Use internal INFOEX if on. Usually defined in consoleapi2.h on modern Windows, therefore default off.
option(INFOEX "Have INFOEX" ON)

# Include directories
include_directories(${PDCURSES_SRCDIR})
include_directories(${DEMODIR})

# Defines
set(PDCURSES_WIN_H ${OSDIR}/pcdwin.h)

# Compiler flags
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -Wall -DPDCDEBUG")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -Wall -DPDCDEBUG")
else()
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -Wall")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -Wall")
endif()

if(WIDE)
    add_definitions(-DPDC_WIDE)
endif()

if(UTF8)
    add_definitions(-DPDC_FORCE_UTF8)
endif()

if(NOT INFOEX)
    add_definitions(-DHAVE_NO_INFOEX)
endif()

# Build library
if(DLL)
    add_definitions(-DPDC_DLL_BUILD)
    add_library(pdcurses SHARED ${LIBOBJS} ${PDCOBJS} ${COMMON}/pdcurses.rc)
    set_target_properties(pdcurses PROPERTIES OUTPUT_NAME "pdcurses")
else()
    add_library(pdcurses STATIC ${LIBOBJS} ${PDCOBJS})
endif()

# Build demos
foreach(demo ${DEMOS})
    get_filename_component(demo_name ${demo} NAME_WE)
    if(${demo_name} STREQUAL "tuidemo")
        set(demo ${demo} ${DEMODIR}/tui.c)
    endif()
    add_executable(${demo_name} ${demo})
    target_link_libraries(${demo_name} pdcurses)
    set_target_properties(${demo_name} PROPERTIES FOLDER "demos")
endforeach()

# Custom targets
add_custom_target(demos ALL DEPENDS ${DEMOS})
add_custom_target(clean-all
    COMMAND ${CMAKE_COMMAND} -E remove *.o *.exe *.dll *.lib
)

# Install rules
install(TARGETS pdcurses DESTINATION lib)
install(FILES ${PDCURSES_SRCDIR}/curses.h ${PDCURSES_SRCDIR}/curspriv.h ${PDCURSES_SRCDIR}/panel.h DESTINATION include)
