project(lingmo-settings-daemon VERSION 1.0.0 DESCRIPTION "Lingmo Settings Daemon" LANGUAGES CXX)
set(TARGET lingmo-settings-daemon)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(ECM REQUIRED NO_MODULE)
list(APPEND CMAKE_MODULE_PATH ${ECM_MODULE_PATH})

find_package(Qt6 REQUIRED COMPONENTS Core Gui Widgets DBus Xml)
find_package(X11 REQUIRED)
find_package(X11_XCB REQUIRED)
find_package(XCB REQUIRED)

pkg_check_modules(XORGLIBINPUT xorg-libinput IMPORTED_TARGET)
pkg_check_modules(XORGSERVER xorg-server IMPORTED_TARGET)
pkg_check_modules(SYNAPTICS xorg-synaptics IMPORTED_TARGET GLOBAL)

file (GLOB_RECURSE SRCS
  "*.cpp"
  "*.h"
  "theme/*.h"
  "theme/*.cpp"
  "brightness/*.h"
  "brightness/*.cpp"
  "brightness/*.c"
  "battery/*.cpp"
  "battery/*.h"
  "language/*.cpp"
  "language/*.h"
  "dock/*.cpp"
  "dock/*.h"
  "mouse/*.h"
  "mouse/*.cpp"
  "touchpad/*.h"
  "touchpad/*.cpp"
  "touchpad/x11/*.h"
  "touchpad/x11/*.cpp"
)

set(SOURCES ${SRCS})
set(HEADERS "")
set(FORMS "")
set(RESOURCES "")

qt_add_dbus_adaptor(DBUS_SOURCES
                     brightness/org.lingmo.Brightness.xml
                     brightness/brightnessmanager.h BrightnessManager)
qt_add_dbus_adaptor(DBUS_SOURCES
                     theme/org.lingmo.Theme.xml
                     theme/thememanager.h ThemeManager)
qt_add_dbus_adaptor(DBUS_SOURCES
                     battery/org.lingmo.PrimaryBattery.xml
                     battery/battery.h Battery)
qt_add_dbus_adaptor(DBUS_SOURCES
                     language/org.lingmo.Language.xml
                     language/language.h Language)
qt_add_dbus_adaptor(DBUS_SOURCES
                     dock/org.lingmo.Dock.xml
                     dock/dock.h Dock)
qt_add_dbus_adaptor(DBUS_SOURCES
                     mouse/org.lingmo.Mouse.xml
                     mouse/mousemanager.h Mouse)
qt_add_dbus_adaptor(DBUS_SOURCES
                     touchpad/org.lingmo.Touchpad.xml
                     touchpad/touchpadmanager.h TouchpadManager)
set_source_files_properties(${DBUS_SOURCES} PROPERTIES SKIP_AUTOGEN ON)

add_executable(${TARGET} ${SOURCES} ${DBUS_SOURCES} ${HEADERS} ${FORMS} ${RESOURCES})

file(GLOB TS_FILES translations/*.ts)
foreach(filepath ${TS_FILES})
    string(REPLACE "${CMAKE_CURRENT_SOURCE_DIR}/" "" filename ${filepath})
    list(APPEND ts_files_replaced ${filename})
endforeach()
qt_create_translation(QM_FILES ${CMAKE_CURRENT_SOURCE_DIR} ${ts_files_replaced})

target_link_libraries(${TARGET}
        Qt6::Core
        Qt6::Gui
        Qt6::Widgets
        Qt6::DBus
        Qt6::Xml
        ${X11_LIBRARIES}
        X11::X11
        X11::Xi
        X11::XCB
        ${XCB_LIBRARIES}
        PkgConfig::XORGLIBINPUT
        PkgConfig::XORGSERVER
        PkgConfig::SYNAPTICS
)

add_custom_target(translations DEPENDS ${QM_FILES} SOURCES ${ts_files_replaced})
add_dependencies(${TARGET} translations)

install(TARGETS ${TARGET} DESTINATION ${CMAKE_INSTALL_BINDIR})
install(FILES ${QM_FILES} DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${TARGET}/translations)
