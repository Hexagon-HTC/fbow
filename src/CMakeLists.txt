ADD_DEFINITIONS(-DNOMINMAX)

SET(  srcs  fbow.cpp  vocabulary_creator.cpp )
SET(  hdrs  fbow.h fbow_exports.h vocabulary_creator.h cpu.h)

SET (THIS_LIBNAME fbow)

ADD_LIBRARY(${THIS_LIBNAME} ${srcs} ${hdrs})

set_target_properties(fbow PROPERTIES DEFINE_SYMBOL FBOW_DSO_EXPORTS
    LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
    RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
    ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin
    ARCHIVE_OUTPUT_NAME fbow
    RUNTIME_OUTPUT_NAME fbow
    LIBRARY_OUTPUT_NAME fbow
)
target_include_directories(fbow PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)

TARGET_LINK_LIBRARIES(${THIS_LIBNAME} PRIVATE ${OpenCV_LIBS} OpenMP::OpenMP_CXX)

install(TARGETS ${THIS_LIBNAME} EXPORT ${CMAKE_PROJECT_NAME}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_BINDIR}
    )

install(EXPORT ${CMAKE_PROJECT_NAME}
       FILE ${THIS_LIBNAME}Config.cmake
       NAMESPACE ${CMAKE_PROJECT_NAME}::
       DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${CMAKE_PROJECT_NAME}
)

IF(FBOW_DEVINSTALL)
    INSTALL(FILES ${hdrs}
            DESTINATION include/fbow
            COMPONENT main)
ENDIF()


#set(sources
#    fbow.cpp
#    vocabulary_creator.cpp
#)

#set(headers
#    fbow.h
#   fbow_exports.h
#    vocabulary_creator.h
#)

#add_library(fbow ${sources} ${headers})

#set_target_properties(fbow PROPERTIES          # create *nix style library versions + symbolic links
#    DEFINE_SYMBOL FBOW_DSO_EXPORTS
#    VERSION ${PROJECT_VERSION}
#    SOVERSION ${PROJECT_SOVERSION}
#    CXX_VISIBILITY_PRESET hidden                # Make all the symbols hidden, so we have to explicitly define an API
#    CLEAN_DIRECT_OUTPUT 1                       # allow creating static and shared libs without conflicts
#    OUTPUT_NAME "fbow${PROJECT_DLLVERSION}"    # avoid conflicts between library and binary target names
#)

#include(GenerateExportHeader)
#generate_export_header(fbow)

#target_link_libraries(fbow PUBLIC opencv_core)
#target_link_libraries(fbow PRIVATE opencv_features2d)

#target_include_directories(fbow PUBLIC
#  $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
#  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
#  $<INSTALL_INTERFACE:include>
#)

#install(TARGETS fbow EXPORT fbowConfig
#    RUNTIME DESTINATION bin
#    LIBRARY DESTINATION lib PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
#    ARCHIVE DESTINATION lib
#)


#install(EXPORT arucoConfig DESTINATION "share/aruco/cmake")
