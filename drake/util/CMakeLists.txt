add_library(drakeUtil OBJECT drakeUtil.cpp)
target_include_directories(drakeUtil PRIVATE ${EIGEN3_INCLUDE_DIRS})

drake_install_headers(drakeUtil.h)

if(lcm_FOUND)
  # note: requires drakeUtil header file, but not linking against it because
  # we only need methods that are templated and defined in the header

  add_library_with_exports(LIB_NAME drakeLCMUtil SOURCE_FILES lcmUtil.cpp)
  target_link_libraries(drakeLCMUtil
    drakeLCMTypes
    drakeTrajectories
    Eigen3::Eigen
    lcmtypes_bot2-core-cpp)
  drake_install_headers(lcmUtil.h)
  drake_install_libraries(drakeLCMUtil)
  drake_install_pkg_config_file(drake-lcm-util
    TARGET drakeLCMUtil
    LIBS
      -ldrakeLCMUtil
    REQUIRES
      # TODO(jamiesnape): Upstream a lcmtypes_bot2-core-cpp.pc file
      drake-lcmtypes-cpp
      drake-trajectories
      eigen3)
endif()

add_library_with_exports(LIB_NAME drakeGeometryUtil SOURCE_FILES drakeGeometryUtil.cpp)
target_link_libraries(drakeGeometryUtil
  drakeCommon
  drakeMath
  Eigen3::Eigen)
drake_install_libraries(drakeGeometryUtil)
drake_install_headers(drakeGeometryUtil.h)
drake_install_pkg_config_file(drake-geometry-util
  TARGET drakeGeometryUtil
  LIBS -ldrakeGeometryUtil
  REQUIRES
    drake-common
    drake-math
    eigen3)

add_library_with_exports(LIB_NAME drakeConvexHull SOURCE_FILES convexHull.cpp)
target_link_libraries(drakeConvexHull Eigen3::Eigen)
drake_install_libraries(drakeConvexHull)
drake_install_headers(convexHull.h)
drake_install_pkg_config_file(drake-convex-hull
  TARGET drakeConvexHull
  LIBS -ldrakeConvexHull
  REQUIRES eigen3)

add_subdirectory(yaml)

if(BUILD_TESTING)
  add_subdirectory(test)
endif()
