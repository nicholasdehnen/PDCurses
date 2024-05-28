cmake_minimum_required(VERSION 3.10)
project(PDCurses)

# Source directory (top-level, this directory)
set(PDCURSES_SRCDIR ${CMAKE_SOURCE_DIR})

# Set OSDIR based on operating system
if(WIN32)
    set(OSDIR ${PDCURSES_SRCDIR}/wincon)
    set_property(GLOBAL PROPERTY USE_FOLDERS ON) # Use folders for VS
else()
    message(FATAL_ERROR "Only Windows (wincon) builds are supported at the moment.")
endif()

# Directories
set(COMMON ${PDCURSES_SRCDIR}/common)
set(SRCDIR ${PDCURSES_SRCDIR}/pdcurses)
set(DEMODIR ${PDCURSES_SRCDIR}/demos)

# Files
set(PDCURSES_CURSES_H ${PDCURSES_SRCDIR}/curses.h)
set(PDCURSES_CURSPRIV_H ${PDCURSES_SRCDIR}/curspriv.h)
set(PDCURSES_HEADERS ${PDCURSES_CURSES_H} ${PDCURSES_CURSPRIV_H})
set(PANEL_HEADER ${PDCURSES_SRCDIR}/panel.h)

# Common elements
set(LIBOBJS
    ${SRCDIR}/addch.c ${SRCDIR}/addchstr.c ${SRCDIR}/addstr.c ${SRCDIR}/attr.c
    ${SRCDIR}/beep.c ${SRCDIR}/bkgd.c ${SRCDIR}/border.c ${SRCDIR}/clear.c
    ${SRCDIR}/color.c ${SRCDIR}/delch.c ${SRCDIR}/deleteln.c ${SRCDIR}/getch.c
    ${SRCDIR}/getstr.c ${SRCDIR}/getyx.c ${SRCDIR}/inch.c ${SRCDIR}/inchstr.c
    ${SRCDIR}/initscr.c ${SRCDIR}/inopts.c ${SRCDIR}/insch.c ${SRCDIR}/insstr.c
    ${SRCDIR}/instr.c ${SRCDIR}/kernel.c ${SRCDIR}/keyname.c ${SRCDIR}/mouse.c
    ${SRCDIR}/move.c ${SRCDIR}/outopts.c ${SRCDIR}/overlay.c ${SRCDIR}/pad.c
    ${SRCDIR}/panel.c ${SRCDIR}/printw.c ${SRCDIR}/refresh.c ${SRCDIR}/scanw.c
    ${SRCDIR}/scr_dump.c ${SRCDIR}/scroll.c ${SRCDIR}/slk.c ${SRCDIR}/termattr.c
    ${SRCDIR}/touch.c ${SRCDIR}/util.c ${SRCDIR}/window.c ${SRCDIR}/debug.c
)
set(PDCOBJS
    ${OSDIR}/pdcclip.c ${OSDIR}/pdcdisp.c ${OSDIR}/pdcgetsc.c ${OSDIR}/pdckbd.c
    ${OSDIR}/pdcscrn.c ${OSDIR}/pdcsetsc.c ${OSDIR}/pdcutil.c
)
set(DEMOS
    ${DEMODIR}/testcurs.c ${DEMODIR}/ozdemo.c ${DEMODIR}/xmas.c ${DEMODIR}/tuidemo.c
    ${DEMODIR}/firework.c ${DEMODIR}/ptest.c ${DEMODIR}/rain.c ${DEMODIR}/worm.c
)

# build / add subdir based on operating system
if(WIN32)
    add_subdirectory(wincon)
endif()