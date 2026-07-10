find_package(Qt6 COMPONENTS Core Gui REQUIRED)
find_package(X11)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(lingmo-cupdatecursor
  main.cpp
)
target_link_libraries(lingmo-cupdatecursor
    Qt6::Core
    Qt6::Gui
    ${X11_LIBRARIES}
    X11::X11
    X11::Xi
    X11::Xcursor
)

install(TARGETS lingmo-cupdatecursor DESTINATION ${CMAKE_INSTALL_BINDIR})
