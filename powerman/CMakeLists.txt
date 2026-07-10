project(lingmo-powerman VERSION 1.0.0 DESCRIPTION "Lingmo Power Manager" LANGUAGES CXX)
set(TARGET lingmo-powerman)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(ECM REQUIRED NO_MODULE)
list(APPEND CMAKE_MODULE_PATH ${ECM_MODULE_PATH})

find_package(Qt6 REQUIRED COMPONENTS Core Gui Widgets DBus)
find_package(KF6IdleTime)
find_package(X11)
find_package(XCB
    REQUIRED COMPONENTS
        XCB
        DPMS
)

set(XCB_LIBS
    XCB::XCB
    XCB::DPMS
)

set(SOURCES
    main.cpp
    application.cpp
    lidwatcher.cpp
    action.cpp
    idlemanager.cpp
    dimdisplayaction.cpp
    cpu/cpuitem.cpp
    cpu/cpumanagement.cpp
)

qt_add_dbus_adaptor(DBUS_SOURCES
                     cpu/org.lingmo.CPUManagement.xml
                     cpu/cpumanagement.h CPUManagement)
qt_add_dbus_adaptor(DBUS_SOURCES
                     org.lingmo.PowerManager.xml
                     application.h Application)
qt_add_dbus_interface(DBUS_SOURCES org.freedesktop.ScreenSaver.xml screenlocker_interface)
set_source_files_properties(${DBUS_SOURCES} PROPERTIES SKIP_AUTOGEN ON)

add_executable(${TARGET} ${SOURCES} ${DBUS_SOURCES})
target_link_libraries(${TARGET}
        Qt6::Core
        Qt6::Gui
        Qt6::Widgets
        Qt6::Quick
        Qt6::DBus

        KF6::IdleTime
        ${XCB_LIBS}
        ${X11_LIBRARIES}
)

install(TARGETS ${TARGET} DESTINATION ${CMAKE_INSTALL_BINDIR})
