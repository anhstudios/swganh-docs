cmake_minimum_required(VERSION 2.8)

project(swganh-docs NONE)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

find_package(Sphinx REQUIRED)

if(NOT DEFINED SPHINX_THEME)
	set(SPHINX_THEME default)
endif()

if(NOT DEFINED SPHINX_THEME_DIR)
	set(SPHINX_THEME_DIR)
endif()
	
# configured documentation tools and intermediate build results
set(BINARY_BUILD_DIR "${CMAKE_CURRENT_BINARY_DIR}/_build")
	
# Sphinx cache with pickled ReST documents
set(SPHINX_CACHE_DIR "${CMAKE_CURRENT_BINARY_DIR}/_doctrees")

# HTML output directory
set(SPHINX_HTML_DIR "${CMAKE_CURRENT_BINARY_DIR}/html")
	
foreach(file conf.py)
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/${file}.in"
		"${BINARY_BUILD_DIR}/${file}"
		@ONLY
	)
endforeach()

add_custom_target(swganh_doc_html ALL
    ${SPHINX_EXECUTABLE}
	-q -b html
	-c "${BINARY_BUILD_DIR}"
	-d "${SPHINX_CACHE_DIR}"
	"${CMAKE_CURRENT_SOURCE_DIR}"
	"${SPHINX_HTML_DIR}"
	COMMENT "Building HTML documentation with Sphinx"
)
