set(VIEWER_LINK_LIBRARIES AntTweakBar glew SPlisHSPlasH Utilities partio zlib MD5)
set(VIEWER_DEPENDENCIES AntTweakBar glew SPlisHSPlasH Utilities partio zlib MD5)

if(WIN32)
  set(VIEWER_LINK_LIBRARIES freeglut opengl32.lib glu32.lib ${VIEWER_LINK_LIBRARIES})
  set(VIEWER_DEPENDENCIES freeglut ${VIEWER_DEPENDENCIES})
else()
  find_package(GLUT REQUIRED)
  find_package(OpenGL REQUIRED)

  set(VIEWER_LINK_LIBRARIES
    ${VIEWER_LINK_LIBRARIES}
    ${GLUT_LIBRARIES}
    ${OPENGL_LIBRARIES}
  )
endif()

find_package( Eigen3 REQUIRED )
include_directories( ${EIGEN3_INCLUDE_DIR} )


add_executable(PartioViewer
	main.cpp
	
	${VIS_FILES}

	CMakeLists.txt
)

add_definitions(-DTW_NO_LIB_PRAGMA -DTW_STATIC)

include_directories(${PROJECT_PATH}/extern/freeglut/include)
include_directories(${PROJECT_PATH}/extern/glew/include)


set_target_properties(PartioViewer PROPERTIES DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX})
set_target_properties(PartioViewer PROPERTIES RELWITHDEBINFO_POSTFIX ${CMAKE_RELWITHDEBINFO_POSTFIX})
set_target_properties(PartioViewer PROPERTIES MINSIZEREL_POSTFIX ${CMAKE_MINSIZEREL_POSTFIX})

add_dependencies(PartioViewer ${VIEWER_DEPENDENCIES})
target_link_libraries(PartioViewer ${VIEWER_LINK_LIBRARIES})
VIS_SOURCE_GROUPS()

set_target_properties(PartioViewer PROPERTIES FOLDER "Tools")
