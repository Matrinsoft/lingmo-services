cmake_minimum_required(VERSION 3.20)
project(lingmo-services VERSION 1.0.0 DESCRIPTION "Lingmo Services" LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake/)

find_package(Qt6 REQUIRED COMPONENTS Core Gui Widgets Quick QuickControls2 DBus Xml)
find_package(PkgConfig REQUIRED)

include(GNUInstallDirs)

add_subdirectory(polkit-agent)
add_subdirectory(screen-brightness)
add_subdirectory(session)
add_subdirectory(settings-daemon)
add_subdirectory(shutdown-ui)
add_subdirectory(xembed-sni-proxy)
add_subdirectory(powerman)
add_subdirectory(cpufreq)
add_subdirectory(cupdatecursor)
add_subdirectory(gmenuproxy)
add_subdirectory(notificationd)
add_subdirectory(sddm-helper)
add_subdirectory(clipboard)
add_subdirectory(chotkeys)
