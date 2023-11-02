cmake_minimum_required(VERSION 3.14)

project(start VERSION 0.1 LANGUAGES CXX)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Core Quick Svg Network)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Core Quick Svg Network)

set(PROJECT_SOURCES
        main.cpp
        qml.qrc
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(start
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET start APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
	add_library(start SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
	add_executable(start
          ${PROJECT_SOURCES}
	  Version.h
	  src/component/CircularReveal.cpp
	  src/component/CircularReveal.h
	  src/component/FileWatcher.cpp
	  src/component/FileWatcher.h
	  src/component/FpsItem.cpp
	  src/component/FpsItem.h
	  src/helper/SettingsHelper.cpp
	  src/helper/SettingsHelper.h
	  src/AppInfo.cpp
	  src/AppInfo.h
	  src/singleton.h
	  src/stdafx.h
	  src/Def.cpp
	  src/Def.h
	  src/FluTextStyle.cpp
	  src/FluTextStyle.h
        )
    endif()
endif()

target_link_libraries(start
  PRIVATE Qt${QT_VERSION_MAJOR}::Core
	  Qt${QT_VERSION_MAJOR}::Quick
	  Qt${QT_VERSION_MAJOR}::Svg
	  Qt${QT_VERSION_MAJOR}::Network
	  Qt${QT_VERSION_MAJOR}::CorePrivate
	  Qt${QT_VERSION_MAJOR}::QuickPrivate
	  Qt${QT_VERSION_MAJOR}::QmlPrivate
	  FramelessHelper::Core
	  FramelessHelper::Quick)


# Qt for iOS sets MACOSX_BUNDLE_GUI_IDENTIFIER automatically since Qt 6.1.
# If you are developing for iOS or macOS you should consider setting an
# explicit, fixed bundle identifier manually though.
if(${QT_VERSION} VERSION_LESS 6.1.0)
  set(BUNDLE_ID_OPTION MACOSX_BUNDLE_GUI_IDENTIFIER com.example.start)
endif()
set_target_properties(start PROPERTIES
    ${BUNDLE_ID_OPTION}
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

include(GNUInstallDirs)
install(TARGETS start
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})

if(QT_VERSION_MAJOR EQUAL 6)
    qt_import_qml_plugins(start)
    qt_finalize_executable(start)
endif()
