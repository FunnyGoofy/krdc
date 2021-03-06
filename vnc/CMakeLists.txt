
if(LIBVNCSERVER_FOUND)

    include_directories(
        ${CMAKE_CURRENT_BINARY_DIR}
        ${LIBVNCCLIENT_INCLUDE_DIR}
    )

    set(vncplugin_SRCS
        vnchostpreferences.cpp
        vncclientthread.cpp
        vncviewfactory.cpp
        vncview.cpp
    )

    ecm_qt_declare_logging_category(vncplugin_SRCS HEADER krdc_debug.h IDENTIFIER KRDC CATEGORY_NAME KRDC)

    ki18n_wrap_ui(vncplugin_SRCS
        vncpreferences.ui
    )

    add_library(krdc_vncplugin ${vncplugin_SRCS})
    target_link_libraries(krdc_vncplugin
        Qt5::Core
        KF5::ConfigCore
        KF5::CoreAddons
        KF5::XmlGui
        Qt5::Gui
        ${LIBVNCCLIENT_LIBRARIES}
        krdccore
    )

    if(JPEG_FOUND)
        target_link_libraries(krdc_vncplugin ${JPEG_LIBRARIES})
    endif(JPEG_FOUND)

    set(kcm_krdc_vncplugin_SRCS
        vncpreferences.cpp
    )

    add_library(kcm_krdc_vncplugin ${kcm_krdc_vncplugin_SRCS})

    target_link_libraries(kcm_krdc_vncplugin
        Qt5::Core
        KF5::Wallet
        KF5::ConfigCore
        KF5::CoreAddons
        KF5::XmlGui
        Qt5::Gui
        ${LIBVNCCLIENT_LIBRARIES}
        krdccore
    )

    kcoreaddons_desktop_to_json(krdc_vncplugin krdc_vnc.desktop)
    kcoreaddons_desktop_to_json(kcm_krdc_vncplugin krdc_vnc_config.desktop)

    add_dependencies(kcm_krdc_vncplugin krdc_vncplugin)

    install(TARGETS kcm_krdc_vncplugin DESTINATION ${PLUGIN_INSTALL_DIR}/krdc/kcms)
    install(TARGETS krdc_vncplugin DESTINATION ${PLUGIN_INSTALL_DIR}/krdc)

    install(FILES krdc_vnc_config.desktop DESTINATION ${SERVICES_INSTALL_DIR})

    install(FILES vnc.protocol DESTINATION ${SERVICES_INSTALL_DIR})
endif(LIBVNCSERVER_FOUND)
