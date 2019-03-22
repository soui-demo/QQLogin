#
# 1394020320@qq.com
#

cmake_minimum_required(VERSION 3.4.3)
project(QQLogin)
file (TO_CMAKE_PATH "$ENV{SOUIPATH}" SOUI_ROOT)

message(STATUS "SOUI_ROOT: ${SOUI_ROOT}")
if ("${SOUI_ROOT}" STREQUAL "")
	message(FATAL_ERROR "FATAL:can't find env variable SOUIPATH, clone soui core and install wizard first, please.")
endif()

include(${SOUI_ROOT}/__cmake/cotire.cmake)
include(${SOUI_ROOT}/__cmake/internal_utils.cmake)
include(${SOUI_ROOT}/__cmake/global.cmake)

readSettingFile(UNICODE "1" USE_UNICODE)
readSettingFile(WCHAR "1" WCHAR_AS_DEFAULT)
readSettingFile(MT "0" SHARED_CRT)
readSettingFile(SUPPORT_XP "1" XP_TOOLSET)

#message(STATUS "USE_UNICODE: ${USE_UNICODE}")
#message(STATUS "WCHAR_AS_DEFAULT: ${WCHAR_AS_DEFAULT}")
#message(STATUS "SHARED_CRT: ${SHARED_CRT}")
#message(STATUS "XP_TOOLSET: ${XP_TOOLSET}")

config_compiler_and_linker()

add_definitions(-D_CRT_SECURE_NO_WARNINGS -D_CRT_NON_CONFORMING_SWPRINTFS)

include_directories(${SOUI_ROOT}/config)
include_directories(${SOUI_ROOT}/components)
include_directories(${SOUI_ROOT}/utilities/include)
include_directories(${SOUI_ROOT}/SOUI/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/extend.ctrls)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/extend.events)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/extend.skins)

file(GLOB_RECURSE CURRENT_HEADERS  *.h *.hpp)
file(GLOB_RECURSE CURRENT_SRCS  *.cpp)
file(GLOB_RECURSE CURRENT_SOUIS  *.xml *.idx *.ico)
file(GLOB_RECURSE CURRENT_RC *.rc *.rc2)

set(CURRENT_HEADERS ${CURRENT_HEADERS}
	${SOUI_ROOT}/controls.extend/SChromeTabCtrl.h
	${SOUI_ROOT}/controls.extend/SImageMaskWnd.h
	${SOUI_ROOT}/controls.extend/STurn3DView.h
	${SOUI_ROOT}/controls.extend/image3d/3dlib.h
	${SOUI_ROOT}/controls.extend/image3d/3dmatrix.h
	${SOUI_ROOT}/controls.extend/image3d/3dTransform.h
	${SOUI_ROOT}/controls.extend/FileHelper.h
	${SOUI_ROOT}/controls.extend/slistboxex.h
)
set(CURRENT_SRCS ${CURRENT_SRCS}
	${SOUI_ROOT}/controls.extend/SChromeTabCtrl.cpp
	${SOUI_ROOT}/controls.extend/SImageMaskWnd.cpp
	${SOUI_ROOT}/controls.extend/STurn3DView.cpp
	${SOUI_ROOT}/controls.extend/image3d/3dlib.cpp
	${SOUI_ROOT}/controls.extend/image3d/3dmatrix.cpp
	${SOUI_ROOT}/controls.extend/image3d/3dTransform.cpp
	${SOUI_ROOT}/controls.extend/slistboxex.cpp
)

source_group("Header Files" FILES ${CURRENT_HEADERS})
source_group("Source Files" FILES ${CURRENT_SRCS})
source_group("SoUI Resouece" FILES ${CURRENT_SOUIS})
source_group("Resource Files" FILES ${CURRENT_RC})

link_directories(${SOUI_ROOT}/bin)
add_executable(QQLogin WIN32 ${CURRENT_HEADERS} ${CURRENT_SRCS} ${CURRENT_SOUIS} ${CURRENT_RC})

#add_dependencies(QQLogin soui-sys-resource)
#target_link_libraries(QQLogin utilities soui ${COM_LIBS} imm32)
target_link_libraries(QQLogin
        debug souid optimized soui
        debug utilitiesd optimized utilities
        imm32
)

set_target_properties(QQLogin PROPERTIES DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX})
set_target_properties(QQLogin PROPERTIES COTIRE_CXX_PREFIX_HEADER_INIT "stdafx.h")

set_target_properties (QQLogin PROPERTIES
    FOLDER demos
)
cotire(QQLogin)
