INCLUDE_DIRECTORIES("${CMAKE_SOURCE_DIR}/include")
FILE(GLOB_RECURSE SOURCES cmt/src/*.cpp)
LIST(SORT SOURCES)
ADD_LIBRARY(cmt MODULE ${SOURCES})
INSTALL(TARGETS cmt LIBRARY DESTINATION "${PLUGIN_DIR}/ladspa")

SET_TARGET_PROPERTIES(cmt PROPERTIES PREFIX "")
SET_TARGET_PROPERTIES(cmt PROPERTIES COMPILE_FLAGS "-Wall -O3 -fno-strict-aliasing")

IF(LMMS_BUILD_WIN32 AND NOT CMAKE_BUILD_TYPE MATCHES "Deb")
	ADD_CUSTOM_COMMAND(TARGET cmt POST_BUILD COMMAND "${STRIP}" "$<TARGET_FILE:cmt>")
ELSE()
	SET_TARGET_PROPERTIES(cmt PROPERTIES COMPILE_FLAGS "${COMPILE_FLAGS} -fPIC")
ENDIF()

IF(NOT LMMS_BUILD_APPLE AND NOT LMMS_BUILD_OPENBSD)
	SET_TARGET_PROPERTIES(cmt PROPERTIES LINK_FLAGS "${LINK_FLAGS} -shared -Wl,-no-undefined")
ENDIF()

