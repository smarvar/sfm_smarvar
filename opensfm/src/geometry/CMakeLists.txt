# Geometry
set(GEOMETRY_FILES
    essential.h
    absolute_pose.h
    pose.h
    camera.h
    camera_functions.h
    relative_pose.h
    triangulation.h
    src/camera.cc
    src/essential.cc
    src/triangulation.cc
    src/absolute_pose.cc
    src/relative_pose.cc
)
add_library(geometry ${GEOMETRY_FILES})
target_link_libraries(geometry
  PRIVATE
    foundation
)
target_include_directories(geometry PUBLIC ${CMAKE_SOURCE_DIR} ${CERES_INCLUDE_DIR})

set(GEOMETRY_TEST_FILES
    test/camera_test.cc
)
add_executable(geometry_test ${GEOMETRY_TEST_FILES})
target_include_directories(geometry_test PRIVATE ${CMAKE_SOURCE_DIR} ${EIGEN_INCLUDE_DIRS})
target_link_libraries(geometry_test
                      PUBLIC
                      geometry
                      ${TEST_MAIN})
add_test(geometry_test geometry_test)

pybind11_add_module(pygeometry python/pybind.cc)
target_link_libraries(pygeometry
  PRIVATE
    geometry
    foundation
    pybind11
)
set_target_properties(pygeometry PROPERTIES
    LIBRARY_OUTPUT_DIRECTORY "${opensfm_SOURCE_DIR}/.."
)
