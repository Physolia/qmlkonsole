# SPDX-FileCopyrightText: 2022 Nicolas Fella <nicolas.fella@gmx.de>
# SPDX-License-Identifier: GPL-2.0-or-later

cmake_minimum_required(VERSION 3.16)

set(RELEASE_SERVICE_VERSION_MAJOR "24")
set(RELEASE_SERVICE_VERSION_MINOR "01")
set(RELEASE_SERVICE_VERSION_MICRO "75")
set(RELEASE_SERVICE_VERSION "${RELEASE_SERVICE_VERSION_MAJOR}.${RELEASE_SERVICE_VERSION_MINOR}.${RELEASE_SERVICE_VERSION_MICRO}")

project(qmlkonsole VERSION ${RELEASE_SERVICE_VERSION})

set(KF6_MIN_VERSION "5.240.0")
set(QT_MIN_VERSION "6.5.2")
set(KDE_COMPILERSETTINGS_LEVEL "5.84.0")

find_package(ECM ${KF6_MIN_VERSION} REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${ECM_MODULE_PATH})

include(ECMSetupVersion)
include(ECMGenerateHeaders)
include(KDEInstallDirs)
include(KDECMakeSettings)
include(ECMPoQmTools)
include(KDECompilerSettings NO_POLICY_SCOPE)
include(ECMFindQmlModule)

ecm_setup_version(${RELEASE_SERVICE_VERSION}
    VARIABLE_PREFIX QMLKONSOLE
    VERSION_HEADER ${CMAKE_CURRENT_BINARY_DIR}/version.h
)

find_package(Qt6 ${QT_MIN_VERSION} REQUIRED NO_MODULE COMPONENTS
    Core 
    Quick 
    Test 
    Gui 
    Svg 
    QuickControls2
    Widgets
)
find_package(KF6 ${KF6_MIN_VERSION} REQUIRED COMPONENTS
    Kirigami2 
    I18n 
    Config 
    CoreAddons 
    WindowSystem
)
find_package(KF6KirigamiAddons 0.6 REQUIRED)

ecm_find_qmlmodule(QMLTermWidget 1.0)
set_package_properties(QMLTermWidget-QMLModule PROPERTIES
    URL "https://github.com/Swordfish90/qmltermwidget")

add_subdirectory(src)

ki18n_install(po)

install(PROGRAMS org.kde.qmlkonsole.desktop DESTINATION ${KDE_INSTALL_APPDIR})
install(FILES org.kde.qmlkonsole.appdata.xml DESTINATION ${KDE_INSTALL_METAINFODIR})
install(FILES org.kde.qmlkonsole.svg DESTINATION ${KDE_INSTALL_FULL_ICONDIR}/hicolor/scalable/apps)

feature_summary(WHAT ALL INCLUDE_QUIET_PACKAGES FATAL_ON_MISSING_REQUIRED_PACKAGES)
