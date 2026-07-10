project(lingmo-cpufreq VERSION 1.0.0 DESCRIPTION "Lingmo CPU Frequency" LANGUAGES CXX)
set(TARGET lingmo-cpufreq)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(SOURCES
    main.cpp
)

find_package(Qt6 REQUIRED COMPONENTS Core Quick DBus)

add_executable(${TARGET} ${SOURCES} ${DBUS_SOURCES})
target_link_libraries(${TARGET}
        Qt6::Core
        Qt6::Quick
        Qt6::DBus
)

configure_file(
    com.lingmo.cpufreq.pkexec.policy.in
    com.lingmo.cpufreq.pkexec.policy
    @ONLY
)

install(TARGETS ${TARGET} DESTINATION ${CMAKE_INSTALL_BINDIR})
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/com.lingmo.cpufreq.pkexec.policy DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/polkit-1/actions/)
