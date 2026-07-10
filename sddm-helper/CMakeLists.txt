project(lingmo-sddm-helper VERSION 1.0.0 DESCRIPTION "Lingmo SDDM Helper" LANGUAGES CXX)
set(PROJECT_NAME lingmo-sddm-helper)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt6 REQUIRED COMPONENTS Core Gui)

set(SRCS
    main.cpp
)

add_executable(${PROJECT_NAME}
    ${SRCS}
)

target_link_libraries(${PROJECT_NAME}
        Qt6::Core
        Qt6::Gui
)

configure_file(
    com.lingmo.sddm.helper.pkexec.policy.in
    com.lingmo.sddm.helper.pkexec.policy
    @ONLY
)

install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/com.lingmo.sddm.helper.pkexec.policy DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/polkit-1/actions/)
