
cmake_minimum_required(VERSION 2.8)

PROJECT(FiberFeaturesCreator)

#Find ITK Package
find_package(ITK REQUIRED)
include(${ITK_USE_FILE})

#Find Slicer Execution Model Package
find_package(SlicerExecutionModel REQUIRED)
include(${SlicerExecutionModel_USE_FILE})

#Find VTK Package
find_package(VTK COMPONENTS vtkCommonCore vtkFiltersCore vtkIOGeometry vtkIOXML REQUIRED)
include(${VTK_USE_FILE})


file(
GLOB
source_files
*.[ch]pp
*.[ch]xx
*.[ch]
)

SEMMacroBuildCLI(
  NAME fiberfeaturescreator
  ADDITIONAL_SRCS ${source_files}
  EXECUTABLE_ONLY
  TARGET_LIBRARIES ${ITK_LIBRARIES} ${VTK_LIBRARIES}
  RUNTIME_OUTPUT_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}
  LIBRARY_OUTPUT_DIRECTORY ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}
  ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}
  INSTALL_RUNTIME_DESTINATION ${INSTALL_RUNTIME_DESTINATION}
  INSTALL_LIBRARY_DESTINATION ${INSTALL_LIBRARY_DESTINATION}
  INSTALL_ARCHIVE_DESTINATION ${INSTALL_ARCHIVE_DESTINATION}
  )


file(GLOB LIBRARY_HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/*.txx ${CMAKE_CURRENT_SOURCE_DIR}/*.hxx ${CMAKE_CURRENT_SOURCE_DIR}/*.h)

set_target_properties(fiberfeaturescreator PROPERTIES
  PUBLIC_HEADER "${LIBRARY_HEADERS}"
  )

install( TARGETS fiberfeaturescreator 
  EXPORT TraficTargets
  RUNTIME DESTINATION ${INSTALL_RUNTIME_DESTINATION} 
  PUBLIC_HEADER DESTINATION include
  )
# Add all targets to the build-tree export set

set_property(GLOBAL APPEND PROPERTY Trafic_LIBRARIES fiberfeaturescreator)