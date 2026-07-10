project(lingmo-screen-brightness VERSION 1.0.0 DESCRIPTION "Lingmo Screen Brightness" LANGUAGES CXX)
set(PROJECT_NAME lingmo-screen-brightness)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(SRCS
    main.cpp
    brightnesshelper.h
    brightnesshelper.cpp
)

find_package(Qt6 REQUIRED COMPONENTS Core Gui Widgets)

add_executable(${PROJECT_NAME}
    ${SRCS}
)

target_link_libraries(${PROJECT_NAME}
        Qt6::Core
        Qt6::Gui
        Qt6::Widgets
)

configure_file(
    com.lingmo.brightness.pkexec.policy.in
    com.lingmo.brightness.pkexec.policy
    @ONLY
)

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/com.lingmo.brightness.pkexec.policy DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/polkit-1/actions/)
