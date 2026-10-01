cmake_minimum_required(VERSION 4.0)
project(glad
	VERSION         "2.0"
	DESCRIPTION     "GL/GLES Loader-Generator based on the official specs"
	HOMEPAGE_URL    "https://glad.dav1d.de/"
	LANGUAGES       "C"
)

include(GNUInstallDirs)
set(GLAD_LIB_NAME glad)

add_library(glad
	src/glad.c
)

target_include_directories(glad PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>
	$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

target_compile_options(glad
	PRIVATE -fPIC
)

install(
	DIRECTORY
		include/glad
		include/KHR
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/glad.pc.in ${CMAKE_CURRENT_BINARY_DIR}/glad.pc @ONLY)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/glad.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)

install(
	TARGETS glad
	EXPORT gladConfig
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(
	EXPORT gladConfig
	NAMESPACE glad::
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/glad
)
