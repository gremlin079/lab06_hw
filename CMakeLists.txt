cmake_minimum_required(VERSION 3.10)
project(lab06_homework)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

include(GNUInstallDirs)

add_subdirectory(formatter_lib)
add_subdirectory(formatter_ex_lib)
add_subdirectory(solver_lib)
add_subdirectory(solver_application)
add_subdirectory(hello_world_application)

set(CPACK_PACKAGE_NAME "solver")
set(CPACK_PACKAGE_VERSION "1.0.0")
set(CPACK_PACKAGE_VENDOR "gremlin079")
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Solver application package")

if(UNIX AND NOT APPLE)
    set(CPACK_GENERATOR "DEB;RPM")
    set(CPACK_DEBIAN_PACKAGE_MAINTAINER "gremlin079")
    set(CPACK_DEBIAN_PACKAGE_DEPENDS "libc6 (>= 2.17)")
    set(CPACK_RPM_PACKAGE_RELEASE 1)
    set(CPACK_RPM_PACKAGE_REQUIRES "libstdc++")
    set(CPACK_PACKAGING_INSTALL_PREFIX "/usr")
    install(TARGETS solver hello_world
            RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
            COMPONENT applications)
    
    install(TARGETS formatter formatter_ex solver_lib
            LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
            COMPONENT libraries)
    set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
    set(CPACK_DEBIAN_DEBUGINFO_PACKAGE ON)  
endif()

if(WIN32)
    set(CPACK_GENERATOR "NSIS")
    set(CPACK_NSIS_MODIFY_PATH ON)
    set(CPACK_NSIS_ENABLE_UNINSTALL_BEFORE_INSTALL ON)
    
    install(TARGETS solver hello_world
            RUNTIME DESTINATION bin
            COMPONENT applications)
            
    install(TARGETS formatter formatter_ex solver_lib
            LIBRARY DESTINATION lib
            COMPONENT libraries)
endif()

if(APPLE)
    set(CPACK_GENERATOR "Bundle")
    set(CPACK_BUNDLE_NAME "Solver")
    set(CPACK_BUNDLE_PLIST "${CMAKE_CURRENT_SOURCE_DIR}/Info.plist")
    
    if(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/icon.icns")
        set(CPACK_BUNDLE_ICON "${CMAKE_CURRENT_SOURCE_DIR}/icon.icns")
    else()
        message(WARNING "icon.icns not found - bundle will be created without icon")
    endif()

    install(TARGETS solver hello_world
            BUNDLE DESTINATION Applications
            COMPONENT applications)
endif()

include(CPack)
