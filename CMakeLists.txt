CMAKE_MINIMUM_REQUIRED(VERSION 2.6)

PROJECT( XKB-SWITCH )
SET(MAJOR_VERSION 1)
SET(MINOR_VERSION 5)
SET(RELEASE_VERSION 0)
SET(XKBSWITCH_VERSION ${MAJOR_VERSION}.${MINOR_VERSION}.${RELEASE_VERSION})

FIND_PROGRAM(CTAGS ctags)

OPTION(BUILD_XKBSWITCH_LIB
    "Build a library compatible with vim's libcall interface" ON)
if(BUILD_XKBSWITCH_LIB)
    SET(xkblib xkbswitch)
    ADD_LIBRARY(${xkblib} SHARED src/XKbSwitchApi.cpp src/XKeyboard.cpp)
    SET_TARGET_PROPERTIES(${xkblib} PROPERTIES VERSION ${XKBSWITCH_VERSION} SOVERSION ${MAJOR_VERSION})
    TARGET_LINK_LIBRARIES(${xkblib} X11 xkbfile)
    ADD_EXECUTABLE(xkb-switch src/XKbSwitch.cpp)
    TARGET_LINK_LIBRARIES(xkb-switch ${xkblib})
else()
    ADD_EXECUTABLE(xkb-switch src/XKbSwitch.cpp src/XKeyboard.cpp)
    TARGET_LINK_LIBRARIES(xkb-switch X11 xkbfile)
endif()

ADD_CUSTOM_TARGET(ctags
    COMMAND ${CTAGS} -R ${CMAKE_SOURCE_DIR}/*
)

INSTALL(TARGETS xkb-switch ${xkblib}
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib OPTIONAL
)
