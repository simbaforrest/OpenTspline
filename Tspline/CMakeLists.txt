set(TSPLINE 1 CACHE BOOL "T-spline implementation" )
IF(TSPLINE)

# CGAL "Computational Geometry Algorithms Library" dependent programs
find_package(CGAL QUIET COMPONENTS Core )

if ( CGAL_FOUND )

  
	
	# find dependent packages
if(WIN32 AND NOT CYGWIN)
  set(CMAKE_CXX_FLAGS "/EHsc")
  add_definitions(-D_USE_MATH_DEFINES)
  set(EIGEN3_INCLUDE_DIRS "C:/Eigen/" CACHE PATH "Include directory for Eigen3 library.")
else()
  set(CMAKE_CXX_FLAGS "-frounding-math /EHsc")
  find_package(PkgConfig REQUIRED)
  pkg_search_module(EIGEN3 REQUIRED eigen3)
endif()
  include_directories(${EIGEN3_INCLUDE_DIRS})


	include( ${CGAL_USE_FILE} )
	
	set(SOURCE_CPP
                Cox.cpp
                Domain.cpp
                File.cpp
                Polyhedron.cpp
                Transition.cpp
                Tspline.cpp
                TsplineCreator.cpp
                Tspline_operators.cpp
                TsplineMultiPatch.cpp
                TsplinePatch.cpp
                Types.cpp)

	set(SOURCE_H
                Cox.h
                Domain.h
                File.h
                Given_border_parameterizer_3.h
                Math.hpp
                Polyhedron.h
                Transition.h
                Tspline.h
                TsplineCreator.h
                TsplineMultiPatch.h
                TsplinePatch.h
                Types.h)

  set(SOURCE_H_EXT Utils.hpp)

  add_library(tspline STATIC ${SOURCE_CPP} ${SOURCE_H})
  target_link_libraries(tspline ${CGAL_LIBRARIES})

  install(FILES ${SOURCE_H} ${SOURCE_H_EXT} DESTINATION "${INSTALL_INCLUDE_DIR}/Tspline")
  message(STATUS "${INSTALL_LIB_DIR}")
  install(TARGETS tspline
    EXPORT OpenTsplineTargets
    RUNTIME DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin
    LIBRARY DESTINATION "${INSTALL_LIB_DIR}" COMPONENT lib
    ARCHIVE DESTINATION "${INSTALL_BIN_DIR}" COMPONENT bin
    PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/Tspline"
      COMPONENT dev)

  


else(CGAL_FOUND)
    message(STATUS "TSPLINE: class Tspline requires the CGAL library, and will not be compiled.")
endif(CGAL_FOUND)


ENDIF(TSPLINE)
