set(sources
  CAN_RUSOKU.cpp
  CAN_RUSOKU_stdafx.cpp)

set(headers
  CAN_RUSOKU.h
  CAN_RUSOKU_Extern.h
  CAN_RUSOKU_stdafx.h
  resource.h
  EXTERNAL/canal.h)

set(resources
  CAN_RUSOKU.rc)

add_library(CAN_RUSOKU SHARED ${sources} ${headers} ${resources})

include_directories(
  ..
  ${ICONV_INCLUDE_DIR}
  ${LIBXML2_INCLUDE_DIR}
  ${ATL_INCLUDE_DIRS}
  ${MFC_INCLUDE_DIRS})

# compiler options
add_definitions(-D_USRDLL)

# linker options
set_target_properties(CAN_RUSOKU PROPERTIES LINK_FLAGS "/NODEFAULTLIB:daouuid")
target_link_libraries(CAN_RUSOKU
  ${CMAKE_CURRENT_SOURCE_DIR}/EXTERNAL/canal.lib
  DataTypes
  Utils)

# installer options
add_custom_command(
  TARGET CAN_RUSOKU
  POST_BUILD
  COMMAND ${CMAKE_COMMAND} ARGS -E make_directory ${PROJECT_SOURCE_DIR}/BIN/${CMAKE_BUILD_TYPE}/
  COMMAND ${CMAKE_COMMAND} ARGS -E copy $<TARGET_FILE:CAN_RUSOKU> ${PROJECT_SOURCE_DIR}/BIN/${CMAKE_BUILD_TYPE}/
  COMMAND ${CMAKE_COMMAND} ARGS -E copy ${CMAKE_CURRENT_SOURCE_DIR}/EXTERNAL/vs_can_api.dll ${PROJECT_SOURCE_DIR}/BIN/${CMAKE_BUILD_TYPE}/)
