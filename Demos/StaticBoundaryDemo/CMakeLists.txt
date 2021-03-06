set(SIMULATION_LINK_LIBRARIES AntTweakBar glew SPlisHSPlasH Utilities partio zlib MD5)
set(SIMULATION_DEPENDENCIES AntTweakBar glew SPlisHSPlasH Utilities partio zlib MD5)

if(WIN32)
  set(SIMULATION_LINK_LIBRARIES freeglut opengl32.lib glu32.lib ${SIMULATION_LINK_LIBRARIES})
  set(SIMULATION_DEPENDENCIES freeglut ${SIMULATION_DEPENDENCIES})
else()
  find_package(GLUT REQUIRED)
  find_package(OpenGL REQUIRED)

  set(SIMULATION_LINK_LIBRARIES
	${SIMULATION_LINK_LIBRARIES}
	${GLUT_LIBRARIES}
	${OPENGL_LIBRARIES}
  )
endif()

find_package( Eigen3 REQUIRED )
include_directories( ${EIGEN3_INCLUDE_DIR} )

############################################################
# CompactNSearch
############################################################
include_directories(${PROJECT_PATH}/extern/install/CompactNSearch/include)
set(SIMULATION_DEPENDENCIES ${SIMULATION_DEPENDENCIES} ExternalProject_CompactNSearch)
set(SIMULATION_LINK_LIBRARIES ${SIMULATION_LINK_LIBRARIES} 
	optimized CompactNSearch 
	debug CompactNSearch_d)
link_directories(${PROJECT_PATH}/extern/install/CompactNSearch/lib)

add_executable(StaticBoundaryDemo
	main.cpp
	
	${PROJECT_PATH}/Demos/Common/DemoBase.cpp
	${PROJECT_PATH}/Demos/Common/DemoBase.h
  
	${VIS_FILES}          
  
	CMakeLists.txt
)

add_definitions(-DTW_NO_LIB_PRAGMA -DTW_STATIC)

include_directories(${PROJECT_PATH}/extern/freeglut/include)
include_directories(${PROJECT_PATH}/extern/glew/include)

set_target_properties(StaticBoundaryDemo PROPERTIES DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX})
set_target_properties(StaticBoundaryDemo PROPERTIES RELWITHDEBINFO_POSTFIX ${CMAKE_RELWITHDEBINFO_POSTFIX})
set_target_properties(StaticBoundaryDemo PROPERTIES MINSIZEREL_POSTFIX ${CMAKE_MINSIZEREL_POSTFIX})
add_dependencies(StaticBoundaryDemo ${SIMULATION_DEPENDENCIES})
target_link_libraries(StaticBoundaryDemo ${SIMULATION_LINK_LIBRARIES})
VIS_SOURCE_GROUPS()

set_target_properties(StaticBoundaryDemo PROPERTIES FOLDER "Demos")
